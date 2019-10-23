.. _flow_secure_app:

----------------
Flow: 應用安全
----------------

*完成本實作預計需要30分鐘*

概述
++++++++

Flow是一個應用為中心的網路安全產品，與Nutanix AHV和Prism Central緊密結合。Flow對運行在AHV上的虛擬機器提供了豐富的網路流量視覺化 ，自動化和安全功能。
微分割是Flow的元件，使用簡單的策略管理來保護虛擬網路安全。使用Prism Central的多個類別（邏輯組），你可以創建功能強大的分散式防火牆，給管理員提供應用為中心的策略管理工具，用以保護虛擬機器流量安全。
將其與Calm結合，可以自動化部署創建時就受保護的應用程式。
本實作中，你將創建一個安全性原則以限制應用虛擬機器之間的通信。

實作設置
+++++++++

本實作取決於多層 **Task Manger** Web應用的可用性。

Refer to the :ref:`taskman` lab for instructions on importing and launching the completed **Task Manager** blueprint. 有關導入和啟動已完成的 **Task Manager** 藍圖，參考 :ref:`taskman` 實作說明。 

啟動 **Task Manager** 部署後即可開始本實作。 **您無需等待藍圖部署完成即可開始本實作。**  

保護應用程式
+++++++++++++++++++++++

Flow提供了多種開箱即用的系統類別，例如AppType，AppTier和Environment，可以用來快速對虛擬機器分組。立即開始使用這些預定義的類別，或添加您自己的類別用來自定義分組。

定義類別值
........................

Prism Central 使用類別來作為中繼資料，以標記虛擬機器，並確定如何應用策略。

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Virtual Infrastructure > Categories**.

#. 選擇 **AppType** 核取方塊並按一下 **Actions > Update**.

   .. figure:: images/12.png

#. 按一下上一個值旁邊的 :fa:`plus-circle` 圖示，添加額外的類別值。

#. 指定 *Initials*-**TaskMan** 值名稱。

   .. figure:: images/13.png

#. 點擊 **Save**.

#. 選擇 **AppTier** 核取方塊並點擊 **Actions > Update**.

#. 按一下上一個值旁邊的 :fa:`plus-circle` 圖示，添加額外的類別值。

#. 指定 *Initials*-**TMWeb**  值名稱。這個類別會被用於應用的Web層。

#. 按一下 :fa:`plus-circle` 並指定 *Initials*-**TMDB**. 這個類別會被用於應用的MySQL資料庫。

#. 點擊 :fa:`plus-circle` 並指定 *Initials*-**TMLB**. 這個類別會用於應用的HAProxy load balancer.

   .. figure:: images/14.png

#. 點擊 **Save**.

創建安全性原則
..........................

Nutanix Flow包含策略驅動的安全性框架，該框架使用以工作負載為中心的方法，而不是以網路為中心。因此，無論它們的網路配置如何更改以及它們在資料中心中的位置如何，它都可以檢查往返VM的流量。 以工作負載為中心，與網路無關的方法還使虛擬化團隊能夠實施這些安全性原則，而不必依賴網路安全團隊。

安全性原則適用於類別（虛擬機器的邏輯分組），而不適用於虛擬機器本身。 因此，在給定類別中啟動多少個虛擬機器是無關緊要的。 類別中虛擬機器的相關聯的流量可以受到保護，無需任何規模的管理干預。

在你等待Task Manager應用通過Calm藍圖部署的過程中，創建保護應用的安全性原則。

#. 在 **Prism Central** 下, 選擇 :fa:`bars` **> Policies > Security Policies**.

#. 按一下 **Create Security Policy > Secure Applications (App Policy) > Create**.

#. 填寫以下欄位:

   - **Name** - *Initials*-AppTaskMan
   - **Purpose** - Restrict unnecessary access to Task Manager
   - **Secure this app** - AppType: *Initials*-TaskMan
   -  **不要** 選擇 **Filter the app type by category**.

   .. figure:: images/18.png

#. 點擊 **Next**.

#. 如果有提示, 在 **Create App Security Policy** 嚮導教程圖上點擊 **OK, Got it!** 。

#. 為了更詳細配置安全性原則，點擊 **Set rules on App Tiers** ，而不是對應用所有的元件應用相同的規則。

   .. figure:: images/19.png

#. 點擊 **+ Add Tier**.

#. 從下拉式功能表中選擇 **AppTier:**\ *Initials*-**TMLB** .

#. 對 **AppTier:**\ *Initials*-**TMWeb** 和 **AppTier:**\ *Initials*-**TMDB** 重複 7-8 。

   .. figure:: images/20.png

   接下來你將定義 **Inbound** 規則，該規則定義了允許與你創建的應用程式通訊的來源端。你可以允許所有的進站流量，或定義來源白名單。預設情況下，安全性原則是預設阻止所有進站流量的。

   在本場景下，我們將允許生產網路上所有用戶端上埠80的進站TCP流量。

