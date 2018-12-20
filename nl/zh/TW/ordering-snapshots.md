---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 訂購 Snapshot

若要自動或手動建立儲存空間磁區的 Snapshot，您需要購買空間來保留它們。您可以購買最多達到儲存空間磁區量的容量（在起始磁區購買期間購買，或之後使用此處說明的步驟購買）。

1. 登入 [{{site.data.keyword.cloud_notm}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/catalog/){:new_window}，然後按一下左上方的功能表圖示。選取**標準基礎架構**。

   或者，您也可以登入 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window}。
2. 透過**儲存空間** > **{{site.data.keyword.blockstorageshort}}** 存取「儲存空間 LUN」。
2. 按一下 Snapshot 頁框中的**變更 Snapshot 空間**。
3. 選取您需要的空間量和付款方法。
4. 按一下**繼續**。
5. 輸入您有的任何**促銷代碼**，然後按一下**重新計算**。依預設，會完成「此訂單的計費」及「訂單檢閱」欄位。
6. 勾選**我已閱讀主要服務合約，並同意其中的條款**勾選框，然後按**下訂單**。在幾分鐘之後，即會佈建您的 Snapshot 空間。

## 判定要訂購的 Spanshot 空間量

一般來說，Snapshot 會根據兩個重要因素來使用 Snapshot 空間：
- 您的作用中檔案系統一段時間內有多少變更、
- 您計劃保留 Snapshot 的時間長度。  

計算所需空間量的方式為**（變更率）**x**（保留小時/天/週/月資料數）**。

最初的 Snapshot 所使用的空間量微不足道，因為它只是指出作用中檔案系統區塊的 meta 資料（指標）副本。
{:note}

具有許多資料變更及冗長保留期間的磁區，比起具有中等變更及中等保留排程的磁區，需要更多的空間。第一種類型的範例是高變更率資料庫。第二種類型的範例是 VMware 資料儲存庫。

如果您擷取 500 GB 實際資料的 12 個每小時 Snapshot，且在每個 Snapshot 之間有 1% 的變更，則 Snapshot 空間最終為 60 GB。

*（5 GB 變更率）x（12 個每小時 Snapshot）=（60 GB 已使用空間）*

反之，如果實際資料為 500 GB（具有 12 個每小時 Snapshot），每小時看到 10% 的變更，則使用的 Snapshot 空間為 600 GB。

*（50 GB 變更率）x（12 個每小時 Snapshot）=（600 GB 已使用空間）*

因此，在決定您需要多少 Snapshot 空間時，請仔細考慮變更率。它對您需要多少 Snapshot 空間有巨大影響。磁區越大越可能更頻繁地變更。不過，具有 5 GB 變更的 500 GB 磁區，與具有 5 GB 變更的 10 TB 磁區，兩者會使用相同的 Snapshot 空間量。

此外，對於大部分工作負載而言，磁區越大，一開始需要特意保留的空間就越少。這主要是由於基礎資料效率，以及 Snapshot 在環境中如何運作的本質所致。
