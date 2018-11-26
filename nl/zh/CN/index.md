---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-29"

---
{:new_window: target="_blank"}
{:tip: .tip} 
{:note: .note} 
{:important: .important}

# {{site.data.keyword.blockstorageshort}} 入门

{{site.data.keyword.blockstoragefull}} 是独立于计算实例进行供应和管理的持久性高性能 iSCSI 存储器。基于 iSCSI 的 {{site.data.keyword.blockstorageshort}} LUN 通过冗余多路径 I/O (MPIO) 连接来连接到授权设备。

{{site.data.keyword.blockstorageshort}} 通过一组无与伦比的功能，实现了同类最优水平的耐久性和可用性。它使用业界标准和最佳实践进行构建。{{site.data.keyword.blockstorageshort}} 旨在发生维护事件和意外故障期间保护数据完整性并保持可用性，同时提供一致的性能基线。

## 核心功能

可利用 {{site.data.keyword.blockstorageshort}} 的以下功能：

- **一致的性能基线**
   - 通过为各个卷分配协议级别的 IOPS 来提供。
- **持久性高，弹性大**
   - 在发生维护事件和意外故障期间保护数据完整性并保持可用性，而无需创建和管理操作系统级别的独立磁盘冗余阵列 (RAID)。
- **静态数据加密**（[在精选数据中心内提供](new-ibm-block-and-file-storage-location-and-features.html)）
   - 免费对静态数据进行提供者管理的加密。
- **所有支持闪存的存储器**（[在精选数据中心内提供](new-ibm-block-and-file-storage-location-and-features.html)）
   - 用于在 2 IOPS/GB 或更高级别供应使用“耐久性”或“性能”类型供应的卷的所有闪存存储器。
- **快照**（[在精选数据中心内提供](new-ibm-block-and-file-storage-location-and-features.html)）
   - 以非破坏性方式捕获时间点数据快照。
- **复制**（[在精选数据中心内提供](new-ibm-block-and-file-storage-location-and-features.html)）
   - 自动将快照复制到合作伙伴的 {{site.data.keyword.BluSoftlayer_full}} 数据中心。
- **高度可用的连接**
   - 使用冗余网络连接以最大限度提高可用性 
   - 基于 iSCSI 的 {{site.data.keyword.blockstorageshort}} 使用多路径 I/O (MPIO)。
- **并行访问**
   - 允许多个主机同时访问集群配置的块卷（最多 8 个）。
- **集群数据库**
   - 支持高级用例，例如集群数据库。
     
## 计费

可以选择按小时或按月对块 LUN 计费。为 LUN 选择的计费类型将应用于其快照空间和副本。例如，如果供应的 LUN 按小时计费，那么任何快照或副本费用都会按小时记帐。如果供应的 LUN 按月计费，那么任何快照或副本费用都会按月记帐。 

