---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 將現有的 {{site.data.keyword.blockstorageshort}} 升級至加強型 {{site.data.keyword.blockstorageshort}}

精選資料中心內現在提供加強型 {{site.data.keyword.blockstoragefull}}。若要查看已升級資料中心及可用特性（例如可調整的 IOPS 速率及可擴充的磁區）的清單，請按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)。如需提供者管理的已加密儲存空間的相關資訊，請參閱 [{{site.data.keyword.blockstorageshort}}靜態加密](block-file-storage-encryption-rest.html)。

偏好的移轉路徑是同時連接至兩個 LUN，並將資料直接從某個 LUN 傳送至另一個 LUN。細節會取決於作業系統，以及是否預期在複製作業期間變更資料。

我們假設您已將未加密 LUN 連接至主機。若否，請遵循最適合您作業系統的指示來完成此作業：

- [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上連接至 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html)

這些資料中心內佈建的所有加強型 {{site.data.keyword.blockstorageshort}} 磁區，都有不同於未加密磁區的裝載點。為了確保兩個儲存空間磁區都是使用正確的裝載點，您可以在主控台的**磁區詳細資料**頁面中檢視裝載點資訊。您也可以透過 API 呼叫來存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
{:tip}

## 建立 {{site.data.keyword.blockstorageshort}}

使用 API 來下訂單時，請指定「儲存空間即服務」套件，以確保使用新的儲存空間來取得已更新的特性。
{:important}

下列指示適用於透過 {{site.data.keyword.slportal}} 訂購加強型 LUN。新 LUN 的大小必須等於或大於原始磁區，以促進移轉。

### 訂購耐久性 LUN

1. 從 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或是從 {{site.data.keyword.BluSoftlayer_full}} 型錄按一下**基礎架構 > 儲存空間 > {{site.data.keyword.blockstorageshort}}**。
2. 在右上方按一下**訂購 {{site.data.keyword.blockstorageshort}}**。
3. 從**選取儲存空間類型**清單中，選取**耐久性**。
4. 選取您的部署**位置**（資料中心）。
   - 確定將新的「儲存空間」新增至與先前磁區相同的位置。
5. 選取計費選項。您可以選擇按小時與按月計費。
6. 選取 IOPS 層級。
7. 按一下**選取儲存空間大小**，然後從清單中選取您的儲存空間大小。
8. 按一下**指定 Snapshot 空間大小**，然後從清單中選取 Snapshot 大小。這是您可以使用的空間之外的額外空間。

   如需 Snapshot 空間考量和建議的相關資訊，請參閱[訂購 Snapshot](ordering-snapshots.html)。
   {:tip}
9. 從清單中，選擇您的 **OS 類型**。
10. 按一下**繼續**。您會看到按月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。
11. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**。

### 訂購效能 LUN

1. 從 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**，或是從 {{site.data.keyword.BluSoftlayer_full}} 型錄按一下**基礎架構 > 儲存空間 > {{site.data.keyword.blockstorageshort}}**。
2. 在右側，按一下**訂購 {{site.data.keyword.blockstorageshort}}**。
3. 從**選取儲存空間類型**清單中，選取**效能**。
4. 按一下**位置**，然後選取資料中心。

   確定將新的「儲存空間」新增至與先前訂購主機相同的位置。
   {:important}
5. 選取計費選項。您可以選擇按小時與按月計費。
6. 選取適當的**儲存空間大小**。
7. 在**指定 IOPS** 欄位中，輸入 IOPS。
8. 按一下**繼續**。您會看到按月及按比例分配的費用，並且會有最後一次機會可檢閱訂單詳細資料。如果您要變更訂單，請按**上一步**。
9. 按一下**我已閱讀主要服務合約**勾選框，然後按一下**下訂單**。

在一分鐘以內即會佈建儲存空間，並且它會顯示在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.blockstorageshort}} 頁面上。



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
  - 如果您有備份、靜態內容，以及在複製期間預期不會變更的事物，則沒有任何主要考量。
  - 如果您正在 {{site.data.keyword.blockstorageshort}} 上執行資料庫或虛擬機器，請確定在複製期間未變更資料，以避免資料毀損。如果您有任何頻寬考量，請在離峰時間執行移轉。如果您需要這些考量的協助，請開立支援問題單。

3. 複製您的資料。
   - 若為 **Microsoft Windows**，請將新的儲存空間格式化，並使用「Windows 檔案總管」，將資料從原始 {{site.data.keyword.blockstorageshort}} LUN 複製到新的 LUN。
   - 若為 **Linux**，您可以使用 `rsync` 來複製資料。範例如下：
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   建議您搭配使用先前的指令與 `--dry-run` 旗標一次，以確定正確地排列路徑。如果此處理程序遭到岔斷，您可以刪除最後一個正在複製的目的地檔案，以確定從頭將它複製到新位置。<br/>
這個指令在沒有 `--dry-run` 旗標的情況下完成時，資料會複製到新的 {{site.data.keyword.blockstorageshort}} LUN。重新執行指令，以確定未遺漏任何項目。您也可以手動檢閱這兩個位置，以尋找任何可能遺漏的項目。<br/>
移轉完成後，您就可以將正式作業移至新的 LUN。然後，您可以分離並刪除配置中的原始 LUN。刪除作業也會移除目標網站上與原始 LUN 相關聯的任何 Snapshot 或抄本。
