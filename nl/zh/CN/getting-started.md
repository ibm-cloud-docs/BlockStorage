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

# 入门教程
{: #getting-started}

{{site.data.keyword.blockstoragefull}} 是独立于计算实例进行供应和管理的持久性高性能 iSCSI 存储器。基于 iSCSI 的 {{site.data.keyword.blockstorageshort}} LUN 通过冗余多路径 I/O (MPIO) 连接来连接到授权设备。

{{site.data.keyword.blockstorageshort}} 通过一组无与伦比的功能，实现了同类最优水平的耐久性和可用性。它使用业界标准和最佳实践进行构建。{{site.data.keyword.blockstorageshort}} 旨在发生维护事件和意外故障期间保护数据完整性并保持可用性，同时提供一致的性能基线。
{:shortdesc}

## 开始之前
{: #prereqs}

通过以下两个选项，可以供应从 20 GB 到 12 TB 的 {{site.data.keyword.blockstorageshort}} LUN：<br/>
- 供应**耐久性**层，具有预定义的性能级别和功能，如快照和复制。
- 通过分配的每秒输入/输出操作数 (IOPS) 来构建强大的**性能**环境。

有关 {{site.data.keyword.blockstorageshort}} 产品的更多信息，请参阅[关于 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About)。

## 供应注意事项

### 块大小

“耐久性”和“性能”的 IOPS 基于 16 KB 的块大小，其中读/写、随机/顺序工作负载的比例为 50/50。一个 16 KB 的块相当于对卷执行一次写操作。
{:important}

应用程序使用的块大小会直接影响存储器性能。如果应用程序使用的块大小小于 16 KB，那么在达到吞吐量限制之前，会先达到 IOPS 限制。相反，如果应用程序使用的块大小大于 16 KB，那么在达到 IOPS 限制之前，会先达到吞吐量限制。

|块大小 (KB)|IOPS|吞吐量（MB/秒）|
|-----|-----|-----|
|4|1,000|16|
|8|1,000|16|
|16|1,000|16|
|32|500|16|
|64|250|16|
|128|128|16|
|512|32|16|
|1024|16|16|
{: caption="表 1 显示了块大小和 IOPS 如何影响吞吐量的示例。<br/>平均 IO 大小 x IOPS = 吞吐量 (MB/s)。" caption-side="top"}

### 已授权主机

另一个要考虑的因素是使用卷的主机数。如果是单个主机在访问卷，那么可能很难实现可用的最大 IOPS，尤其是在极端 IOPS 计数（10,000 以上）的情况下。如果工作负载需要高吞吐量，那么最好配置至少两台服务器来访问卷，以避免出现单服务器瓶颈。

### 网络连接

以太网连接速度必须快于卷的预期最大吞吐量。一般情况下，不要指望以太网连接饱和到超过可用带宽的 70%。例如，如果您有 6,000 IOPS 并且使用的是 16 KB 块大小，那么卷可以处理约 94 MBps 的吞吐量。如果与 LUN 之间存在 1 Gbps 以太网连接，那么当服务器尝试使用最大可用吞吐量时，此连接会成为瓶颈。这是因为 1 Gbps 以太网连接的理论限制（125 MB/秒）的 70% 仅允许 88 MB/秒。

要实现最大 IOPS，需要落实足够的网络资源。其他注意事项包括在存储器外部使用的专用网络、主机端以及特定于应用程序的调整（IP 堆栈或[队列深度](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings)以及其他设置）。

存储流量应与其他流量类型隔离，不得通过防火墙和路由器进行定向。有关更多信息，请参阅[常见问题](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic)。

存储流量包含在公共虚拟服务器的总网络使用量之内。有关服务可能施加的限制的更多信息，请参阅[虚拟服务器文档](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers)。
{:tip}

## 提交订单
{: #submitorder}

准备好提交订单时，可以通过[控制台](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole)或 [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI) 来完成此操作。

## 连接新存储器
{: #mountingstorage}

完成供应请求后，授权主机来访问新存储器并配置连接。根据主机的操作系统，访问相应的链接。
- [在 Linux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [使用 cPanel 配置 Block Storage 进行备份](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [使用 Plesk 配置 Block Storage 进行备份](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## 管理新存储器

通过门户网站或 SLCLI，可以管理 File Storage 的各个方面，例如主机授权和取消。有关更多信息，请参阅[管理 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)。
