.. _prism_pro_xplay:

--------------------------------------------
Prism Pro: X-Play
--------------------------------------------

簡介
++++++++



通過X-Play增加受限VM的記憶體 
+++++++++++++++++++++++++

在本實作中，我們現在將使用X-Play創建一個Playbook，以便在檢測到記憶體不足時自動將記憶體添加到之前創建的VM中。

#. 使用搜索欄導航到 **Playbooks**頁面。

   .. figure:: images/ppro_26.png

#. 我們將從創建Playbook開始。 按一下表視圖頂部的 **Create Playbook**。

   .. figure:: images/ppro_27.png

#. 選擇Alert作為觸發器。

   .. figure:: images/ppro_28.png

#. 搜索並選擇 **VM {vm_name} Memory Constrained** 作為警報策略，因為這是我們希望採取自動化步驟進行修復的問題。

   .. figure:: images/ppro_29.png

#. 選擇 *Special VM* 選項按鈕，然後選擇您為實作創建的VM。 這將使得只有在您的VM上引發的警報才會觸發此Playbook。

   .. figure:: images/ppro_29b.png

#. 我們首先需要對VM進行快照。 按一下左側的 **Add actiion**，然後選擇 **VM Snapshot**操作。

   .. figure:: images/ppro_30.png

#. 目標VM從Alert觸發器自動填充源實體。 要完成此操作的詳細資訊填寫，請在生存時間欄位中輸入值，例如**1**。

   .. figure:: images/ppro_32.png

#. 接下來，我們希望通過向VM添加更多記憶體來修復受約束的記憶體。 按一下 **Add Action**以添加 **VM Add Memory**操作。

   .. figure:: images/ppro_33.png

#. 根據下面的螢幕設置空欄位。

   .. figure:: images/ppro_34.png


#. 接下來，我們想通知某人已採取自動操作。 按一下 **Add Action**以添加電子郵件操作

   .. figure:: images/ppro_35.png

#. 填寫電子郵件操作中的欄位。 以下是示例

**Recipient:** 填寫您的電子郵寄地址。

**Subject :**
``Playbook {{playbook.playbook_name}} addressed alert {{trigger[0].alert_entity_info.name}}``

**Message:**
``Prism Pro X-FIT detected  {{trigger[0].alert_entity_info.name}} in {{trigger[0].source_entity_info.name}}.  Prism Pro X-Play has run the playbook of "{{playbook.playbook_name}}". As a result, Prism Pro increased 1GB memory in {{trigger[0].source_entity_info.name}}.``

歡迎您撰寫自己的主題資訊。 以上只是一個例子。 您可以使用“參數”來豐富消息。

   .. figure:: images/ppro_36.png

#. 點擊 **Add Action** 添加 **Acknowledge Alert** 動作。

   .. figure:: images/ppro_37.png

#. 點擊 **Save & Close** 按鈕並使用名稱“*Initials* - Auto Increase Constrained VM Memory”. **務必啟用‘Enabled’ 切換.**

   .. figure:: images/ppro_39.png

#. 你應該在“Playbooks”清單頁面看到一個新的Playbook。

   .. figure:: images/ppro_40.png

#. 搜索您的VM並記錄當前的記憶體容量。 您可以在屬性小部件中向下滾動以查看配置的記憶體。

   .. figure:: images/ppro_41.png

#. **Switch tabs back to** the http://10.42.247.70 page and continue to the Story 1-3 Step.

   .. figure:: images/ppro_66.png

#. 現在我們將模擬“VM Memory Constrained”的警報，它將觸發我們剛剛創建的Playbook。 按一下“Simulate Alert”按鈕以創建警報。

   .. figure:: images/ppro_64.png

#. 返回Prism頁面並再次檢查您的VM頁面，您現在應該看到記憶體容量增加了1GB。 如果記憶體未顯示更新，則可以刷新流覽器頁面以加快進程。

#. 您還應該收到一封電子郵件。 查看電子郵件，查看其主題和電子郵件正文已填寫您設置的參數的實際值。

#. 轉到 **Playbook** 頁面，按一下剛剛創建的劇本。

   .. figure:: images/ppro_44.png

