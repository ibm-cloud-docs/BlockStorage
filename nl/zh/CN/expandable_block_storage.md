---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-12"

---
{:new_window: target="_blank"}

# 扩展 Block Storage 容量

利用此新功能，当前 {{site.data.keyword.blockstoragefull}} 用户可以立即将其现有 {{site.data.keyword.blockstorageshort}} 的大小扩展到最大 12 TB（以 GB 为增量）。用户无需创建复制项或将数据手动迁移到更大的卷。在调整大小时，不会发生针对存储器的任何中断或访问权缺乏问题。 

对卷的记帐会自动更新，以将新价格的按比例差值添加到当前计费周期。在下一个计费周期中将采用新的完整金额记帐。

此功能在[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内提供。 

## 可扩展存储器的优点

- **成本管理** - 您可能知道自己的数据存在增长的潜力，但一开始需要的存储量较小。通过扩展能力，客户能够节省存储成本，并能根据自己的需求进行增长。  

- **不断增长的存储需求** - 遇到快速数据增长的客户需要一种方法来迅速、轻松地增大其存储器大小，以管理这种增长情况。

## 扩展存储容量对复制的影响

对主存储器执行扩展操作将会自动调整副本大小。 

## 限制

此功能可用于在[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内供应的存储器。 

在此功能发布之前（**2017 年 4 月到 2017 年 12 月 14 日**期间），在这些数据中心内供应的存储器最多只能增大到原始大小的 10 倍。在 **2017 年 12 月 14 日**之后供应的存储器可以增大到最高 12 TB。 

使用“耐久性”类型供应的 {{site.data.keyword.blockstorageshort}} 的现有大小限制仍然适用（对于 10 IOPS 层，最高为 4 TB，对于其他所有层，最高为 12 TB）。

## 调整存储器大小

1. 在 {{site.data.keyword.slportal}} 中，单击**存储** > **{{site.data.keyword.blockstorageshort}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构** > **存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 从列表中选择 LUN，然后单击**操作** > **修改 LUN**。
3. 输入新的存储器大小（以 GB 为单位）。
4. 复查您的选择和新的定价。
5. 单击**我已阅读主服务协议...** 复选框，然后单击**下订单**。
6. 新的存储器分配会在几分钟后可用。
