.. _files_smb_share:

----------------------------
Files: 創建 SMB Share
----------------------------

簡介
++++++++

在本練習中，您將創建和測試SMB Share，用於支援主目錄，使用者設定檔以及其他非結構化檔案資料，例如Windows用戶端通常存取的部門Share。

使用 SMB Shares
++++++++++++++++

創建 Share
..................

#. 在 **Prism > File Server**, 按一下 **+ Share/Export**.

#. 填寫以下欄位：

   - **Name** - Marketing
   - **Description (Optional)** - Departmental share for marketing team
   - **File Server** - *Initials*\ **-Files**
   - **Share Path (Optional)** - Leave blank. This field allows you to specify an existing path in which to create the nested share.
   - **Max Size (Optional)** - Leave blank. This field allows you to set a hard quota for the individual share.
   - **Select Protocol** - SMB

   .. figure:: images/14.png

#. 按一下 **Next**.

#. 選擇 **Enable Access Based Enumeration** 和 **Self Service Restore**.

   .. figure:: images/15.png

   在創建部門Share時，應將其創建為 **Standard** Share。 這意味Share中的所有根目錄和檔案以及與Share的連接均由單個Files VM提供。

   **Distributed** Share適用於home主目錄，使用者設定檔和應用程式檔案夾。 這種類型的Share在所有Files VM上分佈根目錄，並在檔案共用群集內的所有Files VM之間平衡連接的負載。
   **Access Based Enumeration (ABE)** 確保只有給定用戶具有讀取許可權的檔和資料夾對該用戶可見。 Windows共用目錄通常啟用此功能。

   **Self Service Restore** 允許用戶利用Windows先前版本輕鬆地將單個檔案還原到基於Nutanix快照的先前版本。

#. 按一下 **Next**.

#. 查看 **Summary** 並按一下 **Create**.

   .. figure:: images/16.png

驗證 Share
.................

#. 通過RDP或控制台連接 *Initials*\ **-ToolsVM**。

   .. note::

     ToolsVM已加入 **NTNXLAB.local** 網域。 您可以使用任何加入網域的VM來完成以下步驟。

#. 在 **File Explorer**打開 ``\\<Intials>-Files.ntnxlab.local\``。

   .. figure:: images/17.png

#. 通過將上一步中下載的SampleData_Small.zip檔解壓縮到Share中，以測試對Marketing Share的存取。

   .. figure:: images/18.png

    - **NTNXLAB\\Administrator** 在檔案共用群集部署期間，該用戶被指定為檔管理員，預設情況下授予該使用者對所有Share的讀/寫存取權限。
    - 管理其他用戶的存取權限與任何其他SMB Share相同。

#. 右擊 **Marketing > Properties**.

#. 選擇 **Security** 選項卡並點擊 **Advanced**.

   .. figure:: images/19.png

#. 選擇 **Users (**\ *Initials*\ **-Files\\Users)** 並按一下 **Remove**。

#. 按一下 **Add**.

#. 按一下 **Select a principal** 在 **Object Name** 欄位填寫 **Everyone** 。 按一下 **OK**。

   .. figure:: images/20.png

#. 填寫以下欄位，然後按一下 **OK**:

   - **Type** - Allow
   - **Applies to** - This folder only
   - 選擇 **Read & execute**
   - 選擇 **List folder contents**
   - 選擇 **Read**
   - 選擇 **Write**

   .. figure:: images/21.png

#. 按一下 **OK > OK > OK** 保存許可權更改。

   現在，所有用戶都可以在Marketing Share中創建資料夾和檔案。

    許多人利用share來利用配額以確保公平使用資源是很常見的。 通過Files，可以為Active Directory內的單個用戶或特定的Active Directory安全性群組按份額設置軟配額或硬配額。

#. 在 **Prism > File Server > Share > Marketing**, 按一下 **+ Add Quota Policy**.

#. 填寫以下欄位，然後按一下 **Save**:

   - 選擇 **Group**
   - **User or Group** - SSP Developers
   - **Quota** - 10 GiB
   - **Enforcement Type** - Hard Limit

   .. figure:: images/22.png

#. 按一下 **Save**.

#. 在仍選擇“Marketing”共用目錄的情況下，查看 **Share Details**，**Usage** 和 **Performance** 選項卡以瞭解每個share的可用情況，包括檔案和連接的數量，一段時間內的儲存利用率 ，延遲，輸送量和IOPS。


   .. figure:: images/23.png
