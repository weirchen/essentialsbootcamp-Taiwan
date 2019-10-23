.. _calm_linux:

---------------------
Calm: Linux 工作負載
---------------------

*完成本實作的估計時間為60分鐘。*

簡介
++++++++

Nutanix Calm允許您跨私有雲和公共雲基礎架構無縫選擇、供應和管理業務應用程式。 Nutanix Calm提供應用程式生命週期，監視和修復，以管理您的異構基礎架構，例如VM或裸機伺服器。 Nutanix Calm支援多個平臺，因此您可以使用單個自助服務和自動化介面來管理所有基礎架構。

**在本實作中，您將通過構建和部署藍圖來探索Nutanix Calm的基礎知識，該藍圖使用MySQL，nginix和HAProxy安裝和配置多層Task Manager Web應用程式。**

創建藍圖
++++++++++++++++++++

藍圖是使用Nutanix Calm建模的每個應用程式的框架。 藍圖是範本，描述了在已創建的服務和應用程式上置備，配置和執行任務所需的所有步驟。 您可以創建一個代表您的應用程式體系結構的藍圖，然後重複運行該藍圖以創建實例，配置和啟動您的應用程式。 藍圖還定義了應用程式及其底層基礎結構的生命週期，從創建應用程式到對藍圖執行的操作直到應用程式終止為止。

您可以使用藍圖對各種複雜的應用程式進行建模。 從簡單地配置單個虛擬機器到配置和管理多節點，多層應用程式。

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Services > Calm**.

   .. figure:: images/1.png

#. 在左手工具列選擇 |blueprints| **Blueprints** 查看和管理 Calm 藍圖。

   .. note::

     將滑鼠懸停在圖示上將顯示其標題。

#. 按一下 **+ Create Blueprint > Multi VM/Pod Blueprint**.

