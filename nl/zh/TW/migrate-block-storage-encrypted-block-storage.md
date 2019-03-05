---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 將現有的 {{site.data.keyword.blockstorageshort}} 升級至加強型 {{site.data.keyword.blockstorageshort}}
{: #migratestorage}

精選資料中心內現在提供加強型 {{site.data.keyword.blockstoragefull}}。若要查看已升級資料中心及可用特性（例如可調整的 IOPS 速率及可擴充的磁區）的清單，請按一下[這裡](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)。如需提供者管理的已加密儲存空間的相關資訊，請參閱 [{{site.data.keyword.blockstorageshort}} 靜態加密](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)。

偏好的移轉路徑是同時連接至兩個 LUN，並將資料直接從某個 LUN 傳送至另一個 LUN。細節會取決於作業系統，以及是否預期在複製作業期間變更資料。

我們假設您已將未加密 LUN 連接至主機。若否，請遵循最適合您作業系統的指示來完成此作業：

- [在 Linux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上連接至 LUNS](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

這些資料中心內佈建的所有加強型 {{site.data.keyword.blockstorageshort}} 磁區，都有不同於未加密磁區的裝載點。為了確保兩個儲存空間磁區都是使用正確的裝載點，您可以在主控台的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
{:tip}

## 建立 {{site.data.keyword.blockstorageshort}}

使用 API 來下訂單時，請指定「儲存空間即服務」套件，以確保使用新的儲存空間來取得已更新的特性。
{:important}

您可以透過 IBM Cloud 主控台及 {{site.data.keyword.slportal}} 訂購加強的 LUN。新 LUN 的大小必須等於或大於原始磁區，以促進移轉。

- [訂購具有預先定義 IOPS 層級（耐久性）的 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [訂購具有自訂 IOPS（效能）的 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

在幾分鐘之後，您的新儲存空間就可以裝載。您可以在「資源清單」和 {{site.data.keyword.blockstorageshort}} 清單中檢視它。

## 將新 {{site.data.keyword.blockstorageshort}} 連接至主機

「授權的」主機是已獲得磁區存取權的主機。如果沒有主機授權，就無法從系統中存取或使用儲存空間。授權主機存取磁區會產生使用者名稱、密碼及 iSCSI 完整名稱 (IQN)（這是裝載多路徑 I/O (MPIO) iSCSI 連線所需要的項目）。

1. 按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 捲動至**授權主機**。
3. 在右側，按一下**授權主機**。選取可存取磁區的主機。


## Snapshot 及抄寫

您是否已為原始 LUN 建立 Snapshot 及抄寫？如果是，您需要為新的 LUN 使用與原始磁區相同的設定，設定抄寫、Snapshot 空間，以及建立 Snapshot 排程。

如果尚未升級您的抄寫目標資料中心，則在升級該資料中心之前，無法建立新磁區的抄寫。


## 移轉資料

1. 同時連接至原始及新的 {{site.data.keyword.blockstorageshort}} LUN。

   如果您將兩個 LUN 連接至主機時需要協助，請開立支援案例。
   {:tip}

2. 考量您在原始 {{site.data.keyword.blockstorageshort}} LUN 上具有的資料類型，以及如何最適當地將資料複製到新的 LUN。
  - 如果您有備份、靜態內容，以及在複製期間預期不會變更的事物，您不需要太過擔心。
  - 如果您正在 {{site.data.keyword.blockstorageshort}} 上執行資料庫或虛擬機器，請確定在複製期間未變更資料，以避免資料毀損。
  - 如果您有任何頻寬考量，請在離峰時間執行移轉。
  - 如果您需要這些考量的協助，請開立支援案例。

3. 複製您的資料。
   - 若為 **Microsoft Windows**，請將新的儲存空間格式化，並使用「Windows 檔案總管」，將資料從原始 {{site.data.keyword.blockstorageshort}} LUN 複製到新的 LUN。
   - 若為 **Linux**，您可以使用 `rsync` 來複製資料。
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   建議您搭配使用先前的指令與 `--dry-run` 旗標一次，以確定正確地排列路徑。如果此處理程序遭到岔斷，您可以刪除最後一個正在複製的目的地檔案，以確定從頭將它複製到新位置。<br/>
這個指令在沒有 `--dry-run` 旗標的情況下完成時，資料會複製到新的 {{site.data.keyword.blockstorageshort}} LUN。重新執行指令，以確定未遺漏任何項目。您也可以手動檢閱這兩個位置，以尋找任何可能遺漏的項目。<br/>
移轉完成後，您就可以將正式作業移至新的 LUN。然後，您可以分離並刪除配置中的原始 LUN。刪除作業也會移除目標網站上與原始 LUN 相關聯的任何 Snapshot 或抄本。
