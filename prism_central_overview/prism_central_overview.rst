.. _prism_central_overview:

-----------------------
Prism Central: 概述
-----------------------

Overview
++++++++

實作將介紹 Prism Central UI, 並熟悉它的佈局和導航。

Prism Central
+++++++++++++

#. 打開 https://<*Prism-Central-IP*>:9440

#. 填寫以下欄位並按一下 **Enter**:

- **Username** - admin
- **Password** - *HPOC Password*

#. 在登錄Prism Central後, 請自己熟悉Prism UI.

#. 瀏覽 **Home** 介面的有關資訊:

- Cluster Runway
- Cluster Quick Access
- Impacted Cluster | Alerts
- tasks

#. 瀏覽 **Explore** 介面:

- VMs
- Images
- Clusters
- Hosts
- Disks
- Storage Containers

.. figure:: images/nutanix_tech_overview_10.png

#. 查看其他部分，並快速瀏覽:

- Planning
- Analysis
- Apps (We will configure this later in the workshop)
- Alerts
- Tasks :fa:`circle-o`
- Search :fa:`search`
- Help :fa:`question`
- Configuration :fa:`cog`
- User :fa:`user`

.......................
Prism Central UI 瀏覽
.......................

如何找到被一個Prism Central實例管理的所有主機頁面?

.. figure:: images/nutanix_tech_overview_11.png

.. note::

  如果這個Prism Central 實例管理了多個集群, 這個頁面會顯示被管理的所有機器的主機。

#. 在 **Prism Central > Explore**, 點擊左手菜單的 **Hosts**.

你如何找到顯示所有已部署的VMs頁面. 這個頁面是否和下圖類似?

.. figure:: images/nutanix_tech_overview_12.png

#. 在 **Prism Central > Explore** , 點擊左手菜單的 **VMs** .

哪個頁面會顯示系統中的最新活動？ 在此頁面上，您可以監控任何任務的進度，並使用時間戳記跟蹤過去所執行的操作。 你能想出兩種不同的方法嗎？

#. 第一種方法, 在 **Prism Central > Home**, 點擊 **View All Tasks**. 第二種方法, 點擊 :fa:`circle-o`


概要總結
+++++++++

- Prism 是一個全面的UI介面
- 關鍵資訊顯示在正中明顯位置。
- Prism Central 可以管理多個集群