#. 在 **Inbound** 下, 點擊 **+ Add Source**.

#. 指定 **Environment:Production** 並點擊 **Add**.

   .. note::

     來源可以通過IP或子網指定，但類別提供了更多的靈活性，因為資料可以跟隨虛擬機器而與其網路位置變化無關 。

#. 創建進站策略, 選擇 **AppTier:**\ *Initials*-**TMLB** 旁邊的圖示e **+** 。

   .. figure:: images/21.png

#. 填寫以下欄位:

   - **Protocol** - TCP
   - **Ports** - 80

   .. figure:: images/22.png

   .. note::

     可以將多協定和埠添加到單個規則。

#. 按一下 **Save**.

   Calm的工作流可能也需要存取這些虛擬機器，包括橫向擴展，縱向擴展，或升級。Calm通過SSH（TCP埠 22）與這些虛擬機器通信。

#. 在 **Inbound** 下, 點擊 **+ Add Source**.

#. 填寫以下欄位:

   - **Add source by:** - 選擇 **Subnet/IP**
   - 指定 *Your Prism Central IP*\ /32

   .. note::

      **/32** 表示單個IP，而不是一個子網。

   .. figure:: images/23.png

#. 點擊 **Add**.

#. 選擇 **AppTier:**\ *Initials*-**TMLB** 旁邊的圖示 **+**  ，指定 **TCP** 埠 **22** 並按一下 **Save**.

#. 對 **AppTier:**\ *Initials*-**TMWeb** 和 **AppTier:**\ *Initials*-**TMDB** 重複步驟18，以允許Calm跟Web層和資料庫虛擬機器通信。

   .. figure:: images/24.png

   預設情況下，安全性原則允許應用發送所有出站流量到任意目的地。應用程式唯一需要的出站通信是資料庫虛擬機器能夠與DNS伺服器通信。

#. 在  **Outbound**, 從下拉式功能表中選擇 **Whitelist Only** , 並點擊 **+ Add Destination**.

#. 填寫以下欄位:

   - **Add source by:** - 選擇 **Subnet/IP**
   - 指定 *Your Domain Controller IP*\ /32

   .. figure:: images/25.png

#. 點擊 **Add**.

#. 選擇 **AppTier:**\ *Initials*- **TMDB** 右邊的 **+** 圖示, 指定 **UDP** 埠 **53** 並點擊 **Save** ，以允許DNS流量.

   .. figure:: images/26.png

   應用的每一層都與其他層進行通信，該策略必須允許該流量。某些層，如負載平衡器和Web不需要在同一層內進行通信。

#. 定義 intra-app 通信, 點擊 **Set Rules within App**.

   .. figure:: images/27.png

#. 點擊 **AppTier:**\ *Initials*-**TMLB** 並選擇 **No** ，以防止本層虛擬機器間的通信。在本層只有一個負載平衡器。

#.  **AppTier:**\ *Initials*-**TMLB** 依舊被選中, 點擊 **AppTier:**\ *Initials*-**TMWeb** 右邊的 :fa:`plus-circle` 圖示創建分層之間的規則.

#. 填寫以下欄位以允許負載平衡器Web層之間的TCP埠80上的通信：

   - **Protocol** - TCP
   - **Ports** - 80

   .. figure:: images/28.png

#. 點擊 **Save**.

#. 點擊 **AppTier:**\ *Initials*-**TMWeb** 並選擇 **No** 以防止本層虛擬機器之間的通訊。當有多個Web虛擬機器時，他們之間不需要通信。

#. 當 **AppTier:**\ *Initials*-**TMWeb** 依舊被選中, 點擊 **AppTier:**\ *Initials*-**TMDB** 右邊的 :fa:`plus-circle` 圖示創建另一個分層之間的規則。

#. 填寫一下欄位以允許TCP埠3306上的通信，從而允許Web伺服器和MySQL資料庫之間的資料庫連接：

   - **Protocol** - TCP
   - **Ports** - 3306

   .. figure:: images/29.png

#. 點擊 **Save**.

#. 點擊 **Next** 審核安全性原則。

#. 點擊 **Save and Monitor** 並保存策略。

分配類別值
.........................

.. note::

到這個時候，你的應用藍圖應該已經完成部署。如果還沒有完成，請等待直至完成。

現在，你需要將先前創建的類別應用於那些從工作管理員藍圖部署的虛擬機器。Flow類別可以作為Calm藍圖的一部分進行分配，但本實作的目的是為了理解對環境中現有的虛擬機器進行分配類別。

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Virtual Infrastructure > VMs**.

#. 點擊 **Filters** 並搜索 *Initials-* 來羅列你的虛擬機器.

   .. figure:: images/15.png

#. 使用核取方塊，選擇與應用（HAProxy, MYSQL, WebServer-0, WebServer-1）相關聯的4台虛擬機器。

   .. figure:: images/16.png

   .. note::

     您還可以使用 **Label** 功能，將來可以更快地搜索該組虛擬機器。

     .. figure:: images/16b.png