#. 填寫以下欄位：

   - **Name** - *Initials*-CalmLinuxIntro
   - **Description** - [Task Manager Application](\http://@@{HAProxy.address}@@/)
   - **Project** - *Initials*-Calm

   .. figure:: images/2.png

#. 按一下 **Proceed** 打開藍圖編輯器

   藍圖編輯器提供了各種元件的圖形表示，使您可以在環境中視覺化和配置元件及其相依性性。

創建憑證
++++++++++++++++++++

首先，您將創建一個證書，該證書將用於對最終將部署的CentOS VM進行Calm身份驗證。 憑證對於每個藍圖都是唯一的，出於安全目的， **不會** 將其作為藍圖的一部分匯出。 每個藍圖至少需要1個憑證。

本練習使用“通用雲” CentOS映射。 這是多個流行的Linux發行版本的通用選項，這些發行版本輕巧，支援基於Cloud-Init的配置並利用 `SSH keypair authentication <https://www.ssh.com/ssh/public-key-authentication>`_ 而不是密碼。 基於金鑰對的身份驗證在所有公共雲環境中都很普遍。

#. 按一下 **Credentials**.

   .. figure:: images/3.png

#. 按一下 **Credentials** :fa:`plus-circle` 並填寫以下欄位：

   - **Credential Name** - CENTOS
   - **Username** - centos
   - **Secret Type** - SSH Private Key
   - **Key** - Paste in your own private key, or use:

   ::

     -----BEGIN RSA PRIVATE KEY-----
     MIIEowIBAAKCAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNG
     ZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK
     6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9
     HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7Gy
     hCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNR
     uz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5OrwIBJQKCAQB++q2WCkCmbtByyrAp
     6ktiukjTL6MGGGhjX/PgYA5IvINX1SvtU0NZnb7FAntiSz7GFrODQyFPQ0jL3bq0
     MrwzRDA6x+cPzMb/7RvBEIGdadfFjbAVaMqfAsul5SpBokKFLxU6lDb2CMdhS67c
     1K2Hv0qKLpHL0vAdEZQ2nFAMWETvVMzl0o1dQmyGzA0GTY8VYdCRsUbwNgvFMvBj
     8T/svzjpASDifa7IXlGaLrXfCH584zt7y+qjJ05O1G0NFslQ9n2wi7F93N8rHxgl
     JDE4OhfyaDyLL1UdBlBpjYPSUbX7D5NExLggWEVFEwx4JRaK6+aDdFDKbSBIidHf
     h45NAoGBANjANRKLBtcxmW4foK5ILTuFkOaowqj+2AIgT1ezCVpErHDFg0bkuvDk
     QVdsAJRX5//luSO30dI0OWWGjgmIUXD7iej0sjAPJjRAv8ai+MYyaLfkdqv1Oj5c
     oDC3KjmSdXTuWSYNvarsW+Uf2v7zlZlWesTnpV6gkZH3tX86iuiZAoGBAKM0mKX0
     EjFkJH65Ym7gIED2CUyuFqq4WsCUD2RakpYZyIBKZGr8MRni3I4z6Hqm+rxVW6Dj
     uFGQe5GhgPvO23UG1Y6nm0VkYgZq81TraZc/oMzignSC95w7OsLaLn6qp32Fje1M
     Ez2Yn0T3dDcu1twY8OoDuvWx5LFMJ3NoRJaHAoGBAJ4rZP+xj17DVElxBo0EPK7k
     7TKygDYhwDjnJSRSN0HfFg0agmQqXucjGuzEbyAkeN1Um9vLU+xrTHqEyIN/Jqxk
     hztKxzfTtBhK7M84p7M5iq+0jfMau8ykdOVHZAB/odHeXLrnbrr/gVQsAKw1NdDC
     kPCNXP/c9JrzB+c4juEVAoGBAJGPxmp/vTL4c5OebIxnCAKWP6VBUnyWliFhdYME
     rECvNkjoZ2ZWjKhijVw8Il+OAjlFNgwJXzP9Z0qJIAMuHa2QeUfhmFKlo4ku9LOF
     2rdUbNJpKD5m+IRsLX1az4W6zLwPVRHp56WjzFJEfGiRjzMBfOxkMSBSjbLjDm3Z
     iUf7AoGBALjvtjapDwlEa5/CFvzOVGFq4L/OJTBEBGx/SA4HUc3TFTtlY2hvTDPZ
     dQr/JBzLBUjCOBVuUuH3uW7hGhW+DnlzrfbfJATaRR8Ht6VU651T+Gbrr8EqNpCP
     gmznERCNf9Kaxl/hlyV5dZBe/2LIK+/jLGNu9EJLoraaCBFshJKF
     -----END RSA PRIVATE KEY-----

   .. figure:: images/4.png

#. 按一下 **Save**, 然後 **Back**.

定義變數
++++++++++++++++++

變數允許藍圖的可擴展性，這意味著單個藍圖可以根據其變數的配置用於多種用途和環境。
變數可以是保存為藍圖一部分的靜態值，也可以在**Runtime** （啟動藍圖）時指定。變數特定於給定的**Application Profile**，這是將在其上部署藍圖的平臺。例如，能夠同時部署到AHV和AWS的藍圖將具有2個應用程式設定檔。每個設定檔可以具有單獨的變數和VM配置。

預設情況下，變數儲存為 ** String **，並且在“配置”窗格中可見。將變數設置為 **Secret**將掩蓋該值，並且非常適合諸如密碼之類的變數。除了String和Secret選項外，還有Integer，Multi-line String，Date, Time, and Date Time **Data Types** 和更高級的 **“輸入類型” **，但是這些內容不在此範圍之內。實作室。

可以在使用 ** @@ {variable_name} @@ **結構針對物件執行的腳本中使用變數。 Calm將展開並使用適當的值替換該變數，然後再發送到VM。

#. 在Blueprint Editor右邊的 **Configuration Pane** ，在 **Variables**下面, 添加下面變數 ( **Runtime** 通過切換 **Running Man** 標識到藍色來指定):

   +------------------------+-------------------------------+------------+-------------+
   | **Variable Name**      | **Data Type** | **Value**     | **Secret** | **Runtime** |
   +------------------------+-------------------------------+------------+-------------+
   | User_initials          | String        | xyz           |            |      X      |
   +------------------------+-------------------------------+------------+-------------+
   | Mysql\_user            | String        | root          |            |             |
   +------------------------+-------------------------------+------------+-------------+
   | Mysql\_password        | String        | nutanix/4u    |     X      |             |
   +------------------------+-------------------------------+------------+-------------+
   | Database\_name         | String        | homestead     |            |             |
   +------------------------+-------------------------------+------------+-------------+

   .. figure:: images/5.png

#. 按一下 **Save**.

添加可下載的映像檔
+++++++++++++++++++++++++++

可以基於映像檔部署AHV中的VM。 使用Calm，您可以通過URI選擇可下載映像檔。 在應用程式部署期間，Prism Central將自動下載並創建指定的映像檔。 如果群集上已經存在具有相同URI的映像檔，它將跳過下載並改用本地映像檔。

#. 在頂部工具列中，按一下 **Configuration > Downloadable Image Configuration** :fa:`plus-circle` 並填寫以下欄位：

   - **Package Name** - CentOS_7_Cloud
   - **Description** - CentOS 7 Cloud Image
   - **Image Name** - CentOS_7_Cloud
   - **Image Type** - Disk Image
   - **Architecture** - X86_64
   - **Source URI** - http://download.nutanix.com/calm/CentOS-7-x86_64-GenericCloud.qcow2
   - **Product Name** - CentOS
   - **Product Version** - 7

   .. note::

      此通用雲映像檔（Generic Cloud image）與大多數Nutanix預播應用程式藍圖使用的映像檔相同。

   .. figure:: images/6.png

#. 按一下 **Save**, 之後 **Back**.

創建Service
+++++++++++++++++

Services 是虛擬機器實例，現有電腦或裸機，您可以使用Nutanix Calm進行配置和配置。

在本練習中，您將創建組成應用程式的資料庫，Web伺服器和負載平衡器服務。

創建資料庫服務
.............................

#. 在 **Application Overview > Services**, 按一下 :fa:`plus-circle` 增加新的 Service.

   預設情況下，“應用程式概述”位於藍圖編輯器的右下角，用於創建和管理藍圖層，例如服務，應用程式設定檔和操作。

   .. figure:: images/7.png

   注意 **Service1** 出現在 **Workspace** 並且 **Configuration Pane** 反映所選服務的配置。

#. 填寫以下欄位：

   - **Service Name** - MySQL
   - **Name** - MySQLAHV

   .. note::
      這定義了Calm中基底的名稱。 名稱只能包含字母數位字元，空格和底線。

   - **Cloud** - Nutanix
   - **OS** - Linux
   - **VM Name** - @@{User_initials}@@-MYSQL-@@{calm_array_index}@@-@@{calm_time}@@

   .. note::

     Runtime是將使用您先前提供的變數 **User_initials** ，用於在VM名稱前加上首字母縮寫。 它還將使用內置巨集來提供陣列索引（用於橫向擴展服務）和時間戳記。

   - **Image** - CentOS_7_Cloud
   - **Device Type** - Disk
   - **Device Bus** - SCSI
   - Select **Bootable**
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory (GiB)** - 4
   - Select **Guest Customization**

     - **Type** - Cloud-init
     - **Script** -

       .. code-block:: bash

         #cloud-config
         users:
           - name: centos
             ssh-authorized-keys:
               - @@{CENTOS.public_key}@@
             sudo: ['ALL=(ALL) NOPASSWD:ALL']

       .. note::

         使用SSH私密金鑰憑據時，Calm可以將該私密金鑰解碼為匹配的公開金鑰，並通過@@ {Credential_Name.public_key} @@巨集存取已解碼的值。 然後利用Cloud-Init將SSH公開金鑰值填充為授權金鑰，從而允許使用相應的私密金鑰向主機進行身份驗證。

   - 選擇 **Network Adapters (NICs)**下面的 :fa:`plus-circle` 
   - **NIC 1** - Primary
   - **Credential** - CENTOS

#. 按一下 **Save**.

   .. note::

    如果在保存藍圖後出現錯誤或警告，請將滑鼠懸停在頂部工具列中的圖示上，以查看問題列表。 解決所有問題，然後再次**保存**藍圖。

     .. figure:: images/8.png

   現在，您已經完成了與服務關聯的VM的部署詳細資訊，下一步是告訴Calm如何在VM上安裝應用程式。

#. 在Workspace窗格中選擇**MySQL**服務圖示後，滾動到**Configuration Panel**的頂部，然後選擇**Package**選項卡。

    套裝軟體是在服務上安裝的配置和應用程式，通常是通過在服務VM上執行腳本來完成的。

#. 填寫 **MySQL_PACKAGE** 作為 **Package Name** 並點擊 **Configure install**.

   - **Package Name** - MYSQL_PACKAGE

   .. figure:: images/9.png

   請注意，在Workspace窗格MySQL服務上出現的**Package install**欄位。

#. 選擇 **+ Task**, 並填寫以下欄位 **Configuration Panel** 以定義Calm將在MySQL Service VM上遠端執行的腳本：

   - **Task Name** - Install_sql
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo yum install -y "http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm"
       sudo yum update -y
       sudo setenforce 0
       sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
       sudo systemctl stop firewalld || true
       sudo systemctl disable firewalld || true
       sudo yum install -y mysql-community-server.x86_64

       sudo /bin/systemctl start mysqld
       sudo /bin/systemctl enable mysqld

       #Mysql secure installation
       mysql -u root<<-EOF

       UPDATE mysql.user SET Password=PASSWORD('@@{Mysql_password}@@') WHERE User='@@{Mysql_user}@@';
       DELETE FROM mysql.user WHERE User='@@{Mysql_user}@@' AND Host NOT IN ('localhost', '127.0.0.1', '::1');
       DELETE FROM mysql.user WHERE User='';
       DELETE FROM mysql.db WHERE Db='test' OR Db='test\_%';

       FLUSH PRIVILEGES;
       EOF

       mysql -u @@{Mysql_user}@@ -p@@{Mysql_password}@@ <<-EOF
       CREATE DATABASE @@{Database_name}@@;
       GRANT ALL PRIVILEGES ON homestead.* TO '@@{Database_name}@@'@'%' identified by 'secret';

       FLUSH PRIVILEGES;
       EOF

   .. figure:: images/10.png

   .. note::
      您可以按一下腳本欄位上的** Pop Out **圖示以獲得更大的視窗，以查看/編輯腳本。

   查看腳本，您將看到該套裝軟體將安裝MySQL，配置憑據並根據練習中指定的變數創建資料庫。

#.  再次在“工作區”窗格中選擇 **MySQL**服務圖示，然後在 **Configuration Panel**中選擇**Package**選項卡。

#.  按一下 **Configure uninstall**.

#.  按一下 **+ Task**, 並填寫以下欄位 **Configuration Panel**:

   - **Task Name** - Uninstall_sql
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       echo "Goodbye!"

   .. figure:: images/11.png

   .. note::
      卸載腳本可用於刪除套裝程式，更新網路服務（如DHCP和DNS），從Active Directory中刪除條目等。此簡單示例未使用該腳本。

#. 按一下 **Save**. 如果存在驗證問題（例如缺少欄位或不可接受的字元），系統將提示您特定的錯誤。

創建Web伺服器服務
................................

現在，您將按照類似的步驟定義Web伺服器服務。

#. 在 **Application Overview > Services**, 添加其他服務。

#. 選擇新服務，然後在“**Configuration Panel**中填寫以下**VM** 欄位： 

   - **Service Name** - WebServer
   - **Name** - WebServerAHV
   - **Cloud** - Nutanix
   - **OS** - Linux
   - **VM Name** - @@{User_initials}@@-WebServer-@@{calm_array_index}@@
   - **Image** - CentOS_7_Cloud
   - **Device Type** - Disk
   - **Device Bus** - SCSI
   - 選擇 **Bootable**
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory (GiB)** - 4
   - 選擇 **Guest Customization**

     - **Type** - Cloud-init
     - **Script** -

       .. code-block:: bash

         #cloud-config
         users:
           - name: centos
             ssh-authorized-keys:
               - @@{CENTOS.public_key}@@
             sudo: ['ALL=(ALL) NOPASSWD:ALL']

   - 選擇**Network Adapters (NICs)**下面的 :fa:`plus-circle`  
   - **NIC 1** - Primary
   - **Credential** - CENTOS

#. 選擇 **Package** 選項卡。

#. 填寫 **Package Name** 並按一下 **Configure install**.

   - **Package Name** - WebServer_PACKAGE

#. 選擇 **+ Task**, 並填寫以下欄位 **Configuration Panel**:

   - **Name Task** - Install_WebServer
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo yum update -y
       sudo yum -y install epel-release
       sudo setenforce 0
       sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
       sudo systemctl stop firewalld || true
       sudo systemctl disable firewalld || true
       sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
       sudo yum update -y
       sudo yum install -y nginx php56w-fpm php56w-cli php56w-mcrypt php56w-mysql php56w-mbstring php56w-dom git unzip

       sudo mkdir -p /var/www/laravel
       echo "server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;
       root /var/www/laravel/public/;
        index index.php index.html index.htm;
       location / {
        try_files \$uri \$uri/ /index.php?\$query_string;
        }
        # pass the PHP scripts to FastCGI server listening on /var/run/php5-fpm.sock
        location ~ \.php$ {
        try_files \$uri /index.php =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)\$;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME \$document_root\$fastcgi_script_name;
        include fastcgi_params;
        }
       }" | sudo tee /etc/nginx/conf.d/laravel.conf
       sudo sed -i 's/80 default_server/80/g' /etc/nginx/nginx.conf
       if `grep "cgi.fix_pathinfo" /etc/php.ini` ; then
        sudo sed -i 's/cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php.ini
       else
        sudo sed -i 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/' /etc/php.ini
       fi

       sudo systemctl enable php-fpm
       sudo systemctl enable nginx
       sudo systemctl restart php-fpm
       sudo systemctl restart nginx

       if [ ! -e /usr/local/bin/composer ]
       then
        curl -sS https://getcomposer.org/installer | php
        sudo mv composer.phar /usr/local/bin/composer
        sudo chmod +x /usr/local/bin/composer
       fi

       sudo git clone https://github.com/ideadevice/quickstart-basic.git /var/www/laravel
       sudo sed -i 's/DB_HOST=.*/DB_HOST=@@{MySQL.address}@@/' /var/www/laravel/.env

       sudo su - -c "cd /var/www/laravel; composer install"
       if [ "@@{calm_array_index}@@" == "0" ]; then
        sudo su - -c "cd /var/www/laravel; php artisan migrate"
       fi

       sudo chown -R nginx:nginx /var/www/laravel
       sudo chmod -R 777 /var/www/laravel/
       sudo systemctl restart nginx

   此腳本將安裝PHP和Nginx來創建Web伺服器，然後創建基於Laravel的Web應用程式。
    然後，它配置Web應用程式設置，包括使用通過** @@ {MySQL.address} @@ **巨集存取的MySQL IP位址更新** DB_HOST **。

