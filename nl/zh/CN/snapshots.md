---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, block storage, snapshot, snapshot space, snapshot best practices, snapshot usage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 快照
{: #snapshots}

快照是 {{site.data.keyword.blockstoragefull}} 的一项功能。快照表示特定时间点的卷内容。利用快照，您可以保护数据而不影响性能且使空间消耗降至最低。快照被视为数据保护的第一道防线。如果用户意外修改或删除了卷中的关键数据，可以轻松、快速地从快照副本复原数据。

{{site.data.keyword.blockstorageshort}} 提供了两种方法来生成快照。

* 一种方法是通过可配置的快照安排，自动为每个存储卷创建和删除快照副本。您还可以根据需求来创建额外快照安排，手动删除副本和管理安排。
* 另一种方法是生成手动快照。

快照副本是 {{site.data.keyword.blockstorageshort}} LUN 的只读图像，用于捕获卷在某个时间点的状态。快照副本在创建所需时间和存储空间方面效率很高。创建 {{site.data.keyword.blockstorageshort}} 快照副本仅需要几秒钟时间。通常不到 1 秒，与卷的大小或存储器上的活动级别无关。创建快照副本后，对数据对象的更改将反映在当前对象版本的更新中，就像快照副本不存在一样。同时，数据的副本会保持稳定。

快照副本不会导致性能下降。用户可以轻松地为每个 {{site.data.keyword.blockstorageshort}} 卷存储最多 50 个安排的快照和 50 个手动快照，所有这些快照都可以作为只读和联机版本的数据进行访问。

使用快照，您可以：

- 以非破坏性方式创建时间点恢复点，
- 将卷还原到先前的时间点。

您必须先为卷购买一些快照空间量才能生成该卷的快照。在初始订购期间或订购后，可以通过**卷详细信息**页面添加快照空间。安排的快照和手动快照共享该快照空间，因此请确保订购足够的快照空间。有关更多信息，请参阅[订购快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)。

## 快照最佳实践

快照设计取决于客户的环境。以下设计注意事项可以帮助您计划和实施快照副本：
- 在每个卷或 LUN 上，通过安排最多可以创建 50 个快照，并且通过手动方式最多可以创建 50 个快照。
- 不要过度频繁地生成快照。通过安排每小时、每天或每周快照，确保安排的快照频率满足 RTO 和 RPO 需求以及应用程序业务需求。
- 可以使用“快照自动删除”来控制存储器消耗量的增长。<br/>

  “自动删除”阈值固定为 95%。
  {:note}

快照并不能替代实际的非现场灾难恢复复制或长期保留备份。
{:important}

## 安全

缺省情况下，加密 {{site.data.keyword.blockstorageshort}} 的所有快照和副本也都已加密。此功能无法逐个卷加以禁用。有关提供者管理的静态加密的更多信息，请参阅[确保数据安全](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)。

## 快照如何影响磁盘空间

快照副本可保留单个块而不是整个文件，因而可最大限度地降低磁盘使用量。仅当更改或删除了活动文件系统中的文件时，快照副本才会使用额外的空间。

在活动文件系统中，更改的块会重写入磁盘上的其他位置，或者作为活动文件块整个除去。在更改或删除文件时，会将原始文件块保留为一个或多个快照副本的一部分。因此，仍会保留原始块使用的磁盘空间，以反映更改前活动文件系统的状态。此外，还会保留修改后活动文件系统中块使用的磁盘空间。


|磁盘空间使用情况|   |
|-----|-----|
| ![生成快照副本前所使用的空间](/images/bfcircle1.png "生成快照副本前")|创建任何快照副本之前，磁盘空间仅由活动文件系统使用。|
| ![生成快照副本时所使用的空间](/images/bfcircle3.png "生成快照副本后")|创建快照副本之后，活动文件系统和快照副本将指向相同磁盘块。快照副本不会使用额外的磁盘空间。|
| ![生成快照副本后某些内容更改时所使用的空间](/images/bfcircle2.png "生成快照副本后的更改")|从活动文件系统中删除 `myfile.txt` 后，快照副本仍包含该文件并引用其磁盘块。这就是删除活动文件系统数据并不一定会释放磁盘空间的原因。|
{: caption="表 1 显示了快照如何影响存储器上的空间使用情况。" caption-side="top"}

有关快照空间使用情况的更多信息，请参阅[管理快照](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)。
