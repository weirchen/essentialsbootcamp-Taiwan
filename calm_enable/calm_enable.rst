.. _calm_enable：

------------
Calm：啟用
------------

概述
++++++++

.. note:: 參考：`calm_的基礎知識`，然後繼續實作，熟悉Nutanix Calm中使用的UI和常用術語。

預計完成時間：** 10分鐘**

在本練習中，您將啟用Nutanix Calm。

啟用應用程式管理
+++++++++++++++++++++++

Calm內置於Prism Central，無需額外的設備或控制台即可管理。在使用Calm開始在您的環境中管理應用程式之前，必須啟用該服務。

.. note:: 每個Prism Central實例只能啟用一次Calm。如果**啟用應用程式管理**旁邊顯示綠色核取記號，則表示已為正在使用的Prism Central實例啟用了Calm。繼續：ref：`calm_projects`。

#. 在 **Prism Central** 中, 點擊 **?** 的下拉式功能表, 在Prism Central中展開 **New** 並且選擇 **Enable app management**.

#. 按一下 **Enable**。

.. figure :: images / 510enable1.png

#. 選擇 **Enable App Management**，然後按一下 **Save**。

.. note:: Nutanix Calm是一個單獨軟體授權的產品，可以與Acropolis Starter，Pro或Ultimate版本一起使用。在需要額外軟體授權之前，每個Prism Central實例可以免費管理多達25個VM。

.. figure :: images / 510enable2.png

#. 你應該得到Calm正在啟用的確認，這需要5到10分鐘。

.. figure :: images / 510enable3.png

添加Active Directory
+++++++++++++++++++++++

當我們等待Calm啟用時，我們將添加一個Active Directory伺服器。雖然Calm基本使用不需要這樣做，但是需要進行任何基於角色的存取控制，因此最好進行設置。

#.  按一下 **齒輪圖示** 然後 **Authentication**。

.. figure :: images / 510enable4.png

#. 在快顯視窗中，按一下 **New Directory**。

.. figure :: images / 510enable5.png

#. 填寫以下欄位，然後按一下 **Save**：

- **Directory Type** - Active Directory
- **Name** - NTNXLAB
- **Domain** - ntnxlab.local
- **Directory URL** - ldaps://*<DC-VM-IP>*
- **Search Type** - Non Recursive
- **Username** - Administrator@ntnxlab.local
- **Password** - nutanix/4u

.. figure :: images / 510enable6.png

#. 刷新瀏覽器並從巡覽列中選擇 ** Calm **。如果Calm仍在啟用，請等待一分鐘，然後重試。

.. figure:: images/510enable7.png
