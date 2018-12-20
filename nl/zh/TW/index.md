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

# 開始使用 {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} 是持續性的高效能 iSCSI 儲存空間，其獨立於運算實例之外進行佈建及管理。iSCSI 型 {{site.data.keyword.blockstorageshort}} LUN 是透過備用的多路徑 I/O (MPIO) 連線連接至授權裝置。

{{site.data.keyword.blockstorageshort}} 透過無與倫比的特性集提供最佳的延續性和可用性層次。它是使用業界標準和最佳作法進行建置。{{site.data.keyword.blockstorageshort}} 的設計目的為保護資料完整性，以及透過維護事件和非計劃性失敗來維護可用性，同時提供一致的效能基準線。

## 核心特性

利用 {{site.data.keyword.blockstorageshort}} 的下列特性：

- **一致效能基準線**
   - 透過將通訊協定層次 IOPS 配置給個別磁區來提供。
- **高度可延續且具復原力**
   - 保護資料完整性，並透過維護事件及非計劃性故障來維護可用性，而不需要建立及管理作業系統層次的獨立磁碟備用陣列 (RAID) 陣列。
- **靜態資料加密**（[適用於精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)）
   - 靜態資料的提供者管理加密，不需額外付費。
- **全快閃記憶體支援的儲存空間**（[適用於精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)）
   - 已佈建 2 IOPS/GB 或以上層次之「耐久性」或「效能」的磁區的全快閃記憶體儲存空間。
- **Snapshot**（[適用於精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)）
   - 以不中斷的方式擷取時間點資料 Snapshot。
- **抄寫**（[適用於精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)）
   - 自動將 Snapshot 複製到夥伴 {{site.data.keyword.BluSoftlayer_full}} 資料中心。
- **高度可用的連線功能**
   - 使用備用網路連線以讓可用性最大化
   - iSCSI 型 {{site.data.keyword.blockstorageshort}} 會使用「多路徑 I/O (MPIO)」。
- **並行存取**
   - 容許多台主機同時針對叢集配置存取區塊磁區（最多八台）。
- **叢集資料庫**
   - 支援進階使用案例（例如叢集資料庫）。

## 計費

您可以為「區塊 LUN」選取按小時計費或按月計費。為 LUN 選取的計費類型會套用至其 Snapshot 空間及抄本。例如，如果您佈建按小時計費的 LUN，則任何 Snapshot 或抄本費用都會按小時計費。如果您佈建按月計費的 LUN，則任何 Snapshot 或抄本費用都會按月計費。

使用**按小時計費**，會在刪除 LUN 或計費週期結束時（看何者為先），計算區塊 LUN 存在於帳戶上的小時數。如果儲存空間使用期間為幾天或不到一整個月，則按小時計費是一個良好的選擇。按小時計費只適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的儲存空間。

使用**按月計費**，價格是從建立日期到計費週期結束為止，按比例計算，並立即計費。如果在計費週期結束之前刪除 LUN，則不會退款。如果儲存空間用於正式作業工作負載，而正式作業工作負載使用需要長期（一個月或更久）儲存及存取的資料，則按月計費是一個良好的選擇。

**效能**
<table>
  <caption>表 1 顯示「效能儲存空間」按月及按小時計費的價格。</caption>
  <tr>
   <th>每月價格</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>每小時價格</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>

**耐久性**
<table>
  <caption>表 2 顯示「耐久性儲存空間」每個層級的按月及按小時計費選項的價格。</caption>
  <tr>
   <th>IOPS 層級</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>每月價格</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0.20/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>每小時價格</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



## 佈建

您可以使用以下兩種選項，來佈建 20 GB 到 12 TB 的 {{site.data.keyword.blockstorageshort}} LUN：<br/>
- 佈建**耐久性層級**，其特色是預先定義的效能層次，以及例如 Snapshot 及抄寫等其他特性。
- 建置具有已配置每秒輸入/輸出作業 (IOPS) 的高功率**效能**環境。

### 使用耐久性層級進行佈建

耐久性 {{site.data.keyword.blockstorageshort}} 提供四種 IOPS 效能層級，以支援各種應用程式需求。<br />

- **每 GB 0.25 IOPS** 是為了低 I/O 強度的工作負載而設計。這些工作負載的特點通常是隨時會有大百分比的非作用中資料。應用程式範例包括儲存信箱或部門層次檔案共用。

- **每 GB 2 IOPS** 是針對最通用用途而設計。應用程式範例包括管理小型資料庫，支持 Web 應用程式或 Hypervisor 的虛擬機器磁碟映像檔。

- **每 GB 4 IOPS** 是為了較高強度工作負載而設計。這些工作負載的特點通常是隨時會有高百分比的作用中資料。應用程式範例包括交易式資料庫及其他效能相關的資料庫。

- **每 GB 10 IOPS** 是為了最嚴苛的工作負載（例如 NoSQL 資料庫所建立的工作負載）以及進行分析的資料處理而設計。此層級只適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內最多佈建 4 TB 的儲存空間。

