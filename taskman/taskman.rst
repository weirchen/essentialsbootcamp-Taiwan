.. _taskman:

----------------------
部署Task Manager
----------------------

*完成本實作的估計時間為10分鐘。*

簡介
++++++++

**此練習將引導您完成導入和啟動Calm藍圖的工作，以部署用於多個實作室的簡單Task Manager應用程式。 您無需完成此練習，除非指示您轉而準備另一個實作室。**

驗證默認 Project
+++++++++++++++++++++++++++++

在 **Prism Central**, 選擇 :fa:`bars` **> Services > Calm**.

.. figure:: images/0.png

在左手工具列點擊 |projects| **Projects** 並選擇 **default** project.

.. note::

  將滑鼠懸停在圖示上將顯示其標題。

在 **AHV Cluster** 下確認從下拉清單中選擇了您分配的集群，否則選擇它。

.. figure:: images/1.png

在 **Network** 下, 驗證 **Primary** 和 **Secondary** 網路都被選擇，**Primary** 網路為預設。 此外, 進行如下所示的選擇。

.. figure:: images/2.png

如果進行了更改，請按一下 **Save**.

導入藍圖
+++++++++++++++++++++++

右擊 :download:`this link <TaskManager.json>` 並 **Save Link As...** 下載本練習中使用的示例應用程式的藍圖。

點擊左側工具列中 |blueprints| **Blueprints** ，以查看可用的Calm藍圖。

點擊 **Upload Blueprint** 並選擇之前下載好的 **TaskManager.json**。

填寫以下欄位

- **Blueprint Name** - *Initials*-TaskManager
- **Project** - default

.. figure:: images/3.png

點擊 **Upload**.

.. note::

  如果您在嘗試上傳藍圖時遇到錯誤，請刷新瀏覽器，然後重試。

配置藍圖
+++++++++++++++++++++++++

在啟動藍圖之前，必須首先提供指定的資訊，這些資訊未存儲在匯出的Calm藍圖中，包括憑證。

在左邊 **Application Profile** 窗格, 填寫以下欄位：

- **Mysql_password** - nutanix/4u

.. figure:: images/4.png

選擇右邊窗格的 **WinClient** 服務, 在 **VM** 選項卡下, 確保 **Image** 設置為 **Windows10** 鏡像，如下所示。

.. figure:: images/4b.png

在 **Network Adapters (NICs)** 下, 確保 **NIC 1** 設置為 **Primary** ，如下所示.

.. figure:: images/4c.png

選擇 **WebServer**, **HAProxy**, 和 **MySQL** 服務並確保 **NIC 1** 設置為 **Primary**.

.. figure:: images/4d.png

點擊 **Save**.

.. figure:: images/5.png

點擊 **Credentials**.

.. figure:: images/6.png

通過點擊其名字擴展 **CENTOS** 認證。 將以下金鑰複製並粘貼到“ SSH私密金鑰”欄位中：