#. 選擇 **Package** 選項卡並按一下 **Configure uninstall**.

#. 選擇 **+ Task**, 在 **Configuration Panel**填寫以下欄位:

   - **Name Task** - Uninstall_WebServer
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo rm -rf /var/www/laravel
       sudo yum erase -y nginx

   對於許多應用程式，通常需要擴展給定的服務（例如Web層）以處理更多併發用戶。 借助Calm，可以輕鬆地部署包含給定服務的多個副本的陣列。

#. 在Workspace窗格中選擇 **WebServer**服務圖示後，滾動到 **Configuration Panel**的頂部，然後選擇 **Service**選項卡。

#. 在 **Deployment Config > Number of Replicas**, 增加 **Min** 的值從1到2 和 **Max** 的值從 1 到 4.

   .. figure:: images/12.png

   此項更改將為應用程式的每次部署至少提供2個WebServer VM，並使陣列最多可以增長到4個WebServer VM。

   .. note::

     伸縮應用程式將需要其他腳本，以便應用程式瞭解如何利用其他VM。

#. 點擊 **Save**.

.. _haproxyinstall:

創建負載均衡服務
..................................

為了利用橫向擴展Web層，您的應用程式需要能夠在多個Web伺服器VM之間平衡連接的負載。 HAProxy是一個免費的開源TCP / HTTP負載平衡器，用於在多個伺服器之間分配工作負載。 從小型，簡單的部署到大型Web規模的環境（例如GitHub，Instagram和Twitter），都可以使用它。

