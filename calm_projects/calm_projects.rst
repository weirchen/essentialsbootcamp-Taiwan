.. _calm_projects:

------------
Calm: Projects
------------

簡介
++++++++

.. note::

  在繼續練習之前閱讀 :ref:`calm_basics` ，您需要熟悉Nutanix Calm中使用的UI和常用術語。

  預計完成時間：**15 MINUTES**

在本練習中，您將配置一個包含在整個Bootcamp中創建的藍圖和應用程式Project。

生成一個Project
++++++++++++++++++

Projects 是將Calm與Nutanix的內置自助服務入口網站（SSP）功能整合的邏輯結構，允許管理員將基礎結構資源以及Active Directory使用者/組的角色/許可權分配給特定的藍圖和應用程式。
#. 在Calm UI介面內, 從邊欄選擇|proj-icon| **Projects**。

   .. figure:: images/510projects1.png

#. 點擊 **+ Create Project**

#. 填寫以下欄位：

   - **Project Name** - *initials*-Calm
   - **Description** - *initials*-Calm

#. 在 **Users, Groups, and Roles**下面, 點擊 **+ User**.

#. 填寫以下欄位，然後按一下 **Save**:

   - **Name** - SSP Admins
   - **Role** - Project Admin

#. 按一下 **+ User**, 填寫以下欄位，然後按一下 **Save**:

   - **Name** - SSP Developers
   - **Role** - Developer

#. 按一下 **+ User**, 填寫以下欄位，然後按一下 **Save**:

   - **Name** - SSP Power Users
   - **Role** - Consumer

#. 按一下 **+ User**, 填寫以下欄位，然後按一下 **Save**:

   - **Name** - SSP Basic Users
   - **Role** - Operator

   .. figure:: images/projects_name_users.png

#. 在 **Infrastructure**下, 按一下藍色的 **Select Provider** 按鈕, 之後是 **Nutanix**.

#. 在出現的框中，按一下白色 **Select Clusters & Subnets** 按鈕, 並在快顯視窗中，選擇您的AHV群集。 選擇集群後，選擇 **Primary** 網路，如果有的話，再選擇 **Secondary**網路，然後按一下 **Confirm**.

   .. figure:: images/projects_cluster_subnet_selection.png

#. 在 **Selected Subnets** 表內, 為 **Primary**網路選擇 :fa:`star`  使其成為 **Calm** project裡虛擬機器中的預設虛擬網路 

   .. figure:: images/projects_infrastructure.png

#. 按一下 **Save**.

.. note::

  按一下 `here <https://portal.nutanix.com/#/page/docs/details?targetId=Nutanix-Calm-Admin-Operations-Guide-v56:nuc-roles-responsibility-matrix-c.html>`_ 查看默認SSP角色和相關許可權的完整列表。

概要總結
+++++++++

-Nutanix Calm是Nutanix企業雲的完全整合元件。 在Scale Out Prism Central部署中易於啟用，高度可用，並且利用無中斷的一鍵式升級來獲得新功能和修復。

-通過使用分配給不同群集和用戶的不同專案，管理員可以確保每次都以正確的方式部署工作負載。 例如，開發人員可以是開發/測試專案的專案管理員，因此他們可以完全控制部署到其開發集群或雲中，同時具有對生產項目的唯讀存取權限，從而使他們可以存取日誌，但無權 改變生產工作量。

.. |proj-icon| image:: ../images/projects_icon.png
.. |mktmgr-icon| image:: ../images/marketplacemanager_icon.png
.. |mkt-icon| image:: ../images/marketplace_icon.png
.. |bp-icon| image:: ../images/blueprints_icon.png
