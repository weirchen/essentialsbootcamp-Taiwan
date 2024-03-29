-------------
詞彙表
-------------

Nutanix Core
++++++++++++

AOS
...

AOS代表Acropolis作業系統，它是在Controller VM（CVM）上運行的OS。

Pulse
.....

Pulse 向Nutanix客戶支援團隊提供診斷系統資料，以便他們可以為Nutanix解決方案提供主動的，基於上下文的支援。

Prism Element
.............

Prism Element是Nutanix的本地管理平台。 由於其設計基於消費產品介面，因此與許多企業應用程式介面相比，它更直觀，更易於使用。

Prism Central
.............

Prism Central是Nutanix的多雲控制和管理介面。 Prism Central可以管理多個Nutanix群集，並充當監控和分析的聚合點。

Node
....

業界標準的x86伺服器，帶有伺服器附加的SSD和可選的HDD（全快閃記憶體和混合選項）。

Block
.....

2U機架式機箱，包含1個，2個或4個節點，具有共用的電源和風扇，沒有共用的背板。

Storage Pool
............

儲存池是一組物理存放裝置，包括用於群集的PCIe SSD，SSD和HDD設備。

Storage Container
.................

容器是用於實施儲存策略的可用儲存的子集。

Anatomy of a Read I/O
.....................

 效能和可用性

-本地讀取資料
-僅當本地不存在資料時才進行遠端存取

Anatomy of a Write I/O
......................

 效能和可用性

-資料寫入本地
-在其他節點上複製以實現高可用性
-副本分佈在整個群集中以實現高效能

Nutanix flow
++++++++++++

Application Security Policy
...........................

當您想通過指定允許的流量源和目的地來保護應用程式安全時，請使用應用程式安全性原則。

Isolation Environment Policy
............................

當您要阻止按類別標識的兩組VM之間的所有流量（無論方向如何）時，請使用隔離環境策略。 組中的VM可以相互通信。

Quarantine Policy
.................

當您想要隔離受到感染或感染的VM並選擇對其進行取證時，請使用隔離策略。 您不能修改此策略。 隔離VM的兩種模式為“嚴格”或“取證”。

Strict：要阻止所有入站和出站流量時，請使用此值。

Forensic：要阻止除包含取證工具的類別之間的流量以外的所有入站和出站流量，請使用此值。

AppTier
.......

將應用程式中的層（例如，web，application_logic和資料庫）的值添加到此類別，並在配置安全性原則時使用這些值將應用程式劃分為層。

AppType
.......

將應用程式中的VM與適當的內置應用程式類型（例如Exchange和Apache_Spark）相關聯。 您也可以更新類別以為未在此類別中列出的應用程式添加值。

Environment
...........

分別設置要彼此隔離的環境的值，然後將VM與這些值相關聯。