#. 在 **Application Overview > Services**, 添加另一個服務。

#. 選擇一個新服務並在 **Configuration Panel**填寫 **VM** 欄位:

   - **Service Name** - HAProxy
   - **Name** - HAProxyAHV
   - **Cloud** - Nutanix
   - **OS** - Linux
   - **VM Name** - @@{User_initials}@@-HAProxy-@@{calm_array_index}@@
   - **Image** - CentOS\_7\_Cloud
   - **Device Type** - Disk
   - **Device Bus** - SCSI
   - Select **Bootable**
   - **vCPUs** - 2
   - **Cores per vCPU** - 1
   - **Memory (GiB)** - 4
   - Select **Guest Customization**

     - **Type** - Cloud-init
     - **Script** -

       .. code-block:: bash

         #cloud-config
         users:
           - name: centos
             ssh-authorized-keys:
               - @@{CENTOS.public_key}@@
             sudo: ['ALL=(ALL) NOPASSWD:ALL']

   - 選擇 :fa:`plus-circle` under **Network Adapters (NICs)**
   - **NIC 1** - Primary
   - **Credential** - CENTOS

#. 選擇 **Package** 選項卡。

#. 填寫 **Package Name** 並選擇 **Configure install**.

   - **Package Name** - HAProxy_PACKAGE

