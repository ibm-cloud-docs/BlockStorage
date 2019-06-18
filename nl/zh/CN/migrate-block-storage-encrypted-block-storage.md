---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 将现有 {{site.data.keyword.blockstorageshort}} 升级到增强型 {{site.data.keyword.blockstorageshort}}
{: #migratestorage}

现在，增强型 {{site.data.keyword.blockstoragefull}} 在精选数据中心内提供。要查看已升级的数据中心和可用功能（例如，可调整 IOPS 速率和可扩展卷）的列表，请单击[此处](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)。有关提供者管理的加密存储器的更多信息，请参阅 [{{site.data.keyword.blockstorageshort}} 静态加密](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)。

首选迁移路径是同时连接两个 LUN，并将数据从一个 LUN 直接传输到另一个 LUN。具体操作取决于您的操作系统以及在复制操作期间数据是否会更改。

假定您已将非加密 LUN 连接到主机。如果尚未连接，请按照最适合您操作系统的指示信息来完成此任务：

- [在 Linux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

这些数据中心内供应的所有增强型 {{site.data.keyword.blockstorageshort}} 卷的安装点与非加密卷不同。要确保对两个存储卷使用正确的安装点，可以在控制台的**卷详细信息**页面中查看安装点信息。还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。
{:tip}

## 创建 {{site.data.keyword.blockstorageshort}}

使用 API 下订单时，请指定“存储即服务”包，以确保获取新存储器的更新功能。
{:important}

您可以通过 IBM Cloud 控制台来订购增强型 LUN。新 LUN 的大小应该等于或大于原始卷的大小，以便于迁移。

- [订购具有预定义 IOPS 层（耐久性）的 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsoleEndurance)
- [订购具有定制 IOPS（性能）的 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsolePerformance)

几分钟后即可安装新存储器。在资源列表和 {{site.data.keyword.blockstorageshort}} 列表中，可以查看该存储器。

## 将新的 {{site.data.keyword.blockstorageshort}} 连接到主机

“已授权”主机是已向其授予卷访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。授权主机来访问卷会生成用户名、密码和 iSCSI 限定名 (IQN)，这些是安装多路径 I/O (MPIO) iSCSI 连接所需的信息。

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。
2. 滚动到**已授权主机**。
3. 在右侧，单击**授权主机**。选择可以访问该卷的主机。


## 快照和复制

是否已为原始 LUN 建立快照和复制？如果是，那么需要使用与原始卷相同的设置，为新的 LUN 设置复制和快照空间以及创建快照安排。

如果复制目标数据中心尚未升级，那么在该数据中心升级之前，您将无法建立对新卷的复制。


## 迁移数据

1. 同时连接到原始和新 {{site.data.keyword.blockstorageshort}} LUN。

   如果需要有关将这两个 LUN 连接到主机的帮助，请开立支持案例。
   {:tip}

2. 请考虑原始 {{site.data.keyword.blockstorageshort}} LUN 上有什么类型的数据，以及如何以最佳方式将其复制到新 LUN。
  - 如果您有备份、静态内容以及您不希望在复制期间发生更改的内容，那么无需太担心。
  - 如果是在 {{site.data.keyword.blockstorageshort}} 上运行数据库或虚拟机，请确保数据在复制期间不会发生变更，以免发生数据损坏。
  - 如果您担心任何带宽问题，请在非高峰时段执行迁移。
  - 如果需要有关这些注意事项的帮助，请开立支持案例。

3. 复制数据。
   - 对于 **Microsoft Windows**，格式化新存储器，并通过使用 Windows 资源管理器将数据从原始 {{site.data.keyword.blockstorageshort}} LUN 复制到新 LUN。
   - 对于 **Linux**，可以使用 `rsync` 来复制数据。
   ```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

   最好将上一个命令与 `--dry-run` 标志一起使用一次，以确保路径正确排列。如果此过程中断，您可以删除正在复制的最后一个目标文件，以确保该文件从头开始复制到新位置。<br/>
   完成不带 `--dry-run` 标志的此命令后，数据会复制到新的 {{site.data.keyword.blockstorageshort}} LUN。再次运行此命令，以确保没有漏掉任何内容。您还可以手动复查这两个位置，以查找是否有任何可能漏掉的内容。<br/>
   迁移完成后，您可以将生产移至新的 LUN。然后，可以从配置中拆离原始 LUN 并将其删除。删除操作还将除去目标站点上与原始 LUN 关联的任何快照或副本。
