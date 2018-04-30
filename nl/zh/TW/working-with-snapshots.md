---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 管理 Snapshot

## 如何建立 Snapshot 排程？

Snapshot 排程可讓您決定要建立儲存空間磁區之時間點參照的頻率及時間。每個儲存空間磁區最多可以有 50 個 Snapshot。排程是透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 標籤來管理。

您必須先購買 Snapshot 空間（如果未在起始佈建儲存空間磁區期間購買的話），才能設定起始排程。

### 如何新增 Snapshot 排程

Snapshot 排程可以設定為每小時、每日及每週間隔，且各有不同的保留週期。每個儲存空間磁區最多可以有 50 個已排定的 Snapshot（這可以混合使用每小時、每日及每週排程）及手動 Snapshot。

1. 按一下儲存空間磁區，按一下**動作**下拉方框，然後按一下**排程 Snapshot**。
2. 在「新建排程 Snapshot」對話框中，有三種不同的 Snapshot 頻率可供選取。使用三者的任意組合，以建立綜合性的 Snapshot 排程。
   - 每小時
      - 指定應該在每小時的幾分執行 Snapshot；預設值是現行分鐘。
      - 指定在捨棄最舊的 Snapshot 之前，要保留的每小時 Snapshot 數目。
   - 每日
      - 指定應該在幾點幾分執行 Snapshot；預設值是現行小時及分鐘。
      - 選取在捨棄最舊的 Snapshot 之前，要保留的每小時 Snapshot 數目。
   - 每週
      - 指定應該在星期幾的幾點幾分執行 Snapshot；預設值是現行日、小時及分鐘。
      - 選取在捨棄最舊的 Snapshot 之前，要保留的每週 Snapshot 數目。
3. 按一下**儲存**，然後建立另一個頻率不同的排程。請注意，您將收到警告訊息，而且，如果已排定 Snapshot 總數超過 50，則無法儲存。

所擷取 Snapshot 的清單即會顯示在「詳細資料」頁面的 Snapshot 區段中。

## 如何擷取手動 Snapshot？

在應用程式升級或維護期間的各種時間點，都可以擷取手動 Snapshot。它們也可讓您跨多台機器擷取 Snapshot，這些機器已在應用程式層次暫時予以停用。

每個儲存空間磁區最多可以有 50 個手動 Snapshot。

1. 按一下儲存空間磁區。
2. 按一下「動作」下拉方框。
3. 按一下**擷取手動 Snapshot**。將會擷取 Snapshot，並顯示在「詳細資料」頁面的 Snapshot 區段中。它的排程會是「手動」。

## 如何查看具有「已耗用空間」及「管理功能」的 Snapshot 清單？

您可以在**詳細資料**頁面（儲存空間、{{site.data.keyword.blockstorageshort}}）上看到已保留 Snapshot 及已耗用空間的清單。使用**動作**下拉功能表或頁面上各種區段中的鏈結，以在「詳細資料」頁面上處理管理功能（編輯排程以及新增其他空間）。

## 如何檢視保留的 Snapshot 清單？

保留的 Snapshot 是根據您在設定排程時於「保留最後一個欄位」中輸入的數字。您可以在 Snapshot 區段下檢視所擷取的 Snapshot。Snapshot 會依排程列出。

## 如何查看已使用的 Snapshot 空間量？

「詳細資料」頁面頂端的圓餅圖會顯示已使用的空間量，以及剩下的空間量。當您開始達到空間臨界值 75%、90% 及 95% 時，會收到通知。

## 如何變更磁區的 Snapshot 空間量？

您可能需要將 Snapshot 空間新增至先前沒有任何 Snapshot 空間或可能需要額外 Snapshot 空間的磁區。根據需求，您可以新增 5 GB 到 4,000 GB。 

**附註**：Snapshot 空間只能增加，而不能減少。在您確定實際需要的空間量之前，建議您選取較小的空間量。請記住，自動及手動 Snapshot 會共用相同的空間。

Snapshot 空間是透過**儲存空間、{{site.data.keyword.blockstorageshort}}** 來變更。

1. 按一下儲存空間磁區，按一下**動作**下拉方框，然後按一下**新增其他 Snapshot 空間**。
2. 從提示中，選取大小範圍。大小範圍通常是從 0 到您的磁區大小。
3. 按一下**繼續**，以佈建額外的空間。
4. 輸入您有的任何「促銷代碼」，然後按一下**重新計算**。「此訂單的計費」及「訂單檢閱」都會有預設值。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。在幾分鐘之後，即會佈建您的額外 Snapshot 空間。

