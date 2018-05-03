---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 订购 {{site.data.keyword.blockstorageshort}}

根据您的需求和偏好，可以供应两种不同类型的存储器。{{site.data.keyword.blockstorageshort}} 卷的两个选项是： 

- **耐久性**：供应具有预定义性能级别和功能（如快照和复制）的耐久性层。 
- **性能**：构建强大的高性能环境，在其中可以分配所需的每秒输入/输出操作数 (IOPS)。

## 如何为 {{site.data.keyword.blockstorageshort}} 订购“耐久性”

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**存储** > **{{site.data.keyword.blockstorageshort}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构 > 存储 > {{site.data.keyword.blockstorageshort}}**。
2. 在右上角，单击**订购 {{site.data.keyword.blockstorageshort}}**链接。
3. 从“选择存储类型”下拉列表中，选择**耐久性**。
4. 单击下拉列表，然后选择部署**位置**（数据中心）。
   - 确保将新存储器添加到先前订购的主机所在的位置。
   - 如果选择了具有改进功能的数据中心（在下拉列表中用 * 表示），那么可以选择“按月计费”或“按小时计费”。 
     1. 对于**按小时**计费，在删除 LUN 时或在计费周期结束时（以先发生者为准），将计算块 LUN 在帐户上存在的小时数。对于使用了数天或不足一个月的存储器，按小时计费是不错的选择。按小时计费仅可用于在这些[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内供应的存储器。 
     2. 对于**按月**计费，将从创建日期一直到记帐周期结束按比例计算价格并立即记帐。如果在计费周期结束之前删除了块 LUN，那么不会有任何退款。对于所用数据需要长期（一个月或更长时间）存储和访问的生产工作负载，存储器选择按月计费是不错的选择。**注**：缺省情况下，对于在**未**更新为使用改进功能的数据中心内供应的存储器，将使用按月计费类型。
5. 选择您的应用所需的 IOPS 层旁边的单选按钮。
    - *0.25 IOPS/GB* 适用于具有低 I/O 强度的工作负载。这些工作负载的典型特点是在给定时间有很大比例的不活动数据。示例应用包括存储邮箱或部门级别文件共享。
    - *2 IOPS/GB* 适用于最通用的用途。示例应用包括托管支持 Web 应用程序的小型数据库或用于系统管理程序的虚拟机磁盘映像。
    - *4 IOPS/GB* 适用于高强度工作负载。这些工作负载的典型特点是在给定时间有较高比例的活动数据。示例应用包括事务型数据库和其他性能敏感型数据库。
    - *10 IOPS/GB* 适用于要求最苛刻的工作负载，例如由 NoSQL 数据库创建的工作负载以及为 Analytics 进行的数据处理。此层在[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内可用于供应的大小最高达 4 TB 的存储器。
6. 单击**选择存储器大小**下拉列表，然后选择存储器大小。
7. 单击**指定快照空间大小**下拉列表，然后选择快照大小（除了可用空间外）。有关快照空间注意事项和建议，请阅读[订购快照](ordering-snapshots.html)。
8. 从下拉列表中选择**操作系统类型**。
9. 单击下拉列表，然后选择部署“位置”（数据中心）。
10. 单击**继续**。这将显示每月费用和按比例的费用，此时您还有最后一次机会复查订单详细信息。
11. 单击**我已阅读主服务协议**复选框，然后单击**下单**按钮。
12. 新的存储器分配应该会在几分钟后可用。

**注**：缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 卷。要增加卷的数量，请联系销售代表。请阅读[此处](managing-storage-limits.html)以了解有关增大限制的信息。

有关同时授权的限制，请参阅[常见问题](BlockStorageFAQ.html)
 
## 如何为 Block Storage 订购“性能”

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**存储** > **{{site.data.keyword.blockstorageshort}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构 > 存储 > {{site.data.keyword.blockstorageshort}}**。
2. 单击屏幕右上角的**订购 {{site.data.keyword.blockstorageshort}}**。
3. 从**选择存储器类型**下拉列表中，选择**性能**。
4. 单击**位置**下拉列表，然后选择数据中心。
   - 确保将新存储器添加到先前订购的主机所在的位置。
   - 如果选择了具有改进功能的数据中心（在下拉列表中用 * 表示），那么可以选择“按月计费”或“按小时计费”。 
     1. 对于**按小时**计费，在删除 LUN 时或在计费周期结束时（以先发生者为准），将计算块 LUN 在帐户上存在的小时数。对于使用了数天或不足一个月的存储器，按小时计费是不错的选择。按小时计费仅可用于在这些[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内供应的存储器。 
     2. 对于**按月**计费，将从创建日期一直到记帐周期结束按比例计算价格并立即记帐。如果在计费周期结束之前删除了块 LUN，那么不会有任何退款。对于所用数据需要长期（一个月或更长时间）存储和访问的生产工作负载，存储器选择按月计费是不错的选择。**注**：缺省情况下，对于在**未**更新为使用改进功能的数据中心内供应的存储器，将使用按月计费类型。
5. 选择相应的**存储器大小**旁边的单选按钮。
6. 在**指定 IOPS** 字段中，输入 IOPS。
7. 单击**继续**。这将显示每月费用和按比例的费用，此时您还有最后一次机会复查订单详细信息。如果要更改订单，请单击**上一步**。
8. 单击**我已阅读主服务协议**复选框，然后单击**下单**按钮。
9. 新的存储器分配应该会在几分钟后可用。

**注**：缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 卷。要增加卷的数量，请联系销售代表。请阅读[此处](managing-storage-limits.html)以了解有关增大限制的信息。

有关同时授权的限制，请参阅[常见问题](BlockStorageFAQ.html)

## 如何在发票上识别 {{site.data.keyword.blockstorageshort}}

所有 LUN 都将在发票上显示为行项；耐久性将显示为“耐久性存储服务”，“性能”将显示为“性能存储服务”。费率将根据您的存储级别而变化。然后，展开“耐久性”或“性能”即可看到它是 {{site.data.keyword.blockstorageshort}}。