#. 搜索欄中指定 **AppType:**\ *Initials*-**TaskMan** 並點擊 **Save** 圖示將類別批量分配這4台虛擬機器。

#. 只選定 *Initials*\ **-HAProxy** 虛擬機器, 選擇 **Actions > Manage Categories**, 指定 **AppTier:**\ *Initials*-**TMLB** 類別並點擊  **Save** 。

   .. figure:: images/17.png

#. 重複步驟 5 分配 **AppTier:**\ *Initials*-**TMWeb** 給你的web層虛擬機器.

#. 重複步驟 5 分配 **AppTier:**\ *Initials*-**TMDB** 給你的MySQL虛擬機器.

#. 最後, 步驟 5 分配 **Environment:Dev** 給你 Windows用戶端虛擬機器.

監控和應用安全性原則
+++++++++++++++++++++++++++++++++++++++++

在應用Flow策略之前, 您需要確保工作管理員應用按預期正常工作.

應用測試
.......................

#. 從 **Prism Central > Virtual Infrastructure > VMs** , 記錄 *Initials*\ **-HAPROXY-0...** 和 *Initials*\ **-MYSQL-0...** 虛擬機器的IP地址.

#. 啟用 *Initials*\ **-WinClient-0** 虛擬機器控制台.

   這台虛擬機器是工作管理員應用藍圖創建的一部分。

#. 從 *Initials*\ **-WinClient-0** 控制台中打開瀏覽器並存取 \http://*HAPROXY-VM-IP*/.

#. 驗證應用已載入並且任務可以被添加和刪除。

   .. figure:: images/30.png

#. 打開 **Command Prompt** 並運行 ``ping -t MYSQL-VM-IP`` 驗證用戶端與資料庫之間的連通性. 保持ping繼續運行。

#. 打開另外一個 **Command Prompt** 視窗並運行 ``ping -t HAPROXY-VM-IP`` 以驗證用戶端與負載平衡器之間的連通性。保持ping繼續運行。

   .. figure:: images/31.png

使用Flow視覺化
........................

#. 返回 **Prism Central** 並選擇 :fa:`bars` **> Virtual Infrastructure > Policies > Security Policies >**\ *Initials*-**AppTaskMan**.

#. 驗證 **Environment: Dev** 顯示為入站來源。來源和黃色線表明已監測到來自你客戶虛擬機器的流量。

   .. figure:: images/32.png

#. 將滑鼠停留在 **Environment: Dev** 與 **AppTier:**\ *Initials*-**TMLB** 對連接線上查看協定和連接資訊。

#. 點擊這條黃色流線查看過去24小時內連接嘗試圖表。

   .. figure:: images/33.png

   是否還有其他檢測到的出站流量？將滑鼠懸停在這些連接上並確定哪些埠正在被使用。

#. 點擊 **Update** 以編輯策略.

   .. figure:: images/34.png

#. 按一下 **Next** 並等待檢測到的流量進行填充。

#. 滑鼠懸停在連接到 **AppTier:**\ *Initials*-**TMLB** 的 **Environment: Dev** 源上並點擊出現的 :fa:`check` 圖示.

   .. figure:: images/35.png

#. 點擊 **OK** 完成添加規則.

    **Environment: Dev** 來源應該變成藍色, 表示它已是策略的一部分。 將滑鼠懸停在流線上，並驗證是否同時顯示了ICMP（ping通信）和TCP埠80。

#. 點擊 **Next > Save and Monitor** 以更新策略.

應用Flow策略
......................

In order to enforce the policy you have defined, the policy must be applied.為了執行已定義的策略，必須應用策略 。

#. 選擇 *Initials*-**AppTaskMan**  並按一下 **Actions > Apply**.

   .. figure:: images/36.png

#. 在確認對話方塊內輸入  **APPLY** 並按一下 **OK** 開始阻止流量.

#. 返回 *Initials*\ **-WinClient-0** 控制台.

   從Windows用戶端到資料庫伺服器的持續ping通信會發生什麼？ 該流量被阻止了嗎？

#. 驗證Windows客戶虛擬機器通過web瀏覽器仍然可以存取工作管理員應用和負載平衡器的IP。 

   您是否可以輸入需要web伺服器和資料庫通信的新任務？

概要總結
+++++++++

- 微分割技術可提供額外的保護，抵禦源自資料中心內部並從一台電腦橫向傳播到另一台電腦的惡意威脅。
- Prism Central下創建的類別在Calm藍圖內部是可使用的。 
- 在Prism Central，安全性原則利用了基於文本的策略。
- Flow能夠限制運行在AHV平臺上虛擬機器的特定埠和協定上的流量。
- 在 **Save and Monitor** 模式下創建的策略, 意味著流量實際上沒有被阻止，除非應用了策略。這有助於瞭解連接數和保證無意阻止任意流量。