对于**按小时计费**，在删除 LUN 时或在计费周期结束时（以先发生者为准），将计算块 LUN 在帐户上存在的小时数。对于使用了数天或不足一个月的存储器，按小时计费是不错的选择。按小时计费仅可用于在[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内供应的存储器。 

对于**按月计费**，将从创建日期一直到记帐周期结束按比例计算价格并立即记帐。如果在计费周期结束之前删除了 LUN，那么不会有任何退款。对于所用数据需要长期（一个月或更长时间）存储和访问的生产工作负载，使用按月计费的存储器是不错的选择。 

**性能**
<table>
  <caption>表 1 显示了按月计费和按小时计费的性能存储器的价格。</caption>
  <tr>
   <th>每月价格</th>
   <td>0.10 美元/GB + 0.07 美元/IOP</td>
  </tr>
  <tr>
   <th>每小时价格</th>
   <td>0.0001 美元/GB + 0.0002 美元/IOP</td>
  </tr>
</table>
 
**耐久性**
<table>
  <caption>表 2 显示了使用按月和按小时计费选项时每个层的耐久性存储器的价格。</caption>
  <tr>
   <th>IOPS 层</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>每月价格</th>
   <td>0.06 美元/GB</td>
   <td>0.15 美元/GB</td>
   <td>0.20 美元/GB</td>
   <td>0.58 美元/GB</td>
  </tr>
  <tr>
   <th>每小时价格</th>
   <td>0.0001 美元/GB</td>
   <td>0.0002 美元/GB</td>
   <td>0.0003 美元/GB</td>
   <td>0.0009 美元/GB</td>
  </tr>
</table>



## 供应

通过以下两个选项，可以供应从 20 GB 到 12 TB 的 {{site.data.keyword.blockstorageshort}} LUN：<br/>
- 供应**耐久性**层，具有预定义的性能级别和功能，如快照和复制。
- 通过分配的每秒输入/输出操作数 (IOPS) 来构建强大的**性能**环境。 

### 通过耐久性层供应

耐久性 {{site.data.keyword.blockstorageshort}} 有四个 IOPS 性能层，以支持各种不同的应用需求。<br />

- **0.25 IOPS/GB** 适用于具有低 I/O 强度的工作负载。这些工作负载的典型特点是在任意时间都有很大比例的不活动数据。示例应用包括存储邮箱或部门级别文件共享。

- **2 IOPS/GB** 适用于最通用的用途。示例应用包括托管支持 Web 应用程序的小型数据库或用于系统管理程序的 VM 磁盘映像。

- **4 IOPS/GB** 适用于高强度工作负载。这些工作负载的典型特点是在任意时间都有较高比例的活动数据。示例应用包括事务型数据库和其他性能敏感型数据库。

- **10 IOPS/GB** 适用于要求最苛刻的工作负载，例如由 NoSQL 数据库创建的工作负载以及为 Analytics 进行的数据处理。此层仅在[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内提供，用于供应的最高达 4 TB 的存储器。

12 TB 耐久性卷最高提供 48,000 IOPS。
 
为工作负载选择合适的耐久性层非常重要。同样重要的是使用达到最高性能所需的正确块大小、以太网连接速度和主机数。如果其中有任何部分与其他部分不协调，都可能会对产生的吞吐量造成重大影响。

 
### 通过性能供应

“性能”是一类 {{site.data.keyword.blockstorageshort}}，旨在支持具有不太适合“耐久性”层的已知性能需求的高 I/O 应用。通过为各个卷分配协议级别 IOPS，可实现可预测的性能。可以使用大小范围在 20 GB 到 12 TB 的存储器来供应各种 IOPS 速率 (100 - 48,000)。 

{{site.data.keyword.blockstorageshort}} 的“性能”可通过多路径 I/O (MPIO) 因特网小型计算机系统接口 (iSCSI) 连接来访问和安装。在通过单个服务器访问卷时，通常会使用 {{site.data.keyword.blockstorageshort}}。可以在一个主机上安装多个卷，并将这些卷以条带化方式组合在一起，以实现更大的卷和更高的 IOPS 计数。性能卷可以根据表 3 中针对 Linux、XEN 和 Windows 操作系统的大小和 IOPS 速率进行订购。


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>表 3 显示了用于性能存储器的大小和 IOPS 组合。<br/><sup><img src="/images/numberone.png" alt="脚注" /></sup> 超过 6,000 的 IOPS 限制在精选数据中心内提供。</caption>
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
            <td>6,000 或 10,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 或 20,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 或 40,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 或 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
</table>


性能卷旨在以一致地接近所供应 IOPS 级别的方式来运行。通过一致性，能更轻松地调整特定性能级别的应用程序环境的大小和扩展此环境。此外，通过构建具有理想的价格/性能比率的卷，可以优化环境。

### 供应注意事项

**块大小**

“耐久性”和“性能”的 IOPS 基于 16 KB 的块大小，其中读/写随机工作负载的比例为 50/50。一个 16 KB 的块相当于对卷执行一次写操作。
{:important}

应用程序使用的块大小会直接影响存储器性能。如果应用程序使用的块大小小于 16 KB，那么在达到吞吐量限制之前，会先达到 IOPS 限制。相反，如果应用程序使用的块大小大于 16 KB，那么在达到 IOPS 限制之前，会先达到吞吐量限制。

<table>
  <caption>表 4 显示了块大小和 IOPS 如何影响吞吐量的示例。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>块大小 (KB)</th>
            <th>IOPS</th>
            <th>吞吐量（MB/秒）</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4（Linux 的典型值）</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8（Oracle 的典型值）</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32（SQL Server 的典型值）</td>
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

**已授权主机**

另一个要考虑的因素是使用卷的主机数。如果是单个主机在访问卷，那么可能很难实现可用的最大 IOPS，尤其是在极端 IOPS 计数（10,000 以上）的情况下。如果工作负载需要高吞吐量，那么最好配置至少两台服务器来访问卷，以避免出现单服务器瓶颈。

**网络连接**

以太网连接速度必须快于卷的预期最大吞吐量。一般情况下，不要指望以太网连接饱和到超过可用带宽的 70%。例如，如果您有 6,000 IOPS 并且使用的是 16 KB 块大小，那么卷可以处理约 94 MBps 的吞吐量。如果与 LUN 之间存在 1 Gbps 以太网连接，那么当服务器尝试使用最大可用吞吐量时，此连接会成为瓶颈。这是因为 1 Gbps 以太网连接的理论限制（125 MB/秒）的 70% 仅允许 88 MB/秒。

要实现最大 IOPS，需要落实足够的网络资源。其他注意事项包括在存储器外部使用的专用网络、主机端以及特定于应用程序的调整（IP 堆栈或[队列深度](set-host-queue-depth-settings-performance-and-endurance-storage.html)以及其他设置）。

存储流量包含在公共虚拟服务器的总网络使用量之内。要了解服务可能强加的限制，请参阅[虚拟服务器文档](https://console.bluemix.net/docs/vsi/vsi_public.html#public-virtual-servers)。
{:tip}

## 提交订单

准备好提交订单时，请遵循[此处](provisioning-block_storage.html)的指示信息进行操作。

## 连接新存储器

完成供应请求后，授权主机来访问新存储器并配置连接。根据主机的操作系统，访问相应的链接。
- [在 Linux 上连接到 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上连接到 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上连接到 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [使用 cPanel 配置 Block Storage 进行备份](configure-backup-cpanel.html)
- [使用 Plesk 配置 Block Storage 进行备份](configure-backup-plesk.html)