12 TB 耐久性磁區最多提供 48,000 IOPS。

為您的工作負載選擇正確的「耐久性」層級很重要。使用達到最高效能所需的正確區塊大小、乙太網路連線速度及主機數目也同樣重要。如果這其中有任何部分與其他部分不一致，則會對產生的傳輸量有重大的影響。


### 使用效能進行佈建

「效能」是一種 {{site.data.keyword.blockstorageshort}} 類別，其設計旨在支援高 I/O 應用程式，而這些應用程式具有已深入瞭解且不適合「耐久性」層級的效能需求。透過將通訊協定層次 IOPS 配置到個別磁區，即可達成可預測效能。佈建範圍從 20 GB 到 12 TB 的儲存空間大小時可以使用範圍從 100 - 48,000 的各種 IOPS 速率。

{{site.data.keyword.blockstorageshort}} 的「效能」會透過「多路徑 I/O (MPIO) 網際網路小型電腦系統介面 (iSCSI)」連線存取及裝載。{{site.data.keyword.blockstorageshort}} 通常用於單一伺服器存取磁區的情況。可以將多個磁區裝載至主機，並將其分段合在一起，以達到較大的磁區及更高的 IOPS 計數。您可以根據 Linux、XEN 及 Windows 作業系統的「表 3」中的大小和 IOPS 速率來訂購「效能」磁區。


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>表 3 顯示效能儲存空間的大小和 IOPS 組合。<br/><sup><img src="/images/numberone.png" alt="註腳" /></sup> 精選資料中心內提供大於 6,000 的 IOPS 限制。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>大小 (GB)</th>
            <th>最小 IOPS</th>
            <th>最大 IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 或 10,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 或 20,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 或 40,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="註腳" /></sup></td>
          </tr>
</table>


效能磁區的設計旨在以持續接近已佈建的 IOPS 層次運作。一致性可讓您更輕鬆地為具有特定效能層次的應用程式環境調整大小與進行擴充。此外，可以透過建置具有理想價格與效能比的磁區，來進行環境的最佳化。

### 佈建考量

**區塊大小**

「耐久性」及「效能」的 IOPS 都是根據 16 KB 區塊大小（具有 50/50 讀寫 50% 隨機工作負載）。16 KB 區塊相當於寫入一次磁區。
{:important}

應用程式所使用的區塊大小會直接影響儲存空間效能。如果應用程式所使用的區塊大小小於 16 KB，則 IOPS 限制會比傳輸量限制更早實現。反之，如果應用程式所使用的區塊大小大於 16 KB，則傳輸量限制會比 IOPS 限制更早實現。

<table>
  <caption>表 4 顯示區塊大小和 IOPS 如何影響傳輸量的範例。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>區塊大小 (KB)</th>
            <th>IOPS</th>
            <th>傳輸量（MB/秒）</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4（一般用於 Linux）</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8（一般用於 Oracle）</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32（一般用於 SQL Server）</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**授權的主機**

另一個要考量的因素是使用磁區的主機數目。如果有單一主機正在存取該磁區，可能難以實現最大可用 IOPS，特別是極端的 IOPS 計數 (10,000)。如果您的工作負載需要高傳輸量，則最好至少將一些伺服器配置為存取您的磁區，以避免單一伺服器瓶頸。

**網路連線**

乙太網路連線的速度必須比來自您磁區的預期最大傳輸量更快。一般而言，請不要預期乙太網路連線飽和度超過可用頻寬的 70%。例如，如果您有 6,000 IOPS 而且要使用 16 KB 區塊大小，則磁區能處理大約 94 MBps 的傳輸量。如果您的 LUN 有一條 1 Gbps 乙太網路連線，則當伺服器嘗試使用最大可用傳輸量時，它會變成瓶頸。原因是 1 Gbps 乙太網路連線理論限制（每秒 125 MB）的 70% 只容許每秒 88 MB。

為達到最大 IOPS，需要有足夠的網路資源。其他考量包括儲存空間之外的專用網路使用情形，以及主機端和應用程式特定的調整（IP 堆疊或[佇列深度](set-host-queue-depth-settings-performance-and-endurance-storage.html)，以及其他設定）。

儲存空間資料流量包含在「公用虛擬伺服器」的總網路使用情形中。若要進一步瞭解該服務可能強制的限制，請參閱[虛擬伺服器文件](https://{DomainName}/docs/vsi/vsi_public.html#public-virtual-servers)。
{:tip}

## 提交訂單

當您準備好提交訂單時，請遵循[這裡](provisioning-block_storage.html)的指示。

## 連接新的儲存空間

當您的佈建要求完成時，請授權主機存取新的儲存空間，並配置連線。根據主機的作業系統而定，遵循適當的鏈結。
- [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上連接至 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [配置 Block Storage 以便使用 cPanel 進行備份](configure-backup-cpanel.html)
- [配置 Block Storage 以便使用 Plesk 進行備份](configure-backup-plesk.html)
