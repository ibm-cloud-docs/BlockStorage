---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 管理 Snapshot

## 建立 Snapshot 排程

您可以使用 Snapshot 排程決定要建立儲存空間磁區之時間點參照的頻率及時間。每個儲存空間磁區最多可以有 50 個 Snapshot。排程是透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 的**儲存空間** > **{{site.data.keyword.blockstorageshort}}** 標籤來管理。

您必須先購買 Snapshot 空間（如果未在起始佈建儲存空間磁區期間購買的話），才能設定起始排程。
{:important}

### 新增 Snapshot 排程

Snapshot 排程可以設定為每小時、每日及每週間隔，且各有不同的保留週期。Snapshot 的上限為每個儲存空間磁區各 50 個 Snapshot（可以混合每小時、每日和每週排程）以及手動 Snapshot。

1. 按一下儲存空間磁區，並按一下**動作**，然後按一下**排程 Snapshot**。
2. 在「新建排程 Snapshot」視窗中，有三種不同 Snapshot 頻率可供選取。使用三者的任意組合，以建立綜合性的 Snapshot 排程。
   - 每小時
      - 指定應該在每小時的幾分擷取 Snapshot。預設值是現行分鐘。
      - 指定在捨棄最舊的 Snapshot 之前，要保留的每小時 Snapshot 數目。
   - 每日
      - 指定應該在幾點幾分擷取 Snapshot。預設值是現行小時及分鐘。
      - 選取在捨棄最舊的 Snapshot 之前，要保留的每小時 Snapshot 數目。
   - 每週
      - 指定應該在星期幾的幾點幾分擷取 Snapshot。預設值是現行日、小時及分鐘。
      - 選取在捨棄最舊的 Snapshot 之前，要保留的每週 Snapshot 數目。
3. 按一下**儲存**，然後建立另一個頻率不同的排程。如果所排程 Snapshot 的總數超過 50，您會收到警告訊息，而且無法儲存。

所擷取之 Snapshot 的清單會顯示在**詳細資料**頁面的 **Snapshot** 區段中。

## 擷取手動 Snapshot

在應用程式升級或維護期間的各種時間點，都可以擷取手動 Snapshot。您也可以跨多部伺服器擷取 Snapshot，這些伺服器已在應用程式層次暫時予以取消啟動。

每個儲存空間磁區的 Snapshot 上限為 50。

1. 按一下儲存空間磁區。
2. 按一下**動作**。
3. 按一下**擷取手動 Snapshot**。Snapshot 會建立並顯示在**詳細資料**頁面的 **Snapshot** 區段中。它的排程顯示為「手動」。

## 列出具有已使用空間資訊和管理功能的所有 Snapshot

您可以在**詳細資料**頁面上看到所保留的 Snapshot 及所使用空間的清單。使用**動作**功能表或頁面上各種區段中的鏈結，以在「詳細資料」頁面上處理管理功能（編輯排程以及新增其他空間）。

## 檢視已保留 Snapshot 清單

保留的 Snapshot 是根據您在設定排程時於**保留最後一個**欄位中輸入的數字。您可以在 **Snapshot** 區段下檢視所擷取的 Snapshot。Snapshot 會依排程列出。

## 檢視已使用的 Snapshot 空間量

**詳細資料**頁面上的圓餅圖會顯示已使用的空間量，以及剩下的空間量。當您達到空間臨界值（75%、90% 及 95%）時，會收到通知。

## 變更磁區的 Snapshot 空間量

您可能需要將 Snapshot 空間新增至先前沒有任何 Snapshot 空間或可能需要額外 Snapshot 空間的磁區。根據需求，您可以新增 5 - 4,000 GB。

Snapshot 空間只能增加。無法減少。在您確定所需的空間量之前，您可以選取較小的空間量。請記住，自動及手動 Snapshot 會共用空間。
{:note}

Snapshot 空間是透過**儲存空間** > **{{site.data.keyword.blockstorageshort}}** 進行變更。

1. 按一下儲存空間磁區，並按一下**動作**，然後按一下**新增其他 Snapshot 空間**。
2. 從提示中，選取大小範圍。大小範圍通常是從 0 到您的磁區大小。
3. 按一下**繼續**。
4. 輸入您有的任何「促銷代碼」，然後按一下**重新計算**。依預設，會完成「此訂單的計費」及「訂單檢閱」欄位。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。在幾分鐘之後，即會佈建您的額外 Snapshot 空間。