::

  -----BEGIN RSA PRIVATE KEY-----
  MIIEowIBAAKCAQEAii7qFDhVadLx5lULAG/ooCUTA/ATSmXbArs+GdHxbUWd/bNG
  ZCXnaQ2L1mSVVGDxfTbSaTJ3En3tVlMtD2RjZPdhqWESCaoj2kXLYSiNDS9qz3SK
  6h822je/f9O9CzCTrw2XGhnDVwmNraUvO5wmQObCDthTXc72PcBOd6oa4ENsnuY9
  HtiETg29TZXgCYPFXipLBHSZYkBmGgccAeY9dq5ywiywBJLuoSovXkkRJk3cd7Gy
  hCRIwYzqfdgSmiAMYgJLrz/UuLxatPqXts2D8v1xqR9EPNZNzgd4QHK4of1lqsNR
  uz2SxkwqLcXSw0mGcAL8mIwVpzhPzwmENC5OrwIBJQKCAQB++q2WCkCmbtByyrAp
  6ktiukjTL6MGGGhjX/PgYA5IvINX1SvtU0NZnb7FAntiSz7GFrODQyFPQ0jL3bq0
  MrwzRDA6x+cPzMb/7RvBEIGdadfFjbAVaMqfAsul5SpBokKFLxU6lDb2CMdhS67c
  1K2Hv0qKLpHL0vAdEZQ2nFAMWETvVMzl0o1dQmyGzA0GTY8VYdCRsUbwNgvFMvBj
  8T/svzjpASDifa7IXlGaLrXfCH584zt7y+qjJ05O1G0NFslQ9n2wi7F93N8rHxgl
  JDE4OhfyaDyLL1UdBlBpjYPSUbX7D5NExLggWEVFEwx4JRaK6+aDdFDKbSBIidHf
  h45NAoGBANjANRKLBtcxmW4foK5ILTuFkOaowqj+2AIgT1ezCVpErHDFg0bkuvDk
  QVdsAJRX5//luSO30dI0OWWGjgmIUXD7iej0sjAPJjRAv8ai+MYyaLfkdqv1Oj5c
  oDC3KjmSdXTuWSYNvarsW+Uf2v7zlZlWesTnpV6gkZH3tX86iuiZAoGBAKM0mKX0
  EjFkJH65Ym7gIED2CUyuFqq4WsCUD2RakpYZyIBKZGr8MRni3I4z6Hqm+rxVW6Dj
  uFGQe5GhgPvO23UG1Y6nm0VkYgZq81TraZc/oMzignSC95w7OsLaLn6qp32Fje1M
  Ez2Yn0T3dDcu1twY8OoDuvWx5LFMJ3NoRJaHAoGBAJ4rZP+xj17DVElxBo0EPK7k
  7TKygDYhwDjnJSRSN0HfFg0agmQqXucjGuzEbyAkeN1Um9vLU+xrTHqEyIN/Jqxk
  hztKxzfTtBhK7M84p7M5iq+0jfMau8ykdOVHZAB/odHeXLrnbrr/gVQsAKw1NdDC
  kPCNXP/c9JrzB+c4juEVAoGBAJGPxmp/vTL4c5OebIxnCAKWP6VBUnyWliFhdYME
  rECvNkjoZ2ZWjKhijVw8Il+OAjlFNgwJXzP9Z0qJIAMuHa2QeUfhmFKlo4ku9LOF
  2rdUbNJpKD5m+IRsLX1az4W6zLwPVRHp56WjzFJEfGiRjzMBfOxkMSBSjbLjDm3Z
  iUf7AoGBALjvtjapDwlEa5/CFvzOVGFq4L/OJTBEBGx/SA4HUc3TFTtlY2hvTDPZ
  dQr/JBzLBUjCOBVuUuH3uW7hGhW+DnlzrfbfJATaRR8Ht6VU651T+Gbrr8EqNpCP
  gmznERCNf9Kaxl/hlyV5dZBe/2LIK+/jLGNu9EJLoraaCBFshJKF
  -----END RSA PRIVATE KEY-----

點擊其名字擴展 **WIN_VM_CRED** 認證。 輸入 **nutanix/4u** 作為 **Password**.

.. figure:: images/7.png

點擊 **Save**.

藍圖保存後, 點擊 **Back**.

.. figure:: images/8.png

運行藍圖
+++++++++++++++

在提供認證後, **Publish**, **Download**, 和 **Launch** 現在可以從工具列中使用。 點擊 **Launch**.

填寫以下欄位:

- **Name of the Application** - *Initials*-TaskManager1
- **User_initials** - *Initials*

.. figure:: images/9.png

點擊 **Create**.

您可以通過以下方式監視應用程式部署的狀態： |applications| **Applications** 點擊你應用程式的名字。

設置完整的應用程式大約需要15分鐘。 在配置應用程式時，繼續進行實作的下一部分。

.. |projects| image:: images/projects.png
.. |blueprints| image:: images/blueprints.png
.. |applications| image:: images/applications.png