## 如何在接近我的 Snapshot 空間限制及 Snapshot 已刪除時收到通知？

當您達到三個不同的空間臨界值（75%、90% 及 95%）時，會透過「支援中心」內的支援問題單將通知傳送至帳戶上的「主要使用者」。

- **75% 容量**：傳送 Snapshot 空間使用率已超過 75% 的警告。如果您注意到警告，並手動新增空間，或刪除保留或不需要的 Snapshot，則會註明動作並關閉問題單。如果您未執行任何動作，則必須手動確認該問題單，然後將其關閉。
- **90% 容量**：當 Snapshot 空間使用率超過 90% 時，會傳送第二個警告。如同達到 75% 容量一樣，如果您採取必要的動作來減少使用的空間，則會註明該動作並關閉問題單。如果您未執行任何動作，則必須手動確認該問題單，然後將其關閉。
- **95% 容量**：傳送最終警告。如果未採取任何動作讓空間低於臨界值，則會產生通知，並自動進行刪除作業，以便可以建立未來的 Snapshot。會刪除已排定的 Snapshot（從最舊的 Snapshot 開始），直到使用率低於 95% 為止，而且每次使用率超出 95% 時都會繼續進行刪除，直到它低於臨界值為止。如果手動增加空間或刪除 Snapshot，則會重設警告，並在再次超出臨界值時重新發出。如果未採取任何動作，則這會是收到的唯一警告。

## 如何刪除 Snapshot 排程？

Snapshot 排程可以透過**儲存空間、{{site.data.keyword.blockstorageshort}}** 來刪除。

1. 在**詳細資料**頁面的 **Snapshot 排程**頁框中，按一下要刪除的排程。
2. 按一下要刪除之排程旁的勾選框，然後按一下**儲存**。<br />
**注意**：如果您要使用抄寫特性，則請確定所刪除的排程不是抄寫所使用的排程。如需刪除抄寫排程的相關資訊，請按一下[這裡](replication.html)。

## 如何刪除 Snapshot？

可以手動移除不再需要的 Snapshot，以釋放空間供未來的 Snapshot 使用。刪除作業是透過**儲存空間、{{site.data.keyword.blockstorageshort}}** 來進行。

1. 按一下儲存空間磁區，然後向下捲動至 Snapshot 區段，以查看現有 Snapshot 清單。
2. 按一下特定 Snapshot 旁邊的**動作**下拉清單，然後按一下**刪除**來刪除 Snapshot。這不會影響相同排程上的任何未來或過去 Snapshot，因為 Snapshot 之間沒有相依關係。

當您達到空間限制時，會自動刪除未依上述方式刪除的手動 Snapshot（最舊的最先刪除）。

## 如何使用 Snapshot 將我的儲存空間磁區還原至特定時間點？

因為使用者錯誤或資料毀損，所以您可能需要將儲存空間磁區還原至特定時間點。

1. 從主機中卸載並分離您的儲存空間磁區。
   - 按一下[這裡](accessing_block_storage_linux.html)，以取得 Linux 上的 {{site.data.keyword.blockstorageshort}} 指示。
   - 按一下[這裡](accessing-block-storage-windows.html)，以取得 Microsoft Windows 上的 {{site.data.keyword.blockstorageshort}} 指示。
2. 按一下 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中的**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
3. 向下捲動並按一下要還原的磁區。**詳細資料**頁面的 **Snapshot** 區段會顯示所有已儲存 Snapshot 的清單及其大小和建立日期。
4. 按一下要使用之 Snapshot 的**動作**按鈕，然後按一下**還原**。<br/>
  **附註**：執行還原會導致在擷取所使用 Snapshot 之後建立或修改的資料流失。完成時，儲存空間磁區將回到擷取 Snapshot 時所處的相同狀態。系統會出現提示來通知您這種情況。
5. 按一下**是**，以起始還原。您將會在頁面頂端收到一則訊息，指出使用選取的 Snapshot 還原磁區。此外，{{site.data.keyword.blockstorageshort}} 上的磁區旁會出現一個圖示，指出有一個作用中交易正在進行。將游標移至圖示上方會產生一個對話框，指出交易。完成交易之後，圖示即會消失。
6. 將儲存空間磁區裝載並重新連接至主機。
   - 按一下[這裡](accessing_block_storage_linux.html)，以取得 Linux 上的 {{site.data.keyword.blockstorageshort}} 指示。
   - 按一下[這裡](accessing-block-storage-windows.html)，以取得 Microsoft Windows 上的 {{site.data.keyword.blockstorageshort}} 指示。
   
**附註**：還原磁區將導致刪除所還原 Snapshot 之前的所有 Snapshot。
