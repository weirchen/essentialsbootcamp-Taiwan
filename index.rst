.. title:: Nutanix Essentials Bootcamp

.. toctree::
  :maxdepth: 2
  :caption: Prism Central
  :name: _prism_central
  :hidden:

  what_is_prism_central/what_is_prism_central
  prism_central_overview/prism_central_overview
  prism_central_dashboards_reports/prism_central_dashboards_reports

.. toctree::
  :maxdepth: 2
  :caption: Prism Pro
  :name: _prism_pro
  :hidden:

  what_is_prism_pro/what_is_prism_pro
  prism_pro_efficiency_anomaly/prism_pro_efficiency_anomaly
  prism_pro_resource_planning/prism_pro_resource_planning
  prism_pro_xplay/prism_pro_xplay

.. toctree::
  :maxdepth: 2
  :caption:  Files Labs
  :name: _files_labs
  :hidden:

  files_deploy/files_deploy
  files_smb_share/files_smb_share
  files_nfs_export/files_nfs_export
  file_analytics_deploy/file_analytics_deploy
  file_analytics_scan/file_analytics_scan

.. toctree::
  :maxdepth: 2
  :caption: Calm
  :name: _calm
  :hidden:

  what_is_calm/what_is_calm
  calm_enable/calm_enable
  calm_projects/calm_projects
  calm_linux/calm_linux
  calm_win/calm_win

.. toctree::
  :maxdepth: 2
  :caption: Flow
  :name: _flow
  :hidden:

  what_is_flow/what_is_flow
  flow_enable/flow_enable
  flow_secure_app/flow_secure_app
  flow_isolate_environments/flow_isolate_environments
  flow_quarantine_vm/flow_quarantine_vm

.. toctree::
  :maxdepth: 2
  :caption: Optional Labs
  :name: _optional_labs
  :hidden:



.. toctree::
  :maxdepth: 2
  :caption: Appendix
  :name: _appendix
  :hidden:

  appendix/glossary
  appendix/basics
  tools_vms/windows_tools_vm
  tools_vms/linux_tools_vm_cloud-init
  taskman/taskman

.. _getting_started:

---------------
開始實作
---------------

歡迎來到Nutanix Essentials訓練營！ 本實作手冊伴隨著講師指導的培訓，介紹了Nutanix解決方案和許多常見的管理任務。 每個部分都有一個課程和實作，為您提供實踐練習。 講師會講解練習並回答您可能遇到的任何其他問題。

在訓練營結束時，與會者應該瞭解Nutanix企業雲平台的基本概念和技術，並且應該為託管或現場概念驗證（POC）參與做好充分準備。


What's New
++++++++++

- Workshop updated for the following software versions:
    - AOS & PC 5.11

- 可選的實作更新:


日程
++++++

- 簡介
- Prism Pro
- Files
- Nutanix Calm
- Nutanix Flow

簡介
+++++++++++++

- 名稱
- 熟悉 Nutanix

設置介紹
+++++++++++++

- 記下使用的密碼。
- 登錄虛擬桌面（基於下面的連接資訊）

環境說明
+++++++++++++++++++

Nutanix Workshop旨在Nutanix Hosted POC環境中運行。 將為您的群集配置完成練習所需的所有必要圖像，網路和VM。


網路
..........

Hosted POC 集群遵循標準命名約定:

- **Cluster Name** - POC\ *XYZ*
- **Subnet** - 10.**21**.\ *XYZ*\ .0
- **Cluster IP** - 10.**21**.\ *XYZ*\ .37

If provisioned from the marketing pool:
- **Cluster Name** - MKT\ *XYZ*
- **Subnet** - 10.**20**.\ *XYZ*\ .0
- **Cluster IP** - 10.**20**.\ *XYZ*\ .37

示例:

- **Cluster Name** - POC055
- **Subnet** - 10.21.55.0
- **Cluster IP** - 10.21.55.37

在整個Workshop期間，有多個實例需要用* XYZ *替換正確的子網路，例如:

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - IP Address
     - Description
   * - 10.21.\ *XYZ*\ .37
     - Nutanix Cluster Virtual IP
   * - 10.21.\ *XYZ*\ .39
     - **PC** VM IP, Prism Central
   * - 10.21.\ *XYZ*\ .40
     - **DC** VM IP, NTNXLAB.local Domain Controller

每個群集配置有2個可用於VM的VLAN:

.. list-table::
  :widths: 25 25 10 40
  :header-rows: 1

  * - Network Name
    - Address
    - VLAN
    - DHCP Scope
  * - Primary
    - 10.21.\ *XYZ*\ .1/25
    - 0
    - 10.21.\ *XYZ*\ .50-10.21.\ *XYZ*\ .124
  * - Secondary
    - 10.21.\ *XYZ*\ .129/25
    - *XYZ1*
    - 10.21.\ *XYZ*\ .132-10.21.\ *XYZ*\ .253

認證
...........

.. note::

  The *<Cluster Password>* 對每個群集都是唯一的，將由Workshop的負責人提供.

.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Credential
     - Username
     - Password
   * - Prism Element
     - admin
     - *<Cluster Password>*
   * - Prism Central
     - admin
     - *<Cluster Password>*
   * - Controller VM
     - nutanix
     - *<Cluster Password>*
   * - Prism Central VM
     - nutanix
     - *<Cluster Password>*

每個群集都有一個專用的網域控制站VM, **DC**, 負責為 **NTNXLAB.local** 網域提供AD服務. 該網域包括了以下用戶和群組：:


.. list-table::
   :widths: 25 35 40
   :header-rows: 1

   * - Group
     - Username(s)
     - Password
   * - Administrators
     - Administrator
     - nutanix/4u
   * - SSP Admins
     - adminuser01-adminuser25
     - nutanix/4u
   * - SSP Developers
     - devuser01-devuser25
     - nutanix/4u
   * - SSP Power Users
     - poweruser01-poweruser25
     - nutanix/4u
   * - SSP Basic Users
     - basicuser01-basicuser25
     - nutanix/4u

存取說明
+++++++++++++++++++

可以通過多種不同方式存取Nutanix Hosted POC環境:

Parallels VDI
.................

Login to: https://xld-uswest1.nutanix.com (for PHX) or https://xld-useast1.nutanix.com (for RTP)

**Nutanix Employees** - Use your NUTANIXDC credentials
**Non-Employees** - **Username:** POCxxx-User01 (up to POCxxx-User20), **Password:** *<Provided by Instructor>*




Nutanix 版本資訊
++++++++++++++++++++

- **AHV Version** - AHV 20170830.279 (5.10+)
- **AOS Version** - 5.11
- **PC Version** - 5.11
