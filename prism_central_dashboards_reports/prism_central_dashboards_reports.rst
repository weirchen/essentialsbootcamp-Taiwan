.. _prism_central_dashboards_reports:

-------------------------------------
Prism Central: 儀表板和報表
-------------------------------------

簡介
++++++++

本實驗將介紹Prism Central的面板和報表功能.

Prism Central 面板
++++++++++++++++++++++++

Prism Central 儀表板允許您快速查看在Prism Central實例下註冊的系統的當前狀態。

您可以從儀表板獲得的資訊類型取決於您的興趣。它可以包括與集群運行狀況相關的任何事件的警報，一直到效能資訊。

.. figure:: images/510PC1.png

創建客製化儀表板
.........................

儀表板也可以客製化

#. 點擊 **Home X**

主儀表板是系統首次安裝時顯示的第一個儀表板。

#. 此儀表板也可以自訂，但我們會創建新的儀表板。

#. 點擊 **Manage Dashboards** 打開“管理儀表板”視窗

#. 點擊 **+ New Dashboard**

#. 輸入 "Dashboard-*intials*" 作為名稱，然後按一下 **Save**.

.. figure:: images/510PC2.png

#. 現在我們添加一些視窗小工具.


.. note::

  視窗小工具指的是儀表板的不同部分。視窗小工具可以是圖形，警報板，也可以是顯示區塊的資訊表。您可以選擇讓小工具通知您感興趣的任何資訊，只需從可用的實體中進行選擇即可。

#. 點擊 **Add Widgets**.

#. From the list of available Widgets on the left hand side, notice there are three types of Widgets:

- CUSTOM WIDGETS
- CLUSTER WIDGETS
- APP WIDGETS

#. 選擇 **CUSTOM WIDGETS** 裡您可能感興趣的任何小工具，並注意右側顯示一組自訂參數以指定。

#. 根據需要在選項中進行選擇，然後按一下 **Add to Dashboard**.

.. figure:: images/510PC4.png

#. 接下來，從 **CLUSTER WIDGETS**列表中選擇另一個小工具。 這次按一下 **Or, Add & Return to Dashboard.** 返回新創建的儀表板。

.. note::

  Prism Central的右上角有一個 **+ Add Widgets**按鈕，可以隨時添加更多小工具。

  另請注意，將滑鼠懸停在儀表板上的任何小工具上都會在右上角顯示一個灰色的X，可用於刪除小工具。

.. figure:: images/510PC5.png

Prism Central 報表
+++++++++++++++++++++

Prism Central允許您生成有關群集環境的歷史報告，可用于幫助管理員監控他們管理的群集的運行狀況和效能。此類報告可包括資源消耗，異常行為和其他有價值的操作見解。這些報告可以手動生成，也可以從Prism Central自動生成，以便在最方便時通過電子郵件發送。

#. 在 **Prism Central  > Operations > Reports**.

#. 在那裡，您將看到兩個預定義的報告可供您立即使用:

- Cluster Efficiency Summary
- Environment Summary

.. figure:: images/510reports1.png

#. 讓我們運行 **Cluster Efficiency Summary** 報表.

#. 選擇 **Cluster Efficiency Summary**, 點擊在 **Actions** 下拉式功能表中的 **Run** .

.. figure:: images/monitoring_01.png

#. 接下來，填寫以下欄位並從 **Actions** 下拉式功能表按一下 **Run** :

- **Report instance Name** - *initials* - Cluster Efficiency 
- **Time Period for Report** - Last 24 Hours

#. 點擊 **Run**.

#. 現在我們來運行 **Environment Summary** 報表.

#. 選擇 **Environment Summary**, 從 **Actions**下拉式功能表點擊 **Run** .

#. 接下來，填寫以下欄位並按一下 **Run**:

- **Report instance Name** - *initials* - Environment Summary
- **Time Period for Report** - Last 24 Hours

#. 點擊 **Run**.

#. 報告完成後，選擇每個報告，然後執行以下操作:

#. 點擊 **View Instances.** 從 **Actions** 的下拉式功能表.

- 要在單獨的選項卡中查看報告，請按一下報告的名稱。
- 要下載報告，請選中其核取方塊，然後按一下右上頁面的 **Download**。

#. 查看您在本練習中創建的報告的內容。

生成客製化報表
......................

#. 要創建新的自訂報告，請按一下 **+ New Report**.

#. 修改報表名字 **New Report** 為 *initials*-**Report**

.. figure:: images/510reports3.png

#. 從 **CUSTOM VIEWS** 目錄左邊, 點擊 **Line Chart**填寫下面資訊:

- **Entity Type** - Cluster
- **Metric** - Memory Usage
- **Tittle** - *initials* - Cluster Memory Usage
- **Number of Entities** – 10
- **Sort Order** - Ascending

#. 點擊 **Add**

.. figure:: images/510reports2.png

#. 從 **PRE-DEFINED VIEWS**, 點擊任何你感興趣的entities物件。

.. note::

  由於這些是預定義的，因此不需要額外的配置步驟，它們會立即添加到報告中。

#. 點擊位於右邊角落的 **Add Schedule** 按鈕添加自動生成報告計畫。

#. 選擇任何所需的頻率，時間和持續時間以運行報告。

.. figure:: images/510reports4.png

.. note:: 

  如果在Prism Central中正確配置了SMTP，則此自動報告也可以發送到輸入的任何有效電子郵寄地址。

#. 客製完你的報表之後點擊 **Save** 。

#. 現在您的報告已保存，但請注意，它沒有任何實例。 這是因為我們還沒有運行報告。

#. 點擊右上角的 **Run**來運行報告。

.. figure:: images/510reports5.png

.. note::

  複製報告對於利用現有報告並對其進行編輯以進一步進行自訂非常有用。

#. 報告完成後，您將通過單 **下載**下的**PDF**看到報告的第一個實例可供查看。

#. 然後按一下右上角的X退出。

#. 如果您按原樣保留報告，它將自動運行並以設置的特定頻率和時間發送到提供的電子郵寄地址。

#. 如果需要不同的顏色或徽標，也可以在 **Report Settings**下自訂報告。


概要總結
+++++++++

- Prism Central可自訂儀表板允許您使用他們關心的資訊設置使用者和團隊特定儀表板。
- Prism Central報告管理功能使您能夠根據配置的計畫配置和提供包含有關基礎結構資源的資訊的歷史報告。
