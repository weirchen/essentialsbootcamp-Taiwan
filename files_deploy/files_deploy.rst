.. _files_deploy:

-------------
Files: 部署Nutanix Files功能
-------------

*完成本節所有Files實作所需的時間約為：1小時*

簡介
++++++++

傳統架構中，檔案共享儲存往往容易成為IT部門的另一個孤島，從而遇到與SAN儲存中使用過程中相同的擴展問題和缺乏持續創新的困擾，並帶來了很多不必要的複雜性。 Nutanix認為企業雲中沒有孤島存在的空間。通過將檔儲存演變為一種應用程式，並運行在經過驗證的HCI核心之上，Nutanix Files可通過一鍵式管理，同時為用戶提供高性能，可伸縮性和快速創新的檔案共享儲存服務。

**在本實作中，您將逐步完成Nutanix Files功能的部署，並練習如何管理SMB共用和NFS共用，以及如何進行配置擴展，並探索全新的檔案共享功能。該實作還將提供有關部署，配置和用例中需要特別關注的注意事項。**

.. _deploying_files:

實作前置條件
+++++++++
本實作需要使用在實作Windows_tools_vm章節中配置的應用程式。

如果在此實作之前還未尚未部署此VM，請在繼續本練習之前，先根據該連結的步驟完成前置實作條件。

部署Nutanix Files服務
++++++++++++

#. 在 **Prism > File Server** 菜單中, 點擊 **+ File Server** 以打開 **New File Server Pre-Check** 對話方塊.

   .. figure:: images/1.png

    為了節省時間，Files 3.5.2應用套裝程式已經上載到您的集群中，如果您發現沒有此套裝程式，請到my.nutanix.com中進行下載。套裝程式二進位檔案可以直接通過Prism下載，也可以在Nutanix support portal中進行下載並手動上傳。

   .. figure:: images/2.png

    此外，超融合群集的 **Data Services** IP位址已經預先配置為（* 10.XX.YY.38 *）。在Nutanix Files群集中，儲存需要通過iSCSI協定，通過Volume Group 提供給檔虛擬機器，因此依賴於此資料服務IP。
   .. 注::

     如果是在您自己的環境中而不是預先準備的HPOC環境中進行部署，則可以通過以下方式輕鬆配置您自己環境的資料服務IP：選擇> fa：`gear` **>Cluster Details** ，指定 
**iSCSI Data Services IP** ，然後按一下 **Save** 。當前，資料服務IP要求必須與CVM在同一子網中。

   最後，Files將確保在群集上至少配置了1個網路。建議至少配置2個分段網路，以便將用戶端網路和儲存網路進行隔離。

#. 點擊 **Continue**.

   .. figure:: images/3.png

#. 填寫以下欄位:

   - **Name** - *Intials*-Files (e.g. XYZ-Files)
   - **Domain** - ntnxlab.local
   - **File Server Size** - 1 TiB

   .. figure:: images/4.png

   .. 注::

     按一下 **Custom Configuration** 將允許您根據使用者數和輸送量目標更改檔VM的放大和縮小尺寸. 它還允許手動調整“Files”群集的大小。

     .. figure:: images/5.png

#. 點擊 **Next**.

#. 為 **Client Network** 選擇 **Secondary - Managed** VLAN .

   每個Files VM將在用戶端網路上使用一個IP

   .. 注::

    在HPOC環境中，如果需要採用不同的用戶端網路和儲存網路，則將用戶端網路分配至第二VLAN至關重要

    在生產環境中，通常會使用專用虛擬網路來部署Files，以隔離使用者用戶端流程和儲存通訊流量。當使用兩個隔離網路時，根據設計，Files將禁止用戶端訪問儲存網路，這意味著，分配給第一網路的所有VM將無法訪問共用。


   .. 注::

     由於本實作使用通過AHV管理的網路，因此不需要配置單個IP。但在ESXi環境中，或在使用不受管理的AHV網路時，您將需要手工指定網路詳細資訊和可用IP資訊，如下所示

     .. figure:: images/6.png

