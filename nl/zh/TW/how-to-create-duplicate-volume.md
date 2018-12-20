---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 建立重複的區塊磁區

您可以建立現有 {{site.data.keyword.blockstoragefull}} 的重複項目。依預設，重複磁區會繼承原始磁區的容量及效能選項，而且會有到達 Snapshot 中該時間點之前的資料副本。   

因為重複磁區的基礎是時間點 Snapshot 中的資料，所以原始磁區上需要有 Snapshot 空間，您才能建立重複磁區。如需 Snapshot 以及如何訂購 Snapshot 空間的相關資訊，請參閱 [Snapshot 文件](snapshots.html)。  

您可以從**主要**及**抄本**磁區建立重複磁區。新的重複磁區會建立在與原始磁區相同的資料中心內。如果您建立抄本磁區的重複磁區，則新的磁區會建立在與抄本磁區相同的資料中心內。

佈建儲存空間之後，主機就可以存取重複磁區來進行讀寫。不過，除非從原始磁區到重複磁區的資料複製已完成，否則不容許進行 Snapshot 及抄寫。

資料複製完成時，就可以管理重複磁區，並用來作為完全無關的磁區。

此特性適用於大部分位置。如需可用的資料中心清單，請按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)。

如果您是 {{site.data.keyword.containerlong}} 的「專用」帳戶使用者，請參閱 [{{site.data.keyword.containerlong_notm}} 文件](/docs/containers/cs_storage_file.html#backup_restore)中您用於複製磁區的選項。
{:tip}

重複磁區的一些常見用途：
- **災難回復測試**。建立抄本磁區的重複磁區，驗證資料是完整的，而且可以在發生災難時使用，而不岔斷抄寫。
- **正式副本**。使用儲存空間磁區作為正式副本，您可以從正式副本建立多個實例以進行各種用途。
- **重新整理資料**。建立正式作業資料副本，以裝載至非正式作業環境進行測試。
- **從 Snapshot 還原**。從 Snapshot 還原具有特定檔案和日期之原始磁區上的資料，而不使用 Snapshot 還原功能來改寫整個原始磁區。
- **開發及測試 (dev/test)**。一次最多可同時建立磁區的四個重複磁區，以建立重複資料來進行開發及測試。
- **調整儲存空間大小**。建立具有新大小及（或）IOPS 速率的磁區，而不需要移動資料。  

有幾種方法可讓您透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 來建立重複磁區。


## 建立儲存空間清單中特定磁區的重複磁區

1. 移至您的 {{site.data.keyword.blockstorageshort}} 清單：
    - 從客戶入口網站，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或
    - 從 {{site.data.keyword.BluSoftlayer_full}} 主控台，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.blockstorageshort}}**。
2. 從清單中選取磁區，然後按一下**動作** > **複製 LUN（磁區）**。
3. 選擇 Snapshot 選項：
    - 如果您從**非抄本**磁區訂購，
      - 選取**從新 Snapshot 建立** - 此動作會建立將用於重複磁區的 Snapshot。如果您的磁區目前沒有任何 Snapshot，或您要建立該時間點的重複項目，則請使用此選項。<br/>
      - 選取**從最新 Snapshot 建立** - 此動作會從此磁區的現有最新 Snapshot 建立重複項目。
    - 如果從**抄本**磁區訂購，Snapshot 的唯一選項是使用可用的最新 Snapshot。
4. 「儲存空間類型」和「位置」會維持與原始磁區相同。
5. 按小時或按月計費 - 您可以選擇佈建按小時或按月計費的重複 LUN。會自動選取原始磁區的計費類型。如果您想要為重複儲存空間選擇不同的計費類型，則可以在這裡進行該選擇。
5. 需要時，您可以指定新磁區的 IOPS 或「IOPS 層級」。依預設，會設定原始磁區的 IOPS 指定。會顯示可用的效能與大小組合。
    - 如果您的原始磁區是 0.25 IOPS 的「耐久性」層級，則無法進行新的選擇。
    - 如果您的原始磁區是 2、4 或 10 IOPS 的「耐久性」層級，則可以將新磁區移到這些層級之間的任何位置。
6. 您可以更新新磁區的大小，讓它大於原始磁區。依預設，會設定原始磁區的大小。

   {{site.data.keyword.blockstorageshort}} 可以調整為磁區原始大小的 10 倍。
   {:tip}
7. 您可以更新新磁區的 Snapshot 空間，以新增更多、更少 Snapshot 空間，或不新增 Snapshot 空間。依預設，會設定原始磁區的 Snapshot 空間。
8. 按一下**繼續**，以下訂單。



## 建立特定 Snapshot 的重複項目

1. 移至您的 {{site.data.keyword.blockstorageshort}} 清單：
2. 按一下清單中的 LUN，以檢視詳細資料頁面。（它可以是抄本或非抄本磁區。）
3. 向下捲動並從詳細資料頁面的清單中選取現有 Snapshot，然後按一下**動作** > **複製**。   
4. 「儲存空間類型」（「耐久性」或「效能」）及「位置」會維持與原始磁區相同。
5. 會顯示可用的效能與大小組合。依預設，會設定原始磁區的 IOPS 指定。您可以指定新磁區的 IOPS 或「IOPS 層級」。
    - 如果您的原始磁區是 0.25 IOPS 的「耐久性」層級，則無法進行新的選擇。
    - 如果您的原始磁區是 2、4 或 10 IOPS 的「耐久性」層級，則可以將新磁區移到這些層級之間的任何位置。
6. 您可以更新新磁區的大小，讓它大於原始磁區。依預設，會設定原始磁區的大小。

   {{site.data.keyword.blockstorageshort}} 可以調整為磁區原始大小的 10 倍。
   {:tip}
7. 您可以更新新磁區的 Snapshot 空間，以新增更多、更少 Snapshot 空間，或不新增 Snapshot 空間。依預設，會設定原始磁區的 Snapshot 空間。
8. 按一下**繼續**，以訂購重複項目。


## 管理重複磁區

當資料從原始磁區複製到重複磁區時，您會在詳細資料頁面上看到一個顯示正在進行複製的狀態。在此期間，您可以連接主機，並且讀取/寫入磁區，但無法建立 Snapshot 排程。複製處理程序完成之後，新的磁區即與原始磁區無關，而且可以如常使用 Snapshot 及抄寫進行管理。
