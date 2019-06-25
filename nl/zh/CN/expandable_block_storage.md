---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 扩展 Block Storage 容量
{: #expandingcapacity}

利用此功能，当前 {{site.data.keyword.blockstoragefull}} 用户可以立即将其现有 {{site.data.keyword.blockstorageshort}} 的大小扩展到最大 12 TB（以 GB 为增量）。用户无需创建复制项或将数据手动迁移到更大的卷。在调整大小时，不会发生针对存储器的任何中断或访问权缺乏问题。

对卷的记帐会自动更新，以将新价格的按比例差值添加到当前计费周期。在下一个计费周期中将采用新的完整金额记帐。

此功能在[大多数数据中心](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC)内提供。

## 可扩展存储器的优点

- **成本管理** - 您可能知道自己的数据存在增长的潜力，但一开始需要的存储量较小。通过扩展能力，客户能够节省存储成本，并且以后能根据自己的需求进行增长。  

- **不断增长的存储需求** - 遇到快速数据增长的客户需要一种方法来迅速、轻松地增大其存储器大小，以管理这种增长情况。

## 扩展存储容量对复制的影响

对主存储器执行扩展操作将会自动调整副本大小。

## 限制
{: #limitsofexpandingstorage}

此功能可用于在[大多数数据中心](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC)内供应的存储器。

发布此功能（**2017 年 4 月到 2017 年 12 月 14 日**期间）之前在这些数据中心内供应的存储器最多只能增大到原始大小的 10 倍。在 **2017 年 12 月 14 日**之后供应的存储器可以增大到最高 12 TB。

使用“耐久性”类型供应的 {{site.data.keyword.blockstorageshort}} 的现有大小限制仍然适用（对于 10 IOPS 层，最高为 4 TB，对于其他所有层，最高为 12 TB）。

## 调整存储器大小
{: #resizingsteps}

1. 在 {{site.data.keyword.cloud}} 控制台中，单击**菜单**图标。然后单击**基础架构** > **存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 从列表中选择 LUN，然后单击**操作** > **修改 LUN**。
3. 输入新的存储器大小（以 GB 为单位）。
4. 复查您的选择和新的定价。
5. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。
6. 新的存储器分配会在几分钟后可用。

或者，可以通过 SLCLI 来调整卷的大小。

```
# slcli block volume-modify --help
用法：slcli block volume-modify [OPTIONS] VOLUME_ID

选项：
  -c, --new-size INTEGER        块卷的新大小（以 GB 为单位）。***如果未指定
                                大小，那么将使用卷的原始大小。***
                                可能的大小为：[20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                最小值为：[卷的原始大小]
  -i, --new-iops INTEGER        性能存储器 IOPS，介于 100 到 6000 之间，且为 100 的倍数 [仅针对性能卷]
                                ***如果未指定 IOPS 值，那么将使用该卷的原始 IOPS 值。 ***
                                要求：[如果该卷的原始 IOPS/GB 小于 0.3，那么新 IOPS/GB 也必须小于 0.3。
                                如果该卷的原始 IOPS/GB 大于或者等于 0.3，那么该卷的新 IOPS/GB 也必须
                                大于或者等于 0.3。]
  -t, --new-tier [0.25|2|4|10]  耐久性存储器层 (IOPS/GB) [仅针对耐久性卷]
                                ***如果未指定层，那么将使用该卷的原始层。***
                                要求：[如果该卷的原始 IOPS/GB 为 0.25，那么
                                该卷的新 IOPS/GB 也必须为 0.25。如果该卷的原始 IOPS/GB
                                大于 0.25，那么该卷的新 IOPS/GB 也必须大于 0.25。]
  -h, --help                    显示此消息并退出。
```
{:codeblock}

有关扩展卷上的文件系统（和分区，如果有）以使用新空间的更多信息，请查看操作系统文档。
{:tip}
