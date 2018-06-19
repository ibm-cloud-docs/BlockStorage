---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 訂購 Snapshot

若要自動或手動建立儲存空間磁區的 Snapshot，您需要購買空間來保留它們。您可以購買最多達到儲存空間磁區量的容量（在起始磁區購買期間購買，或之後使用本文章中所述的步驟購買）。

1. 透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 標籤，存取「儲存空間 LUN」。
2. 按一下 Snapshot 頁框中的**新增 Snapshot 空間**。
3. 選取您需要的空間量。
4. 按一下**繼續**。
5. 輸入您有的任何**促銷代碼**，然後按一下**重新計算**。依預設，會完成「此訂單的計費」及「訂單檢閱」欄位。
6. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。在幾分鐘之後，即會佈建您的 Snapshot 空間。

## 如何判定要訂購的 Spanshot 空間量

一般來說，Snapshot 會根據兩項重要資訊來使用 Snapshot 空間：
- 您的作用中檔案系統有多少變更、
- 您計劃保留 Snapshot 的時間長度。  

實質上，計算所需空間量的方式為**（變更率）**x**（保留小時/天/週/月資料數）**。  
**附註**：最初的 Snapshot 所使用的空間量微不足道，因為它只是指出作用中檔案系統區塊的 meta 資料（指標）副本。 

具有大量資料變更（例如高變更率資料庫）及冗長 Snapshot 保留期間的磁區，比起具有適度變更（例如 VM 資料儲存庫）及更適度的 Snapshot 保留排程的磁區，將需要更多的 Snapshot 空間。 

如果您要建立具有 500 GB 實際資料之磁區的 12 個每小時 Snapshot，且在每個 Snapshot 之間看到 1% 的變更，則 Snapshot 空間最終為 60 GB。

*（5 G 變更率）x（12 個每小時 Snapshot）=（60 GB 已使用空間）*

反之，如果實際資料為 500 GB（具有 12 個每小時 Snapshot），每小時看到 10% 的變更，則最終為 600 GB。

*（50 GB 變更率）x（12 個每小時 Snapshot）=（600 GB 已使用空間）*

因此，在決定您需要多少 Snapshot 空間時，請仔細考慮變更率。它對您需要多少 Snapshot 空間有巨大影響。儘管磁區大小可能表示較高的變更量，但具有 5 GB 變更的 500 GB 磁區，與具有 5 GB 變更的 10 TB 磁區，兩者會造成相同的 Snapshot 空間用量。

此外，對於大部分工作負載而言，磁區越大，一開始需要特意留給 Snapshot 的空間就越少。這主要是由於我們平台的基礎資料效率，以及 Snapshot 在我們環境中如何運作的本質所致。



