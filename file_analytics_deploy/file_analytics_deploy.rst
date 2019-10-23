.. _file_analytics_deploy:

----------------------
File Analytics: 部署
----------------------

簡介
++++++++

在本練習中，您將部署File Analytics VM，並掃描現有共用以構建儀錶板。 您還將創建異常警報並查看檔案伺服器實例的審核詳細資訊。

部署 File Analytics
+++++++++++++++++++++

#. 在 **Prism** > **File Server** > 按一下 **Deploy File Analytics**

   .. figure:: images/31.png

#. 選擇 **Deploy**

   為了節省時間，File Analytics 2.0.0套裝程式已經上載到您的集群中。 可以從Nutanix入口網站網站下載檔案二進位檔案並手動上傳。

#. 上傳完成後，選擇 **Install**

#. 填寫詳細資訊

   - **Name** - Initials
   - **Storage Container** – Will automatically select the container used by your file server instance
   - **Network List** – Secondary - Managed

#. 選擇 **Show Advanced Settings**

#. 確保 **DNS Resolver IP** 設置為Active Directory的IP, ntnxlab.local, domain controller/DNS IP address and **ONLY** that address.

#. 選擇 **Deploy**

#. 你可以從 **Tasks** 頁面監控部署情況。 Analytics VM部署大約需要5分鐘。

#. In **Prism** > **File Server** > click **File Analytics**

   .. figure:: images/33.png

#. 在Enable File Analytics 頁面上，輸入您的網域管理員，該網域管理員也是您的檔案伺服器管理員。
   - **Username**: administrator
   - **Password**: nutanix/4u

   .. figure:: images/34.png

#. 選擇 **Enable**
