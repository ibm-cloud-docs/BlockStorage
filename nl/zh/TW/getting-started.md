---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-08"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# 入門指導教學
{: #getting-started}

{{site.data.keyword.blockstoragefull}} 是持續性的高效能 iSCSI 儲存空間，其獨立於運算實例之外進行佈建及管理。iSCSI 型 {{site.data.keyword.blockstorageshort}} LUN 是透過備用的多路徑 I/O (MPIO) 連線連接至授權裝置。

{{site.data.keyword.blockstorageshort}} 透過無與倫比的特性集提供最佳的延續性和可用性層次。它是使用業界標準和最佳作法進行建置。{{site.data.keyword.blockstorageshort}} 的設計目的為保護資料完整性，以及透過維護事件和非計劃性失敗來維護可用性，同時提供一致的效能基準線。{:shortdesc}

## 開始之前
{: #prereqs}

您可以使用以下兩種選項，來佈建 20 GB 到 12 TB 的 {{site.data.keyword.blockstorageshort}} LUN：<br/>
- 佈建**耐久性層級**，其特色是預先定義的效能層次，以及例如 Snapshot 及抄寫等其他特性。
- 建置具有已配置每秒輸入/輸出作業 (IOPS) 的高功率**效能**環境。

如需 {{site.data.keyword.blockstorageshort}} 供應項目的相關資訊，請參閱[關於 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About)。

## 佈建考量

### 區塊大小

「耐久性」及「效能」的 IOPS 都是根據 16 KB 區塊大小（具有 50/50 讀寫 50/50 隨機/循序工作負載）。16 KB 區塊相當於寫入一次磁區。
{:important}

應用程式所使用的區塊大小會直接影響儲存空間效能。如果應用程式所使用的區塊大小小於 16 KB，則 IOPS 限制會比傳輸量限制更早實現。反之，如果應用程式所使用的區塊大小大於 16 KB，則傳輸量限制會比 IOPS 限制更早實現。

|區塊大小 (KB)|IOPS|傳輸量（MB/秒）|
|-----|-----|-----|
|4|1,000|16|
|8|1,000|16|
|16|1,000|16|
|32|500|16|
|64|250|16|
|128|128|16|
|512|32|16|
| 1024 |16|16|
{: caption="表 1 顯示區塊大小和 IOPS 如何影響傳輸量的範例。<br/>平均 IO 大小 x IOPS = 傳輸量（以 MB/s 為單位）。" caption-side="top"}

### 授權的主機

另一個要考量的因素是使用磁區的主機數目。如果有單一主機正在存取該磁區，可能難以實現最大可用 IOPS，特別是極端的 IOPS 計數 (10,000)。如果您的工作負載需要高傳輸量，則最好至少將一些伺服器配置為存取您的磁區，以避免單一伺服器瓶頸。

### 網路連線

乙太網路連線的速度必須比來自您磁區的預期最大傳輸量更快。一般而言，請不要預期乙太網路連線飽和度超過可用頻寬的 70%。例如，如果您有 6,000 IOPS 而且要使用 16 KB 區塊大小，則磁區能處理大約 94 MBps 的傳輸量。如果您的 LUN 有一條 1 Gbps 乙太網路連線，則當伺服器嘗試使用最大可用傳輸量時，它會變成瓶頸。原因是 1 Gbps 乙太網路連線理論限制（每秒 125 MB）的 70% 只容許每秒 88 MB。

為達到最大 IOPS，需要有足夠的網路資源。其他考量包括儲存空間之外的專用網路使用情形，以及主機端和應用程式特定的調整（IP 堆疊或[佇列深度](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings)，以及其他設定）。

儲存空間資料流量應該與其他資料流量類型隔離，且不應該透過防火牆和路由器導向。如需相關資訊，請參閱[常見問題](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic)。

儲存空間資料流量包含在「公用虛擬伺服器」的總網路使用情形中。若要進一步瞭解該服務可能強制的限制，請參閱[虛擬伺服器文件](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers)。
{:tip}

## 提交訂單
{: #submitorder}

當您準備好提交訂單時，可以透過[主控台](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole)或 [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI) 下訂單。

## 連接新的儲存空間
{: #mountingstorage}

當您的佈建要求完成時，請授權主機存取新的儲存空間，並配置連線。根據主機的作業系統而定，遵循適當的鏈結。
- [在 Linux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上連接至 LUNS](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [配置 Block Storage 以便使用 cPanel 進行備份](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [配置 Block Storage 以便使用 Plesk 進行備份](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## 管理新的儲存空間

透過入口網站或 SLCLI，您可以管理 File Storage 的各種層面，例如主機授權和取消。如需相關資訊，請參閱[管理 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)。
