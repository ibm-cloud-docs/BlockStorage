---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 訂購 {{site.data.keyword.blockstorageshort}}

根據需求及喜好設定，您可以佈建兩種不同類型的儲存空間。{{site.data.keyword.blockstorageshort}} 磁區的兩個選項如下： 

- **耐久性**：佈建「耐久性」層級，其特色是預先定義的效能層次，以及例如 Snapshot 及抄寫等特性。 
- **效能**：建置您可以配置所需每秒輸入/輸出作業 (IOPS) 的高功率「效能」環境。

## 如何訂購 {{site.data.keyword.blockstorageshort}} 的耐久性

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或從「{{site.data.keyword.BluSoftlayer_full}} 型錄」中，按一下**基礎架構 > 儲存空間 > {{site.data.keyword.blockstorageshort}}**。
2. 在右上角按一下**訂購 {{site.data.keyword.blockstorageshort}}** 鏈結。
3. 從「選取儲存空間類型」下拉清單中，選取**耐久性**。
4. 點選下拉清單，然後選取您的部署**位置**（資料中心）。
   - 確定將新的「儲存空間」新增至與先前訂購的主機相同的位置。
   - 如果您已選取具有改良功能的資料中心（在下拉清單中會以 * 表示），則可以選擇「每月計費」或「每小時計費」。 
     1. 使用**每小時**計費，會在刪除 LUN 或計費週期結束時（看何者為先），計算區塊 LUN 存在於帳戶上的小時數。如果儲存空間使用期間為幾天或不到一整個月，則每小時計費是一個良好的選擇。每小時計費只適用於這些[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。 
     2. 使用**每月**計費，價格是從建立日期到計費週期結束按比例計算，並立即計費。如果區塊 LUN 在計費週期結束之前遭到刪除，則不會退款。對於正式作業工作負載中使用的儲存空間，每月計費是很好的選項，因為這些正式作業工作負載使用需要長時間（一個月或更長的時間）儲存及存取的資料。**附註**：對於在**未**使用改良功能更新的資料中心內佈建的儲存空間，依預設會使用每月計費類型。
5. 選取應用程式所需之 IOPS 層級旁邊的圓鈕。
    - *每 GB 0.25 IOPS* 是為了低 I/O 強度的工作負載而設計。這些工作負載的特點通常是在給定時間會有大百分比的非作用中資料。應用程式範例包括儲存信箱或部門層次檔案共用。
    - *每 GB 2 IOPS* 是為了大部分的一般用途而設計。應用程式範例包括管理支持小型資料庫的 Web 應用程式或 Hypervisor 的虛擬機器磁碟映像檔。
    - *每 GB 4 IOPS* 是為了較高強度工作負載而設計。這些工作負載的特點通常是在給定時間會有高百分比的作用中資料。應用程式範例包括交易式資料庫及其他效能相關的資料庫。
    - *每 GB 10 IOPS* 是為了最嚴苛的工作負載（例如 NoSQL Database 所建立的工作負載）以及進行分析的資料處理而設計。此層級適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內大小最多佈建 4TB 的儲存空間。
6. 按一下**選取儲存空間大小**下拉清單，然後選取您的儲存空間大小。
7. 按一下**指定 Snapshot 空間大小**下拉清單，然後選取 Snapshot 大小（除了可用空間之外）。如需 Snapshot 空間考量及建議，請閱讀[訂購 Snapshot](ordering-snapshots.html)。
8. 從下拉清單中選擇您的 **OS 類型**。
9. 點選下拉清單，然後選取您的部署「位置」（資料中心）。
10. 按一下**繼續**。您會看到每月及按比例分配的費用，並且會有最後一次機會檢閱訂單詳細資料。
11. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**按鈕。
12. 在幾分鐘之後，應該就可以使用您的新儲存空間配置。

**附註**：依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。請在[這裡](managing-storage-limits.html)閱讀增加限制的相關資訊。

如需同時授權的限制，請參閱[常見問題](BlockStorageFAQ.html)
 
## 如何訂購 Block Storage 的效能

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**，或從「{{site.data.keyword.BluSoftlayer_full}} 型錄」中，按一下**基礎架構 > 儲存空間 > {{site.data.keyword.blockstorageshort}}**。
2. 按一下畫面右上角的**訂購 {{site.data.keyword.blockstorageshort}}**。
3. 從**選取儲存空間類型**下拉清單中，選取**效能**。
4. 按一下**位置**下拉清單，然後選取資料中心。
   - 確定將新的「儲存空間」新增至與先前訂購的主機相同的位置。
   - 如果您已選取具有改良功能的資料中心（在下拉清單中會以 * 表示），則可以選擇「每月計費」或「每小時計費」。 
     1. 使用**每小時**計費，會在刪除 LUN 或計費週期結束時（看何者為先），計算區塊 LUN 存在於帳戶上的小時數。如果儲存空間使用期間為幾天或不到一整個月，則每小時計費是一個良好的選擇。每小時計費只適用於這些[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。 
     2. 使用**每月**計費，價格是從建立日期到計費週期結束按比例計算，並立即計費。如果區塊 LUN 在計費週期結束之前遭到刪除，則不會退款。對於正式作業工作負載中使用的儲存空間，每月計費是很好的選項，因為這些正式作業工作負載使用需要長時間（一個月或更長的時間）儲存及存取的資料。**附註**：對於在**未**使用改良功能更新的資料中心內佈建的儲存空間，依預設會使用每月計費類型。
5. 選取適當**儲存空間大小**旁的圓鈕。
6. 在**指定 IOPS** 欄位中，輸入 IOPS。
7. 按一下**繼續**。您會看到每月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。如果您要變更訂單，請按**上一步**。
8. 按一下**我已閱讀主要服務合約**勾選框，然後按**下訂單**按鈕。
9. 在幾分鐘之後，應該就可以使用您的新儲存空間配置。

**附註**：依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。請在[這裡](managing-storage-limits.html)閱讀增加限制的相關資訊。

如需同時授權的限制，請參閱[常見問題](BlockStorageFAQ.html)

## 如何在發票上識別 {{site.data.keyword.blockstorageshort}}

所有 LUN 在發票上都會顯示為一個行項目；「耐久性」將顯示為「耐久性儲存空間服務」，而「效能」將顯示為「效能儲存空間服務」。費率將根據您的儲存空間層次而有所不同。然後，您可以展開「耐久性」或「效能」，即可看到它是 {{site.data.keyword.blockstorageshort}}。