#. 按一下**Plays**選項卡，您應該看到播放剛剛完成。

   .. figure:: images/ppro_45.png

#. 按一下“Play”以檢查詳細資訊。

   .. figure:: images/ppro_46.png


X-Play和3rd Party API結合使用
+++++++++++++++++++++++++++++++++++++++++++++

我們將使用Habitica展示如何在X-Play中使用3rd Party API。 Habitica是一款免費的習慣和生產力應用程式，可將您的現實生活像遊戲一樣對待。 我們將與Habitica共同創建任務。

#. 使用搜索欄導航到**Playbooks**頁面。

   .. figure:: images/ppro_26.png

#. 我們將首先創建一個Playbook。 點擊表格視圖頂部的**Create Playbook**。
   .. figure:: images/ppro_27.png

#. 使用搜索欄導航到 **Action Gallery** 頁面。

   .. figure:: images/ppro_47.png

#. 點擊“ Rest API”項旁邊的核取方塊，然後從操作功能表中選擇“複本”選項。

   .. figure:: images/ppro_48.png

#. 我們正在創建一個動作，以後可以在我們的劇本中使用它在Habitica中創建任務。 在 <YOUR NAME HERE>部分中填寫以下值來替換您的名字。

**Name:** *Initials* - Create Habitica Task

**Method:** POST

**URL:** https://habitica.com/api/v3/tasks/user

**Request Body:** ``{"text":"*Initials* Check {{trigger[0].source_entity_info.name}}","type":"todo","notes":"VM has been detected as a bully VM and has been temporarily powered off.","priority":2}``

**Request Header:**

| x-api-user:fbc6077f-89a7-46e1-adf0-470ddafc43cf
| x-api-key:c5343abe-707a-4f7c-8f48-63b57f52257b
| Content-Type:application/json;charset=utf-8


   .. figure:: images/ppro_49.png

#. 按一下 **copy**按鈕以保存操作。

#. 使用搜索欄導航回到Playbooks頁面。

#. 選擇 **Alert trigger**並搜索並選擇警報策略**VM Bully {vm_name}**。 這是我們希望在系統檢測到Bully VM時採取的警報。

   .. figure:: images/ppro_50.png

#. 選擇 **Specify VMs**選項按鈕，然後選擇您為實作室創建的虛擬機器。 這樣一來，只有在您的VM上發出的警報才會觸發此Playbook。

   .. figure:: images/ppro_50b.png

#. 我們要做的第一件事是關閉虛擬機器電源，因此我們可以確保它不會耗盡其他虛擬機器的資源。 按一下 **Add Action**按鈕，然後選擇 **Power Off VM**。

   .. figure:: images/ppro_51.png

#. 接下來，我們想創建一個任務，以便我們調查導致此VM成為欺負者的原因。 添加另一個動作。 這次選擇您創建的名為“創建Habitica任務”的操作。

   .. figure:: images/ppro_53.png

#. 再添加一個動作，選擇“確認警報”動作。 使用此操作的參數來填寫“警告”參數。

   .. figure:: images/ppro_54.png

#. 保存並啟用playbook。 您可以將其命名為“*Initials* - Power Off Bully VM for Investigation”。 **請確保啟用‘Enabled’開關**，點擊“保存”按鈕。

   .. figure:: images/ppro_55.png

#. **切換回另一個tab** 運行 http://10.42.247.70 並模擬故事5的‘VM Bully Detected’警報。

   .. figure:: images/ppro_65.png

#. 成功模擬警報後，您可以檢查Playbook是否已運行，並像以前一樣查看詳細資訊。

   .. figure:: images/ppro_75.png

#. 您可以如下許可權通過從以下位置的另一個選項卡登錄來驗證Rest API是否為Habitica所調用 https://habitica.com:

| Username : next19LabUser
| Password: Nutanix.123

And verify your task is created.

   .. figure:: images/ppro_57.png

概要總結
+++++++++

-X-Play 企業的IFTTT，是我們實現日常操作任務自動化的引擎。
-X-Play 使管理員可以在數分鐘內自信地自動化其日常任務。
-X-Play 廣泛，可以將客戶現有的API和腳本用作其劇本的一部分。
