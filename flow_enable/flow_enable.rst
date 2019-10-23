.. _flow_enable:

-------------
Flow: Enable
-------------

*完成本實作需要大約10分鐘。*

概述
++++++++

本練習中，你需要啟用Flow.

啟用 Flow
++++++++++++++++++++++++++

Flow內置於Prism Central中，不需要額外的應用或控制台來管理。在開始用Flow保護你的環境之前，必須先啟用該服務。

.. note::

  每個Prism Central實例，Flow只能啟用一次。 如果 **Microsegmentation** 旁邊有顯示綠色核取方塊，這意味著對於該Prism Central的Flow服務已經被啟用。前往:ref:`flow_secure_app`.
  

#. 在 **Prism Central**, 點擊 **?** 下拉式功能表並選擇 **Microsegmentation**.

   .. figure:: images/10.png
   
   啟用Flow，每個Prism Central虛擬機器需要額外的1GB記憶體，但對於使用者來說不需要額外的操作，這個操作是自動的。此外，啟用流程會驗證每台AHV主機至少有1GB的可用RAM。在啟用視窗中會羅列具有Flow功能的AHV集群。

#. 選擇 **Enable Microsegmentation** 並點擊 **Enable**.

   .. figure:: images/11.png

概要總結
+++++++++

- 微分割，Flow的一部分，是一個非集中式的安全實施框架，由Prism Central集中式管理。
- 集群中啟用Flow後，即可通過Prism Central管理介面創建安全性原則輕鬆保護虛擬機器。安全性原則是基於簡單文本的類別來定義虛擬機器組。這些功能用作標籤，可以輕鬆將其應用於虛擬機器，而無需任何其他網路設置。
