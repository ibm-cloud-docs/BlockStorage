---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="DomainName"}

# 管理存储限制
{: #managingstoragelimits}

缺省情况下，总共可以全局供应 250 个 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 卷。

如果您不确定拥有多少卷，可以使用以下 `slcli` 命令列出每个数据中心的卷。
```
# slcli block volume-count --help
用法：slcli block volume-count [OPTIONS]

选项：
  -d, --datacenter TEXT  数据中心短名称
  --sortby TEXT          要作为排序依据的列
  -h, --help             显示此消息并退出。
```

您可以通过在[控制台](https://{DomainName}/unifiedsupport/cases/add){: external}中提交支持案例来请求增大限制。请求得到核准后，您将获得针对特定数据中心设置的卷限制。  

要请求增大限制，请打开案例并将其提交给销售代表。

在案例中，请提供以下信息：

- **凭单主题**：请求增大数据中心卷计数存储限制

- **增加卷请求的用例是什么？**<br />
*例如，您的回答可能类似于“新的 VMware 数据存储”、“新的开发和测试（开发/测试）环境”、“SQL 数据库”或“日志记录”。*

- **按类型、大小、IOPS 和位置需要多少额外的块卷？**<br />
*例如，您的回答可能类似于“25 个耐久性 2 TB，4 IOPS，在 DAL09 中”或“25 个性能 4 TB，2 IOPS，在 WDC04 中”。*

- **按类型、大小、IOPS 和位置需要多少额外的文件卷？**<br />
*例如，您的回答可能类似于“25 个性能 20 GB，10 IOPS，在 DAL09 中”或“50 个耐久性 2 TB，0.25 IOPS，在 SJC03 中”。*

- **提供预期或计划供应完所有请求的卷增加量的估计时间。**<br />
*例如，您的回答可能类似于“90 天”。*

- **提供这些卷预期平均容量使用率的 90 天预测。**<br />
*例如，您的回答可能类似于“预期 30 天内的利用率为 25%，60 天内的利用率为 50%，90 天内的利用率为 75%”。*

响应请求中的所有问题和声明。它们是处理和核准所必需的。
{:important}

系统将通过案例流程向您通知对限制的更新。
