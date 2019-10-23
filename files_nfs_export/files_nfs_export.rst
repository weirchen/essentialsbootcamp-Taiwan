.. _files_nfs_export:

------------------------
Files: 創建 NFS Export
------------------------

簡介
++++++++

在本練習中，您將創建和測試NFSv4 Export，該Export用於支援集群應用程式，儲存應用程式資料（例如日誌記錄）或儲存Linux用戶端通常存取的其他非結構化檔資料。

使用 NFS Exports
+++++++++++++++++

創建Export
...................

#. 在 **Prism > File Server**, 按一下 **+ Share/Export**.

#. 填寫以下欄位：

   - **Name** - logs
   - **Description (Optional)** - File share for system logs
   - **File Server** - *Initials*\ **-Files**
   - **Share Path (Optional)** - Leave blank
   - **Max Size (Optional)** - Leave blank
   - **Select Protocol** - NFS

   .. figure:: images/24.png

#. 按一下 **Next**.

#. 填寫以下欄位：

   - 選擇 **Enable Self Service Restore**
      - These snapshots appear as a .snapshot directory for NFS clients.
   - **Authentication** - System
   - **Default Access (For All Clients)** - No Access
   - 選擇 **+ Add exceptions**
   - **Clients with Read-Write Access** - *The first 3 octets of your cluster network*\ .* (e.g. 10.38.1.\*)

   .. figure:: images/25.png

   預設情況下，NFS Export將允許對安裝exports的任何主機進行讀/寫存取，但是可以將其限制為特定的IP或IP範圍。

#. 按一下 **Next**.

#. 閱讀 **Summary** 並按一下 **Create**.

測試 Export
..................

首先，您將提供一個CentOS VM用作Files exprot的用戶端。

.. note:: 如果已在另一個實作部署 :ref:`linux_tools_vm` ，則可以將此VM用作NFS用戶端。

#. 在 **Prism > VM > Table**, 點擊 **+ Create VM**.

#. 填寫以下欄位:

   - **Name** - *Initials*\ -NFS-Client
   - **Description** - CentOS VM for testing Files NFS export
   - **vCPU(s)** - 2
   - **Number of Cores per vCPU** - 1
   - **Memory** - 2 GiB
   - Select **+ Add New Disk**
      - **Operation** - Clone from Image Service
      - **Image** - CentOS
      - Select **Add**
   - Select **Add New NIC**
      - **VLAN Name** - Primary
      - Select **Add**

#. 點擊 **Save**.

#. 選擇 *Initials*\ **-NFS-Client** VM 並按一下 **Power on**.

#. 在Prism中記下VM的IP位址，並使用以下憑據通過SSH連接：

   - **Username** - root
   - **Password** - nutanix/4u

#. 執行以下命令：

     .. code-block:: bash

       [root@CentOS ~]# yum install -y nfs-utils #This installs the NFSv4 client
       [root@CentOS ~]# mkdir /filesmnt
       [root@CentOS ~]# mount.nfs4 <Intials>-Files.ntnxlab.local:/ /filesmnt/
       [root@CentOS ~]# df -kh
       Filesystem                      Size  Used Avail Use% Mounted on
       /dev/mapper/centos_centos-root  8.5G  1.7G  6.8G  20% /
       devtmpfs                        1.9G     0  1.9G   0% /dev
       tmpfs                           1.9G     0  1.9G   0% /dev/shm
       tmpfs                           1.9G   17M  1.9G   1% /run
       tmpfs                           1.9G     0  1.9G   0% /sys/fs/cgroup
       /dev/sda1                       494M  141M  353M  29% /boot
       tmpfs                           377M     0  377M   0% /run/user/0
       *intials*-Files.ntnxlab.local:/             1.0T  7.0M  1.0T   1% /afsmnt
       [root@CentOS ~]# ls -l /filesmnt/
       total 1
       drwxrwxrwx. 2 root root 2 Mar  9 18:53 logs

#. 觀察 **logs**目錄已安裝在 ``/filesmnt/logs``.

#. 重新啟動VM，並觀察到export不再掛載。 要保持掛載，通過執行以下命令將其添加到``/etc/fstab``中：

       echo 'Intials-Files.ntnxlab.local:/ /filesmnt nfs4' >> /etc/fstab

#. 以下命令將添加100個2MB的檔，其中填充了隨機資料 ``/filesmnt/logs``:

     .. code-block:: bash

       mkdir /filesmnt/logs/host1
       for i in {1..100}; do dd if=/dev/urandom bs=8k count=256 of=/filesmnt/logs/host1/file$i; done

#. 返回 **Prism > File Server > Share > logs** 監控效能和使用情況。

  請注意，利用率資料每10分鐘更新一次。
