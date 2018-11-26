---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 快照

快照是 {{site.data.keyword.blockstoragefull}} 的一项功能。快照表示特定时间点的卷内容。通过快照，既能保护数据，又不影响性能，而且使用空间量最小，因此快照被视为数据保护的第一道防线。如果用户意外修改或删除了卷中的关键数据，可以轻松、快速地从快照副本复原数据。

{{site.data.keyword.blockstorageshort}} 提供了两种方法来生成快照。

* 一种方法是通过可配置的快照安排，自动为每个存储卷创建和删除快照副本。您还可以根据需求来创建额外快照安排，手动删除副本和管理安排。
* 另一种方法是生成手动快照。

快照副本是 {{site.data.keyword.blockstorageshort}} LUN 的只读图像，用于捕获卷在某个时间点的状态。快照副本在创建所需时间和存储空间方面效率很高。创建 {{site.data.keyword.blockstorageshort}} 快照副本仅需要几秒钟时间。通常不到 1 秒，与卷的大小或存储器上的活动级别无关。创建快照副本后，对数据对象的更改将反映在当前对象版本的更新中，就像快照副本不存在一样。同时，数据的副本会保持稳定。

快照副本不会导致性能下降。用户可以轻松地为每个 {{site.data.keyword.blockstorageshort}} 卷存储最多 50 个安排的快照和 50 个手动快照，所有这些快照都可以作为只读和联机版本的数据进行访问。

使用快照，您可以：

- 以非破坏性方式创建时间点恢复点，
- 将卷还原到先前的时间点。

您必须先为卷购买一些快照空间量才能生成该卷的快照。在初始订购期间或订购后，可以通过**卷详细信息**页面添加快照空间。安排的快照和手动快照共享该快照空间，因此请确保订购足够的快照空间。有关更多信息，请参阅[订购快照](ordering-snapshots.html)。

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

缺省情况下，加密 {{site.data.keyword.filestorage_short}} 的所有快照和副本也都已加密。此功能无法逐个卷加以禁用。有关提供者管理的静态加密的更多信息，请参阅[确保数据安全](block-file-storage-encryption-rest.html)。

## 快照如何影响磁盘空间

快照副本通过保留单个块而不是整个文件，最大限度减少磁盘空间使用量。仅当更改或删除了活动文件系统中的文件时，快照副本才会使用额外的空间。发生这种情况时，原始文件块仍保留为一个或多个快照副本的一部分。


在活动文件系统中，更改的块会重写入磁盘上的其他位置，或者作为活动文件块整个除去。因此，不仅会保留修改后的活动文件系统中的块所使用磁盘空间，还仍然会保留原始块使用的磁盘空间，以反映出更改前活动文件系统的状态。

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">生成快照副本前后的磁盘空间使用情况</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="生成快照副本前"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="生成快照副本后"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="生成快照副本后更改"></td>
     </tr><tr>
        <td style="border: 0.0px;">创建任何快照副本之前，磁盘空间仅由活动文件系统使用。</td>
        <td style="border: 0.0px;">创建快照副本之后，活动文件系统和快照副本将指向相同磁盘块。快照副本不会使用额外的磁盘空间。</td>
        <td style="border: 0.0px;">从活动文件系统中删除 <i>myfile.txt</i> 后，快照副本仍包含该文件并引用其磁盘块。这就是删除活动文件系统数据并不一定会释放磁盘空间的原因。</td>
      </tr>
</table>

有关如何查看已使用的快照空间量的更多信息，请参阅[管理快照](working-with-snapshots.html)。
