---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 將 {{site.data.keyword.blockstorageshort}} 移轉至加密的 {{site.data.keyword.blockstorageshort}}

已在精選資料中心內提供「耐久性」或「效能」的加密 {{site.data.keyword.blockstoragefull}}。您將在下面找到如何將 {{site.data.keyword.blockstorageshort}} 從未加密移轉至加密的相關資訊。如需提供者管理之加密儲存空間的相關資訊，請閱讀 [{{site.data.keyword.blockstorageshort}} 靜態加密](block-file-storage-encryption-rest.html)文章。若要查看已升級資料中心及可用特性的清單，請按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)。

偏好的移轉路徑是同時連接至兩個 LUN，並將資料直接從某個檔案 LUN 傳送至另一個檔案 LUN。細節將取決於作業系統，以及是否預期在複製作業期間變更資料。

已概述較常見的情境，方便您使用。我們假設您已將未加密檔案 LUN 連接至主機。若否，請遵循下面最適合您所執行之作業系統的指示來完成此作業。

- [在 Linux 上存取 {{site.data.keyword.blockstorageshort}}](accessing_block_storage_linux.html)

- [在 Windows 上存取 {{site.data.keyword.blockstorageshort}}](accessing-block-storage-windows.html)

 
## 建立加密 LUN

使用下列步驟，以建立一個相同大小或較大的加密 LUN，以協助進行移轉處理程序。訂購加密耐久性儲存空間 LUN

1. 按一下 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 首頁中的**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或者，按一下 {{site.data.keyword.BluSoftlayer_full}} 型錄中的**基礎架構** > **儲存空間** > **{{site.data.keyword.blockstorageshort}}**。

2. 按一下 {{site.data.keyword.blockstorageshort}} 頁面上的**訂購 {{site.data.keyword.blockstorageshort}}** 鏈結。

3. 選取**耐久性**。

4. 選取原始 LUN 所在的資料中心。<br/> **附註**：只有精選資料中心內才提供加密。

5. 選取所需的 IOPS 層級。

6. 選取所需的儲存空間量（以 GB 為單位）。若為 TB，1 TB 等於 1,000 GB，而 12 TB 等於 12,000 GB。

7. 輸入 Snapshot 所需的儲存空間量（以 GB 為單位）。

8. 從下拉清單中，選取 VMware OS。

9. 提交訂單。

## 訂購加密效能儲存空間 LUN

1. 按一下 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 首頁中的**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或者，按一下 {{site.data.keyword.BluSoftlayer_full}} 型錄中的**基礎架構** > **儲存空間** > **{{site.data.keyword.blockstorageshort}}**。

2. 按一下**訂購 {{site.data.keyword.blockstorageshort}}**。

3. 選取**效能**。

4. 選取原始 LUN 所在的資料中心。請注意，加密僅適用於有星號 (`*`) 的資料中心。

5. 選取所需的儲存空間量（以 GB 為單位）。若為 TB，1 TB 等於 1,000 GB，而 12 TB 等於 12,000 GB。

6. 輸入所需的 IOPS 數量（間隔為 100）。

7. 從下拉清單中，選取 VMware OS。

8. 提交訂單。

在一分鐘以內即會佈建儲存空間，並且它會顯示在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.blockstorageshort}} 頁面上。

 
## 將新磁區連接至主機

「授權」主機是已獲得磁區存取權的主機。如果沒有主機授權，就無法從系統中存取或使用儲存空間。授權主機存取磁區會產生「使用者名稱」、「密碼」及 iSCSI 完整名稱 (IQN)（這是裝載多路徑 I/O (MPIO) iSCSI 連線所需要的項目）。

1. 按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。

2. 捲動至頁面的**授權主機**區段。

3. 按一下頁面右側的**授權主機**鏈結。選取可存取磁區的主機。

 
## Snapshot 及抄寫

您是否已為原始 LUN 建立 Snapshot 及抄寫？如果是，則需要為新的加密的 LUN 使用與原始磁區相同的設定，設定抄寫、Snapshot 空間，以及建立 Snapshot 排程。 

請注意，如果未升級您的抄寫目標資料中心來進行加密，則除非升級該資料中心，否則無法建立加密磁區的抄寫。

 
## 移轉資料

您應該同時連接至原始及加密 {{site.data.keyword.blockstorageshort}} 磁區。 
- 如果沒有，請確定您已正確遵循上述及其他貼文中提到的步驟。請開立支援問題單，以取得連接兩個 LUN 的協助。

### 資料考量

此時，請考量您在原始 {{site.data.keyword.blockstorageshort}} LUN 上具有的資料類型，以及如何最適當地將資料複製到加密 LUN。如果您有備份、靜態內容，以及在複製期間預期不會變更的事物，則沒有任何主要考量。

如果您正在 {{site.data.keyword.blockstorageshort}} 上執行資料庫或虛擬機器，請確定在複製期間未變更原始 LUN 上的資料，以期不會發生毀損。如果您有任何頻寬考量，則應該在離峰時間執行移轉。如果您需要關於這些考量的協助，歡迎開立支援問題單。
 
### Microsoft Windows

若要將資料從原始 {{site.data.keyword.blockstorageshort}} LUN 複製到加密 LUN，請使用「Windows 檔案總管」格式化新的儲存空間並將檔案複製到其中。

 
### Linux

您可以考慮使用 rsync 來複製資料。以下是範例指令：

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

建議您搭配使用上述指令與 --dry-run 旗標一次，以確定正確地排列路徑。如果此程序被岔斷，建議您刪除最後一個正在複製的目的地檔案，以確定從頭將它複製到新位置。

此指令在沒有 --dry-run 旗標的情況下完成之後，資料應該已複製到加密 {{site.data.keyword.blockstorageshort}} LUN。您應該向上捲動並重新執行指令，確定未遺漏任何項目。您也可以手動檢閱這兩個位置，以尋找任何可能遺漏的項目。

移轉完成後，您就可以將正式作業移至加密 LUN，然後分離並刪除配置中的原始 LUN。請注意，刪除作業也會移除目標網站上與原始 LUN 相關聯的任何 Snapshot 或抄本。
