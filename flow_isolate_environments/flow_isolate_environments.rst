.. _flow_isolate_environments:

--------------------------
Flow: 環境隔離
--------------------------

*完成本實作預計30分鐘。*

概述
++++++++

在本實作中，你需要創建一個新的環境類別，並將其分配給工作管理員。之後 ，你需要用新建的類別來創建和實現隔離安全性原則，以限制未經授權的存取。

隔離環境
++++++++++++++++++++++

當必須完全禁止一組虛擬機器與其他一組虛擬機器進行通信而沒有任何白名單例外時，將會使用隔離策略。 一個常見的示例是使用隔離策略來阻止標記為 **Environment: Dev**的虛擬機器與 **Environment: Production**中的虛擬機器進行通信。 如果要在兩個組之間創建例外，請不要使用隔離策略，而應使用允許白名單模型的應用程式策略。

在本練習中，你將創建一個新的環境類別並將其分配給工作管理員。 然後，您將創建並實施使用新建的類別的隔離安全性原則，以限制未經授權的存取。

創建並分配類別
.................................

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Virtual Infrastructure > Categories**.

#. 選中 **Environment** 核取方塊並點擊 **Actions > Update**.

#. 點擊最後一個值旁邊的 :fa:`plus-circle` 圖示，以添加其他類別值。

#. 指定 *Initials*-**Prod** 值的名稱.

   .. figure:: images/37.png

#. 點擊 **Save**.

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Virtual Infrastructure > VMs**.

#. 點擊 **Filters** 並搜索 *Initials-* 以顯示你的虛擬機器。

   .. note::

     如果你先前為應用虛擬機器創建了標籤，你也可以搜索標籤。或者，你可以在篩檢程式（Filters）面板中搜索**AppType:** *Initials*-**TaskMan**類別。

     .. figure:: images/38.png

#. 使用核取方塊，選擇與應用關聯的4台虛擬機器(HAProxy, MYSQL, WebServer-0, WebServer-1)，並選擇**Actions > Manage Categories**。

#. 在搜索欄指定 **Environment:**\ *Initials*- **Prod** 並點擊 **Save** 圖示，將類別批量分配給所有4台虛擬機器。

   .. figure:: images/39.png

創建環境隔離策略
............................

#. 在 **Prism Central**, 按一下 :fa:`bars` **> Virtual Infrastructure > Policies > Security Policies**.

#. 點擊 **Create Security Policy > Isolate Environments (Isolation Policy) > Create**.

#. 填寫以下欄位:

   - **Name** - *Initials*-Isolate-dev-prod
   - **Purpose** - *Initials* - Isolate dev from prod
   - **Isolate This Category** - Environment:Dev
   - **From This Category** - Environment:*Initials*-Prod
   - **不要** 選擇 **Apply this isolation only within a subset of the datacenter**. 這個選項提供了額外的細微性，僅需要通過將虛擬機器分配給第三個、共同的類別。

   .. figure:: images/40.png

#. 點擊 **Apply Now** 保存策略並立即開始實行.

#. 返回到 *Initials*\ **-WinClient-0** 控制台.

   工作管理員可以存取嗎？為什麼不行？

   使用這些簡單的策略，使之有可能阻止虛擬機器組之間的流量（如生產和開發），隔離實作室系統，或為法規遵從提供隔離。

刪除策略
.................

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Virtual Infrastructure > Policies > Security Policies**.

#. 選擇 *Initials*- **Isolate-dev-prod** 並按一下 **Actions > Delete**.

#. 在確認對話方塊中輸入 **DELETE**  並按一下 **OK** 以禁用該策略.

   .. note::

     要禁用該策略，你可以選擇進入 **Monitor** 模式, 而不是完全地刪除該策略.

#. 返回 *Initials*\ **-WinClient-0** 控制台並驗證可以再次從瀏覽器中存取工作管理員。

概要總結
+++++++++

- 在本練習中，您輕鬆創建了類別和環境隔離安全性原則，而無需更改或更改任何網路配置。
- 用創建的類別標記虛擬機器後，虛擬機器會根據其所屬的策略運行。
- 環境隔離策略的優先順序高於應用程式安全性原則的優先順序，並阻止應用程式安全性原則允許的流量。

