---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-10"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 災難回復 - 使用可存取的主要磁區進行失效接手

如果主要站台發生災難性失效或災難，而主要儲存空間仍可供存取，客戶可以執行下列動作，以在次要站台上快速存取其資料。

開始進行失效接手之前，請確定所有代管權限都已安排妥當。

授權主機及磁區必須位在相同的資料中心內。例如，不能抄本磁區在「倫敦」，而主機在「阿姆斯特丹」。兩者都必須在「倫敦」，或兩者都必須在「阿姆斯特丹」。
{:note}

1. 登入 [{{site.data.keyword.cloud}} 主控台 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/catalog/){:new_window}，然後按一下左上方的**功能表**圖示。選取**標準基礎架構**。

   或者，您也可以登入 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window}。
2. 從 **{{site.data.keyword.blockstorageshort}}** 頁面，按一下來源或目的地磁區。
3. 按一下**抄本**。
4. 向下捲動至**授權主機**頁框，然後按一下右側的**授權主機**。
5. 強調顯示要授權進行抄寫的主機。若要選取多台主機，請使用 CTRL 鍵，然後按一下適用的主機。
6. 按一下**提交**。如果您沒有可用的主機，系統會提示您在相同的資料中心內購買運算資源。


## 開始從磁區到其抄本的失效接手

如果即將發生故障事件，您可以開始**失效接手**至目的地或目標磁區。目標磁區會變成作用中。啟動前次順利抄寫的 Snapshot，而且磁區變成可用以進行裝載。將會遺失自前次抄寫週期以來寫入至來源磁區的所有資料。開始失效接手時，會翻轉抄寫關係。您的目標磁區會變成來源磁區，而先前的來源磁區會變成您的目標，並且後面接著 **REP** 的 **LUN 名稱**來表示。

失效接手是在 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 中的**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 下開始。

**繼續執行這些步驟之前，請先中斷磁區連線。否則，會導致毀損及資料遺失。**

1. 按一下作用中 LUN（「來源」）。
2. 按一下**抄本**，然後按一下**動作**。
3. 選取**失效接手**。

   預期頁面上會出現一則訊息，指出正在進行失效接手。此外，**{{site.data.keyword.blockstorageshort}}** 上的磁區旁會出現一個圖示，指出正在進行作用中交易。將游標移至圖示上方會產生一個視窗，顯示交易。完成交易時，圖示即會消失。在失效接手處理程序期間，配置相關動作是唯讀的。您無法編輯任何 Snapshot 排程或變更 Snapshot 空間。事件會記載在抄寫歷程中。<br/> 當您的目標磁區處於作用中時，您會收到另一則訊息。您原始來源磁區的「LUN 名稱」會更新為以 REP 結束，且其「狀態」變成「非作用中」。
   {:note}
4. 按一下**檢視全部 ({{site.data.keyword.blockstorageshort}})**。
5. 按一下作用中 LUN（先前稱為目標磁區）。
6. 將儲存空間磁區裝載並連接至主機。如需指示，請按一下[這裡](provisioning-block_storage.html)。


## 開始從磁區到其抄本的失效回復

修復原始來源磁區時，您可以開始對原始來源磁區的受管制失效回復。在受管制的失效回復中，

- 作用中的來源磁區會離線，
- 擷取 Snapshot，
- 完成抄寫週期，
- 啟動剛才建立的資料 Snapshot，
- 然後，來源磁區會變成作用中以進行裝載。

開始失效回復時，會再次翻轉抄寫關係。來源磁區會還原為來源磁區，而您的目標磁區會再次成為目標磁區，並且後面接著 **REP** 的 **LUN 名稱**來表示。

失效回復是在 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 中的**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 下開始。

1. 按一下作用中 LUN（「目標」）。
2. 在右上方，按一下**抄本**，然後按一下**動作**。
3. 選取**失效回復**。
   

   預期頁面上會出現一則訊息，顯示正在進行失效接手。此外，**{{site.data.keyword.blockstorageshort}}** 上的磁區旁會出現一個圖示，指出正在進行作用中交易。將游標移至圖示上方會產生一個視窗，顯示交易。完成交易時，圖示即會消失。在失效回復處理程序期間，配置相關動作是唯讀的。您無法編輯任何 Snapshot 排程或變更 Snapshot 空間。事件會記載在抄寫歷程中。
   {:note}
4. 在右上方按一下**檢視所有 {{site.data.keyword.blockstorageshort}}** 鏈結。
5. 按一下作用中 LUN（「來源」）。
6. 將儲存空間磁區裝載並連接至主機。如需指示，請按一下[這裡](provisioning-block_storage.html)。
