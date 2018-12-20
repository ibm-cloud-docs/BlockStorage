---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-13"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 訂購 {{site.data.keyword.blockstorageshort}}

您可以佈建 {{site.data.keyword.blockstorageshort}} 並微調，以符合您的容量和 IOPS 需求。利用兩個指定效能的選項來充份利用儲存空間。

- 您可以從耐久性 IOPS 層級進行選擇，其特色為預先定義的效能層次，可適合沒有妥善定義之效能需求的工作負載。
- 您可以對儲存空間進行細部調整，藉由指定效能的 IOPS 總數來滿足特定效能需求。

## 使用預先定義的 IOPS 層級（耐久性）訂購 {{site.data.keyword.blockstorageshort}}

1. 登入 [IBM Cloud 型錄](https://{DomainName}/catalog/){:new_window}，並按一下**儲存空間**。然後，選取 **{{site.data.keyword.blockstorageshort}}**，並按一下**建立**。

   或者，您也可以登入 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window}，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**。在右上方按一下**訂購 {{site.data.keyword.blockstorageshort}}**。

2. 選取您的部署**位置**（資料中心）。
   - 確定將新的「儲存空間」新增至與您具有的運算主機相同的位置。
3. 計費。如果您已選取具有改良功能的資料中心（已標示星號），則可以選擇「按月計費」或「按小時計費」。
     1. 使用**按小時**計費，會在刪除 LUN 或計費週期結束時，計算區塊 LUN 存在於帳戶上的小時數。看何者為先。如果儲存空間使用期間為幾天或不到一整個月，則每小時計費是一個良好的選擇。按小時計費只適用於這些[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。
     2. 使用**按月**計費，價格是從建立日期到計費週期結束按比例計算，並立即計費。如果區塊 LUN 在計費週期結束之前遭到刪除，則不會退款。如果儲存空間用於正式作業工作負載，而正式作業工作負載使用需要長期（一個月或更久）儲存及存取的資料，則按月計費是一個良好的選擇。
        

        對於在**未**使用改良功能更新的資料中心內佈建的儲存空間，依預設會使用按月計費類型。
        {:important}
4. 在**新儲存空間大小**欄位中輸入儲存空間的大小。
5. 在**儲存空間 IOPS 選項**區段中，選取**耐久性（分層 IOPS）**。
6. 選取應用程式所需的 IOPS 層級。
    - **每 GB 0.25 IOPS** 是為了低 I/O 強度的工作負載而設計。這些工作負載的特點通常是在某個時間會有大百分比的非作用中資料。應用程式範例包括儲存信箱或部門層次檔案共用。
    - **每 GB 2 IOPS** 是針對最通用用途而設計。應用程式範例包括管理小型資料庫，支持 Web 應用程式或 Hypervisor 的虛擬機器磁碟映像檔。
    - **每 GB 4 IOPS** 是為了較高強度工作負載而設計。這些工作負載的特點通常是在某個時間會有高百分比的作用中資料。應用程式範例包括交易式資料庫及其他效能相關的資料庫。
    - **每 GB 10 IOPS** 是為了最嚴苛的工作負載（例如 NoSQL 資料庫所建立的工作負載）以及進行分析的資料處理而設計。此層級適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內最多佈建 4 TB 的儲存空間。
7. 按一下**指定 Snapshot 空間大小**，然後從清單中選取 Snapshot 大小。這是您可以使用的空間之外的額外空間。如需 Snapshot 空間考量及建議，請閱讀[訂購 Snapshot](ordering-snapshots.html)。
8. 從清單中，選擇您的 **OS 類型**。<br/>

   此選擇是根據主機執行的作業系統，之後將無法修改。例如，您的伺服器是 Ubuntu 或 RHEL，請選取 Linux。如果主機是 Windows 2012 或 Windows 2016 伺服器，請從清單中選取 Windows 2008+ 選項。如需各種 Windows 選項的相關資訊，請參閱[常見問題](faqs.html)。
   {:tip}
9. 在右邊檢閱訂單摘要，如果您有「促銷代碼」，請套用它。
10. 檢閱條款之後，請勾選**我已閱讀並同意協力廠商服務合約**方框。
11. 按一下**建立**。在幾分鐘之後，就可以使用您的新儲存空間配置。

依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。請在[這裡](managing-storage-limits.html)閱讀增加限制的相關資訊。<br/><br/>如需同時授權的限制，請參閱[常見問題](faqs.html)。
{:important}

## 使用自訂 IOPS（效能）訂購 {{site.data.keyword.blockstorageshort}}

1. 登入 [IBM Cloud 型錄](https://{DomainName}/catalog/){:new_window}，並按一下**儲存空間**。然後，選取 {{site.data.keyword.blockstorageshort}}，並按一下**建立**。

   或者，您也可以登入 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window}，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**。在右上方按一下**訂購 {{site.data.keyword.blockstorageshort}}**。
2. 按一下**位置**，然後選取資料中心。
   - 確定將新的「儲存空間」新增至與您具有的運算主機相同的位置。
3. 計費。如果您已選取具有改良功能的資料中心（已標示星號），則可以選擇「按月計費」或「按小時計費」。
     1. 使用**按小時**計費，會在刪除 LUN 或計費週期結束時，計算區塊 LUN 存在於帳戶上的小時數。看何者為先。如果儲存空間使用期間為幾天或不到一整個月，則按小時計費是一個良好的選擇。按小時計費只適用於這些[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。
     2. 使用**按月**計費，價格是從建立日期到計費週期結束按比例計算，並立即計費。如果區塊 LUN 在計費週期結束之前遭到刪除，則不會退款。如果儲存空間用於正式作業工作負載，而正式作業工作負載使用需要長期（一個月或更久）儲存及存取的資料，則按月計費是一個良好的選擇。
        

        對於在**未**使用改良功能更新的資料中心內佈建的儲存空間，依預設會使用按月計費類型。
        {:note}
4. 在**新儲存空間大小**欄位中輸入儲存空間的大小。
5. 在**儲存空間 IOPS 選項**區段中，選取**效能（已配置的 IOPS）**。
6. 在**效能（已配置的 IOPS）**欄位中，輸入 IOPS。
7. 按一下**指定 Snapshot 空間大小**，然後從清單中選取 Snapshot 大小。這是您可以使用的空間之外的額外空間。如需 Snapshot 空間考量及建議，請閱讀[訂購 Snapshot](ordering-snapshots.html)。
8. 從清單中，選擇您的 **OS 類型**。<br/>

   此選擇是根據主機執行的作業系統，之後將無法修改。例如，您的伺服器是 Ubuntu 或 RHEL，請選取 Linux。如果主機是 Windows 2012 或 Windows 2016 伺服器，請從清單中選取 Windows 2008+ 選項。如需各種 Windows 選項的相關資訊，請參閱[常見問題](faqs.html)。
   {:tip}
9. 在右邊檢閱訂單摘要，如果您有「促銷代碼」，請套用它。
10. 檢閱條款之後，請勾選**我已閱讀並同意協力廠商服務合約**方框。
11. 按一下**建立**。在幾分鐘之後，就可以使用您的新儲存空間配置。

依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。請在[這裡](managing-storage-limits.html)閱讀增加限制的相關資訊。<br/><br/>如需同時授權的限制，請參閱[常見問題](faqs.html)。
{:important}

## 連接新的儲存空間

當您的佈建要求完成時，請授權主機存取新的儲存空間，並配置連線。根據主機的作業系統而定，遵循適當的鏈結。
- [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上連接至 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [配置 Block Storage 以便使用 cPanel 進行備份](configure-backup-cpanel.html)
- [配置 Block Storage 以便使用 Plesk 進行備份](configure-backup-plesk.html)

## 災難回復考量

為了避免資料遺失，並確保業務持續運作，請考慮將伺服器和儲存空間抄寫在另一個資料中心。抄寫會依據您的 Snapshot 排程，將資料同步保留在兩個不同位置。如需相關資訊，請參閱[抄寫資料](replication.html)。

如果您想要複製磁區，並與原始磁區分開使用，請參閱[建立重複的區塊磁區](how-to-create-duplicate-volume.html)。


## 識別發票上的 {{site.data.keyword.blockstorageshort}}

所有 LUN 在發票上都會顯示為一個明細行項目。「耐久性」會顯示為「耐久性儲存空間服務」，而「效能」會顯示為「效能儲存空間服務」。費率將根據您的儲存空間層次而有所不同。您可以在「耐久性」或「效能」上展開，即可看到它是 {{site.data.keyword.blockstorageshort}}。