#. 選擇 **+ Task**, 填寫以下欄位 **Configuration Panel**:

   - **Name Task** - Install_HAProxy
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo yum update -y
       sudo yum install -y haproxy
       sudo setenforce 0
       sudo sed -i 's/enforcing/disabled/g' /etc/selinux/config /etc/selinux/config
       sudo systemctl stop firewalld || true
       sudo systemctl disable firewalld || true

       echo "global
        log 127.0.0.1 local0
        log 127.0.0.1 local1 notice
        maxconn 4096
        quiet
        user haproxy
        group haproxy
       defaults
        log global
        mode http
        retries 3
        timeout client 50s
        timeout connect 5s
        timeout server 50s
        option dontlognull
        option httplog
        option redispatch
        balance roundrobin
       # Set up application listeners here.
       listen admin
        bind 127.0.0.1:22002
        mode http
        stats uri /
       frontend http
        maxconn 2000
        bind 0.0.0.0:80
        default_backend servers-http
       backend servers-http" | sudo tee /etc/haproxy/haproxy.cfg

       hosts=$(echo "@@{WebServer.address}@@" | tr "," "\n")
       port=80

       for host in $hosts
         do echo " server host-${host} ${host}:${port} weight 1 maxconn 100 check" | sudo tee -a /etc/haproxy/haproxy.cfg
       done

       sudo systemctl daemon-reload
       sudo systemctl enable haproxy
       sudo systemctl restart haproxy

   注意在以上腳本中 @@{WebServer.address}@@ 巨集的使用。 該巨集返回該服務內VM的所有IP的逗號分隔列表。 然後，腳本使用 `tr <https://www.geeksforgeeks.org/tr-command-unixlinux-examples/>`_ 命令用回車符替換逗號。 結果是一個陣列， **$hosts**, 包含所有WebServer IP位址的字串。 然後將這些位址分別添加到 **HAProxy** 設定檔。