#. 將群集的 **Domain Controller** IP（位於：ref：`stagingdetails`中）指定為“ DNS解析器IP”（例如10.XX.YY.40）。保留預設（群集）NTP伺服器
   .. raw:: html

     <strong><font color="red">為使Files群集成功找到並加入NTNXLAB.local域，將DNS解析器IP設置為您的集群的網域控制站的虛擬機器IP是至關重要的。預設情況下，此欄位設置為Nutanix群集配置的主DNS伺服器IP，此值不正確，將不起作用。</font></strong>

   .. figure:: images/7.png

#. 點擊 **Next**.

#. 選擇儲存網路的 **Primary - Managed** VLAN.

   Each Files VM will consume a single IP on the storage network, plus 1 additional IP for the cluster.
   每個Files VM將在儲存網路上消耗一個IP位址，同時整個群集還需要額外分配1個IP位址。

   .. figure:: images/8.png

#. 點擊 **Next**.

#. 填寫以下欄位:

   - 選擇 **Use SMB Protocol**
   - **用戶名** - Administrator@ntnxlab.local
   - **密碼** - nutanix/4u
   - 選擇 **Make this user a File Server admin**
   - 選擇 **Use NFS Protocol**
   - **User Management and Authentication** - 選擇非託管模式(unmanaged)

   .. figure:: images/9.png

   .. 注:: 在非託管模式下，僅通過UID / GID來標識使用者，在Files 3.5版本中，Files可同時支援NFSv3 和 NFSv4

#. 點擊 **Next**.

   預設情況下，Files將自動創建一個“保護域”，並為Files群集的每天創建Daily的快照並保留最後兩個快照。在部署完成後，可以修改快照日程計畫並定義遠端複製網站。

   .. figure:: images/10.png

#. 點擊 **Create** 以開始Files部署.

#. 在 **Prism > Tasks** 中監視部署進度

   部署大約需要10分鐘.

   .. figure:: images/11.png

   .. 注::

   如果您收到有關DNS記錄驗證失敗的警告，可以放心地忽略。共用群集沒有使用與檔案共享群集相同的DNS伺服器，因此無法解析在部署Files服務時創建的DNS條目。

#. 在等待Files 伺服器部署過程中，如果您尚未部署Windows Tools 虛擬機器，則可以在此時間進行

#. 通過RDP協議或遠端控制台連接到Windows Tools VM

#. 將用於文件分析的示例文件下載到工具VM：

   - `https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip <https://peerresources.blob.core.windows.net/sample-data/SampleData_Small.zip>`_

#. 將檔案共用分析json和qcow檔下載到Tools VM

   - `nutanix-file-analytics-2.0.0-metadata.json <http://10.42.194.11/workshop_staging/fileanalytics-2.0.0.json>`_
   - `nutanix-file-analytics-2.0.0.qcow2 <http://10.42.194.11/workshop_staging/nutanix-file_analytics-el7.6-release-2.0.0.qcow2>`_

#. 完成後，返回到“ Prism> File Server”，然後選擇*Initials*\ **-Files** 伺服器，然後按一下 **Protect**。

   .. figure:: images/12.png

#. 觀察預設的自助服務還原的日程計畫，此功能同時控制Windows以前版本功能的快照計畫。支援Windwos先前版本的功能，允許最終使用者無需聯繫儲存或備份管理員，就可以實現對檔的恢復和回滾操作。請注意，這些本地快照不能保護在本地集群出故障時，檔案伺服器集群不受影響，並且可以支援整個檔案系統的資料可以直接複製到遠端網站。點擊 **Close** 。
   .. figure:: images/13.png

概要總結
+++++++++

關於 **Nutanix Files** ，您應該瞭解哪些關鍵知識？

-Files 可以快速部署在現有Nutanix群集之上，從而為使用者共用，主目錄，部門共用，應用程式和任何其他通用檔案儲存需求提供SMB和NFS儲存。
-Files不是單點解決方案。VM，檔案，區塊和物件儲存都可以在同一平臺上使用相同的管理工具來交付，從而降低了複雜性和管理孤島

