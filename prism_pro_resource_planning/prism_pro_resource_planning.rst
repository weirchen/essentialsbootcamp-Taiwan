.. _prism_pro_resource_planning:

--------------------------------
Prism Pro: 資源規劃
--------------------------------

簡介
++++++++

本實作將介紹Prism Central的資源規劃功能，幫助您掌握集群利用率並更準確地預測集群擴展。

Lab 創建
+++++++++

該實作需要配置VM，稍後將在實作室中說明生成CPU和記憶體矩陣。

#. 在繼續這個實作之前請按提示部署 :ref:`linux_tools_vm` 


#. 按右鍵以下URL以打開新選項卡，然後導航到http://10.42.247.70上的網頁，並在表單的“設置”部分中輸入詳細資訊。 填寫完所有欄位後，按一下“開始設置”。 這將為您的環境做好準備。 **在整個Prism Pro實作室中保持此選項卡打開，以便按照以後部分的指示返回。**

   .. figure:: images/ppro_08.png

#. 點擊繼續後，設置完成需要一些時間。 在此期間，切換回Prism Central並通過實作室。

Prism Central 資源規劃
+++++++++++++++++++++++++++++++

Nutanix利用我們的X-Fit機器學習和資料分析作為Prism Pro的一部分。 我們利用該機器學習和資料分析來提供Cluster Runway資源倒計時和及時預測（What If規劃）。

容量倒計時
...............

使用Prism Central的資源倒計時瞭解群集資源規劃和建議的功能。

讓我們查看集群的資源倒計時

#. 在 **Prism Central > Planning > Capacity Runway**.

  - 請注意資源倒計時摘要顯示每個群集的剩餘天數。
  - 在記憶體，CPU和儲存空間不足之前，當前集群有多長時間？

#. 點擊 **Prism-Pro-Cluster**集群。

. 您現在可以查看儲存，CPU和記憶體的剩餘資源。

   .. figure:: images/ppro_12.png

#. 選擇“記憶體”選項卡時，您可以看到紅色驚嘆號，指示此群集將耗盡記憶體的位置。 此時您可以將圖表懸停在圖表上，以查看這將發生在哪一天。

   .. figure:: images/ppro_13.png

#. 點擊 **Optimize Resources**左邊的按鈕。 在這裡，您可以看到環境中效率低下的虛擬機器，並提供有關如何優化這些資源以盡可能高效的建議。

   .. figure:: images/ppro_14.png

#. 關閉優化資源快顯視窗。

What If 規劃
................

#. 在頁面左邊的的 **Adjust Resources**下邊, 點擊 **Get Started**按鈕. 我們現在可以使用它來開始規劃新的工作負載，並瞭解將來如何擴展跑道。

#. 點擊左邊 **Workloads**選項下面的 **add/adjust**按鈕。

   .. figure:: images/ppro_15.png

#. 為VDI添加一個並選擇1000個用戶。 您還可以設置應將此工作負載添加到系統的日期。 完成後保存此工作負載。

   .. figure:: images/ppro_16.png

   .. figure:: images/ppro_17.png

#. 添加另一個你選擇的工作負載

#. 點擊右邊頁面的 **Recommend** 按鈕。

   .. figure:: images/ppro_18.png

#. 一旦建議書可用，在清單和圖表視圖之間切換，以更好地瀏覽您的場景。

   .. figure:: images/ppro_19.png

#. 點擊右上角的 **Generate PDF** 按鈕。 這將打開一個新選項卡，其中包含您已創建的方案/工作負載的PDF報告。

   .. figure:: images/ppro_19b.png

#. 查看報告

   .. figure:: images/ppro_20.png

概要總結
+++++++++

 -  Planning儀錶板中的Capacity Runway視圖允許您查看已註冊集群的摘要資源倒計時資訊，並訪問有關每個集群的詳細資源倒計時資訊。
 -  規劃儀錶板中的“方案”視圖允許您創建“假設”方案，以評估您指定的潛在工作負載的未來資源需求。
 -  您必須擁有Prism Pro許可才能使用資源規劃工具。