#. 選擇 **Package** 選項卡並點擊 **Configure uninstall**.

#. 選擇 **+ Task**, 並填寫以下欄位 **Configuration Panel**:

   - **Name Task** - Uninstall_HAProxy
   - **Type** - Execute
   - **Script Type** - Shell
   - **Credential** - CENTOS
   - **Script** -

     .. code-block:: bash

       #!/bin/bash
       set -ex

       sudo
       yum -y erase haproxy

#. 點擊 **Save**.

添加相依性項
+++++++++++++++++++

由於我們的應用程式將需要資料庫在Web伺服器啟動之前運行，因此我們的藍圖需要相依性項來強制執行此排序。 有兩種方法可以完成此操作，其中一種您已經完成但沒有意識到。

#. 在 **Application Overview > Application Profile** 部分, 擴展 **Default** 應用程式設定檔，然後按一下 **Create** 動作。

   .. figure:: images/13.png

   注意** Orange Orchestration Edge **從** MySQL Start **任務轉到** WebServer Package Install **任務。 由於“ WebServer Package Install”任務中的** @@ {MySQL.address} @@ **巨集引用，Calm自動創建了此邊線。 由於系統在繼續執行WebServer安裝任務之前需要知道MySQL服務的IP位址，因此Calm會為您智慧地創建業務流程邊線。 這要求在繼續進行WebServer安裝任務之前啟動MySQL服務。

#. 返回 **HAProxy Package Install** 任務，為什麼在WebServer和HAProxy服務之間自動創建業務流程邊線？ 

#. 接下來，選擇 **Stop** 設定檔操作。

   請注意，停止應用程式時，服務之間的編排邊線不足。 為什麼向應用程式內的所有服務發出關閉命令會同時導致問題？

#. 按一下每個Profile Action以記錄當前編排邊線的存在（或不存在）。

   .. figure:: images/14.png

   要解決此問題，您將手動定義服務之間的相依性關係。

