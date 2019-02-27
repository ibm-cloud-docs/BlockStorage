---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 调整 IOPS
{: #adjustingIOPS}

利用此新功能，{{site.data.keyword.blockstoragefull}} 存储器用户可以立即调整其现有 {{site.data.keyword.blockstorageshort}} 的 IOPS。用户无需创建复制项或将数据手动复制到新存储器。在进行调整时，用户不会遇到针对存储器的任何类型的中断或访问权缺乏问题。

对存储器的记帐会更新，以将新价格的按比例差值添加到当前计费周期。在下一个计费周期中将采用整个新金额记帐。


## 可调整 IOPS 的优点

- 成本管理 - 某些客户可能只在峰值使用时间内需要高 IOPS。例如，大型零售店在假期使用量达到峰值，因此在假期其存储器上可能需要更高的 IOPS 速率。然而，在仲夏时节并不需要更高的 IOPS。通过此功能，零售店可以管理其成本，并在需要时为更高的 IOPS 付费。

## 限制

此功能仅在[精选数据中心](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)内提供。

客户调整其 IOPS 时，无法在“耐久性”和“性能”之间进行切换。但是，客户可以根据以下条件和限制，为其存储器指定新的 IOPS 层或 IOPS 级别：

- 如果原始卷是 0.25 耐久性层，那么无法更新 IOPS 层。
- 如果原始卷是小于或等于 0.30 IOPS/GB 的性能卷，那么可用的选项仅包括导致小于或等于 0.30 IOPS/GB 的大小和 IOPS 组合。
- 如果原始卷是大于 0.30 IOPS/GB 的性能卷，那么可用的选项仅包括导致大于 0.30 IOPS/GB 的大小和 IOPS 组合。

## IOPS 调整对复制的影响

如果卷已复制到位，那么将自动更新该副本以与主卷的 IOPS 选择相匹配。

## 调整存储器上的 IOPS
{: #steps}

1. 转至 {{site.data.keyword.blockstorageshort}} 的列表。
   - 在 {{site.data.keyword.slportal}} 中，单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
   - 在 {{site.data.keyword.BluSoftlayer_full}} 控制台中，单击**基础架构** > **存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 从列表中选择 LUN，然后单击**操作** > **修改 LUN**。
3. 在**存储器 IOPS 选项**下，进行新的选择：
    - 对于耐久性（分层 IOPS），选择存储器中大于 0.25 IOPS/GB 的 IOPS 层。可以随时增大 IOPS 层。但是，一个月只能减小一次。
    - 对于性能（分配的 IOPS），通过输入 100 到 48,000 IOPS 之间的值，为存储器指定新的 IOPS 选项。

请务必查看订购表单中根据大小所需的任何特定边界。
    {:tip}
4. 复查您的选择和新的定价。
5. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。
6. 新的存储器分配会在几分钟后可用。


或者，可以通过 SLCLI 来调整 IOPS。
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
