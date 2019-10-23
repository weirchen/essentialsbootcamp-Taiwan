.. _flow_quarantine_vm:

-------------------
Flow: 檢疫隔離虛擬機器（Quarantine）
-------------------

*完成本實作預計需要10分鐘.*

概述
++++++++

檢疫隔離可以分配虛擬機器受限的策略，為管理員提供了要麼阻止所有流量或允許部分子網流量的一個選項。嚴格檢疫隔離（Strict）阻止虛擬機器的所有通信，而取證檢疫隔離（Forensic）則允許預定義列表的進站和出站流量。

檢疫隔離虛擬機器
+++++++++++++++++

在本任務中，我們會把一台虛擬機器做檢疫隔離，並觀察這台虛擬機器的行為。我們也將檢查檢疫隔離策略裡面的可配置選項。

#. 返回 *Initials*\ **-WinClient-0** 控制台.

#. 打開 **Command Prompt** 並運行 ``ping -t HAPROXY-VM-IP`` 以驗證用戶端與負載平衡器之間的連通性。

   .. note::

     如果PING測試不成功，你可能需要將 **Environment:Dev** 的入站規則更新為 **AppTier:**\ *Initials*-**TMLB** ，把 **ICMP** 流量的 **Type** 和 **Code** 置為 **Any** ，如下圖所示。應用更新後的 **AppTaskMan-**\ *Initials* 策略，PING應該可以成功。

     .. figure:: images/41.png

#. 在 **Prism Central > Virtual Infrastructure > VMs**, 選擇 *Initials*\ **-HAPROXY-0...** VM.

#. 按一下 **Actions > Quarantine VMs**.

   .. figure:: images/42.png

#. 選擇 **Forensic** 並點擊 **Quarantine**.

   用戶端和負載平衡器之間連續ping會怎樣？ 您可以從用戶端虛擬機器存取工作管理員應用程式網頁嗎？

#. 在 **Prism Central**, 選擇 :fa:`bars` **> Virtual Infrastructure > Policies > Security Policies > Quarantine** 以查看所有檢疫隔離的虛擬機器。

#. 點擊 **Update** 以編輯檢疫隔離策略（Quarantine policy）.

   為了說明Flow此特殊策略的功能，您將用戶端虛擬機器添加為“取證工具”。 在生產環境中，允許對檢疫隔離虛擬機器進行入站存取的虛擬機器可用於運行安全性和取證套件，例如Kali Linux或SANS SIFT。

#. 在 **Inbound** 下, 點擊 **+ Add Source**.

#. 填寫以下欄位:

   - **Add source by:** - 選擇 **Subnet/IP**
   - 指定 *Your WinClient VM IP*\ /32

   這個來源端可以連接到哪些目標端呢？取證（Forensic）和嚴格（Strict）檢疫隔離模式有什麼區別呢？

   請注意，把虛擬機器添加到 **Strict** 檢疫隔離策略後，會禁止所有與虛擬機器間的進站和出站通信。 **Strict** 策略適用於網路上對環境產生威脅的虛擬機器。

#. 點擊 :fa:`plus-circle` 圖示左邊的 **Quarantine: Forensic** 來創建進站規則.

#. 按一下 **Save** 以允許客戶虛擬機器與 **Quarantine: Forensic** 類別之間的任意埠上的任意協議。

   .. figure:: images/43.png

#. 按一下 **Next > Apply Now** 以保存和應用更新的策略。

   添加來源後，與負載平衡器之間的PING會發生什麼呢？你可以存取工作管理員Web應用嗎？ 

#. 通過選定Prism Central中的虛擬機器，按一下 **Actions > Unquarantine VMs** ，你可以從 **Quarantine: Forensic** 類別中移除負載平衡器虛擬機器。

概要總結
+++++++++

- 本練習中，你使用Flow來檢疫隔離的兩種模式來隔離一台虛擬機器（Strict嚴格和Forensic取證）
- 檢疫隔離的策略的優先順序高於應用策略。檢疫隔離可以阻止被應用策略所允許的流量。
- 當虛擬機器被檢疫隔離時，取證檢疫模式（Forensic）是允許對其有限存取的關鍵。