#. 選擇 **WebServer** 服務，然後按一下“服務”圖示上方顯示的 **Create Dependency**圖示，然後按一下 **MySQL**服務。

   .. figure:: images/15.png

#. 這表示 **WebServer**服務相依性於MySQL服務，這意味著 **MySQL**服務將在W**MySQL**服務之前啟動，然後在 **MySQL**服務之後停止。

#. 現在，為 **HAProxy**服務創建相依性項以相依性 **WebServer**服務。

#. 點擊 **Save**.

#. 重新存取概要檔操作並確認邊線現在可以正確反映服務之間的相依性關係，如下所示：

   .. figure:: images/16.png

   繪製白色的相依性關係箭頭將使Calm為所有“系統定義的設定檔操作”（創建，開始，重新啟動，停止，刪除和軟刪除）創建編排邊線。

部署和管理應用
+++++++++++++

#. 在藍圖編輯器的上方工具列中，按一下 **Launch**.

#. 指定唯一 **Application Name** (e.g. *Initials*\ -CalmLinuxIntro1) 和你的 **User_initials** 為VM命名的Runtime變數值。

#. 點擊 **Create**.

    **Audit** 選項卡可用于監視應用程式的部署。

   為什麼在下載磁片映射後所有基於CentOS的服務都不同時部署？

#. 一旦應用進入 **Running** 狀態, 導航到**Services**選項卡，然後選擇**HAProxy**服務以確定您的負載等化器的IP位址。

#. 在新的瀏覽器標籤或視窗中，導航到 \http://<HAProxy-IP>, 並驗證您的工作管理員應用程式是否正常運行。

   .. note::

    您也可以按一下“應用程式描述”中的連結。

   .. figure:: images/17.png

概要總結
+++++++++

您應該瞭解 ** Nutanix Calm **的關鍵要點是什麼？

-Nutanix Calm作為Prism的本機元件，建立在該平臺上並發揚光大。 Acropolis提供的簡單性使Calm專注于應用程式，而不是試圖掩蓋基礎架構管理的複雜性。

-Calm藍圖易於使用。在60分鐘內，您從零開始進行了完整的基礎架構堆疊部署。由於Calm使用標準工具（bash，PowerShell，Python等）進行配置，因此無需學習任何新語言，因此您可以立即應用已有的技能和代碼。

-儘管視覺效果不佳，但即使是單個VM藍圖也會對客戶產生巨大影響。印度的一家銀行正在將Calm用於單VM部署，從而將這些應用程式的部署時間從3天減少到2小時。請記住，當今許多客戶很少或根本沒有自動化（或者他們擁有的自動化非常複雜/難以理解，因此限制了它的採用）。這意味著Calm可以立即，立即，即時地為他們提供幫助。

-“多雲應用程式自動化和生命週期管理”聽起來很嚇人。 “未來”聽起來很棒，但是許多操作員看不到通往那裡的道路。聆聽客戶今天所苦惱的事情（備份需要專業技能，VM部署需要很長時間，升級很困難），並講解Calm如何提供幫助；跳到多雲自動化的故事，將Calm從“我現在需要這個”推到“一旦事情平靜下來，讓我們稍後再評估一下”（而且事情永遠不會真正“安靜下來（Calm）”。）

-藍圖編輯器提供了一個簡單的UI，用於為可能複雜的應用程式建模。

-藍圖與SSP專案相關，可用於實施配額和基於角色的存取控制。

-具有藍圖安裝和配置二進位檔案意味著不再為單個應用程式創建特定的映射。相反，可以通過對藍圖或安裝腳本的更改來修改應用程式，這兩種方法都可以儲存在原始程式碼儲存庫中。

-變數允許自訂應用程式的另一個維度，而無需編輯基礎藍圖。

-有多種對VM進行身份驗證的方法（金鑰或密碼），具體取決於源映射。

-可以即時監視應用程式狀態。

-應用程式通常跨多個VM，每個VM負責不同的服務。 Calm能夠自動化和協調完整的應用程式。

-服務之間的相依性關係可以在藍圖編輯器中輕鬆建模。

-使用者可以快速調配整個應用程式堆疊以進行生產或測試，以獲得可重複的結果，而不會浪費時間進行手動配置。

-有興趣使用Calm進行更多應用生命週期操作嗎？看看 :ref:`calm_day2`!


.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/blueprints.png
.. |projects| image:: images/projects.png