## 達到 Snapshot 空間限制且刪除 Snapshot 時收到通知

當您達到三個不同的空間臨界值（75%、90% 及 95%）時，會透過支援問題單將通知傳送至帳戶上的「主要使用者」。

- 達到 **75% 容量**時，會傳送 Snapshot 空間用量已超過 75% 的警告。如果您注意到警告，並手動新增空間，或刪除保留或不需要的 Snapshot，則會註明動作並關閉問題單。如果您未執行任何動作，則必須手動確認該問題單，然後將其關閉。
- 達到 **90% 容量**時，會傳送 Snapshot 空間用量已超過 90% 的第二次警告。如同達到 75% 容量一樣，如果您採取必要的動作來減少使用的空間，則會註明該動作並關閉問題單。如果您未執行任何動作，則必須手動確認該問題單，然後將其關閉。
- 達到 **95% 容量**時，會傳送最終警告。如果未採取任何動作讓空間用量低於臨界值，則會產生通知，並自動進行刪除作業，以供建立未來的 Snapshot。會從最舊的排程 Snapshot 開始刪除，直到使用率降到 95% 以下為止。每次用量超過 95% 時都會繼續刪除 Snapshot，直到低於臨界值為止。如果手動增加空間或刪除 Snapshot，則會重設警告，並在再次超出臨界值時重新發出。如果未採取任何動作，則此通知會是您唯一收到的警告。

## 刪除 Snapshot 排程

Snapshot 排程可以透過**儲存空間** > **{{site.data.keyword.blockstorageshort}}** 進行取消。

1. 在**詳細資料**頁面的 **Snapshot 排程**頁框中，按一下要刪除的排程。
2. 按一下要刪除之排程旁的勾選框，然後按一下**儲存**。<br />

如果您要使用抄寫特性，請確定您要刪除的排程不是抄寫所使用的排程。如需刪除抄寫排程的相關資訊，請參閱[抄寫資料](replication.html)。
{:important}

## 刪除 Snapshot

可以手動移除不再需要的 Snapshot，以釋放空間供未來的 Snapshot 使用。透過**儲存空間** > **{{site.data.keyword.blockstorageshort}}** 完成刪除。

1. 按一下儲存空間磁區，然後捲動至 **Snapshot** 區段，以查看現有 Snapshot 清單。
2. 按一下特定 Snapshot 旁的**動作**，然後按一下**刪除**來刪除 Snapshot。這項刪除動作不會影響相同排程上的任何未來或過去 Snapshot，因為 Snapshot 之間沒有相依關係。

當您達到空間限制時，會自動刪除入口網站中未手動刪除的手動 Snapshot（最舊的最先刪除）。

## 使用 Snapshot 將儲存空間磁區還原至特定時間點

因為使用者錯誤或資料毀損，所以您可能需要將儲存空間磁區還原至特定時間點。

1. 從主機中卸載並分離您的儲存空間磁區。
   - [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html#unmounting-block-storage-volumes)
   - [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html#unmounting-block-storage-volumes)
2. 按一下 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 中的**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
3. 向下捲動，並按一下要還原的磁區。**詳細資料**頁面的 **Snapshot** 區段會顯示所有已儲存 Snapshot 的清單及其大小和建立日期。
4. 按一下要使用之 Snapshot 旁的**動作**，然後按一下**還原**。<br/>

   完成還原會導致在擷取 Snapshot 之後建立或修改的資料流失。發生此資料流失是因為您的儲存空間磁區回到 Snapshot 時間當時的相同狀態。
   {:note}
5. 按一下**是**，以開始還原。

   預期頁面上會出現一則訊息，指出正在使用所選取的 Snapshot 來還原磁區。此外，{{site.data.keyword.blockstorageshort}} 上的磁區旁會出現一個圖示，指出正在進行作用中交易。將游標移至圖示上方會產生一個視窗，顯示交易。完成交易時，圖示即會消失。
   {:note}
6. 將儲存空間磁區裝載並重新連接至主機。
   - [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html)
   - [在 CloudLinux 上連接至 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
   - [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html)

還原磁區會導致刪除在用於還原的 Snapshot 之後擷取的所有 Snapshot。
{:important}
