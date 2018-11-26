---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 订购 {{site.data.keyword.blockstorageshort}}

您可以供应 {{site.data.keyword.blockstorageshort}} 并通过微调来满足您的容量和 IOPS 需求。通过两个指定性能的选项，最充分地利用存储器。

- 可以从具有预定义性能级别的耐久性 IOPS 层中进行选择，以适合没有明确定义性能需求的工作负载。
- 可以通过指定性能类型的 IOPS 总数来微调存储器，以满足非常具体的性能需求。

## 订购使用预定义的 IOPS 层的 {{site.data.keyword.blockstorageshort}}（耐久性）

1. 登录到 [IBM Cloud 目录](https://console.bluemix.net/catalog/){:new_window}，然后单击**存储**。接下来，选择 **{{site.data.keyword.blockstorageshort}}**，然后单击**创建**。

   也可以登录到 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}，然后单击**存储** > **{{site.data.keyword.blockstorageshort}}**。单击右上角的**订购 {{site.data.keyword.blockstorageshort}}**。

2. 选择部署**位置**（数据中心）。
   - 确保将新存储器添加到您拥有的计算主机所在位置。
3. 计费。如果选择了具有改进功能的数据中心（标记有星号），那么可以选择“按月计费”或“按小时计费”。
     1. 对于**按小时**计费，在删除 LUN 时或在计费周期结束时（以先发生者为准），将计算块 LUN 在帐户上存在的小时数。对于使用了数天或不足一个月的存储器，按小时计费是不错的选择。按小时计费仅可用于在这些[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内供应的存储器。
     2. 对于**按月**计费，将从创建日期一直到记帐周期结束按比例计算价格并立即记帐。如果在计费周期结束之前删除了块 LUN，那么不会有任何退款。对于所用数据需要长期（一个月或更长时间）存储和访问的生产工作负载，使用按月计费的存储器是不错的选择。

        如果数据中心由于**未**进行更新而不具有改进的功能，那么在缺省情况下，将对在其中供应的存储器使用按月计费类型。
        {:important}
4. 在**新存储器大小**字段中，输入存储器大小。
5. 在**存储器 IOPS 选项**部分中，选择**耐久性（分层 IOPS）**。
6. 选择应用所需的 IOPS 层。
    - **0.25 IOPS/GB** 适用于具有低 I/O 强度的工作负载。这些工作负载的典型特点是同时有很大比例的不活动数据。示例应用包括存储邮箱或部门级别文件共享。
    - **2 IOPS/GB** 适用于最通用的用途。示例应用包括托管支持 Web 应用程序的小型数据库或用于系统管理程序的虚拟机磁盘映像。
    - **4 IOPS/GB** 适用于高强度工作负载。这些工作负载的典型特点是同时有较高比例的活动数据。示例应用包括事务型数据库和其他性能敏感型数据库。
    - **10 IOPS/GB** 适用于要求最苛刻的工作负载，例如由 NoSQL 数据库创建的工作负载以及为 Analytics 进行的数据处理。此层在[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内提供，用于供应的最高达 4 TB 的存储器。
7. 单击**指定快照空间大小**，然后从列表中选择快照大小。这是除了可用空间以外的空间。有关快照空间注意事项和建议，请阅读[订购快照](ordering-snapshots.html)。
8. 从列表中选择**操作系统类型**。<br/>

   此选择基于主机所运行的操作系统，无法日后进行修改。例如，如果服务器为 Ubuntu 或 RHEL，请选择 Linux。如果主机为 Windows 2012 或 Windows 2016 服务器，请从列表中选择 Windows 2008+ 选项。有关各种 Windows 选项的更多信息，请参阅[常见问题解答](faqs.html#which-windows-version-should-i-choose-for-my-block-storage-lun-)。
   {:tip}
9. 在右侧，查看订单摘要，并应用促销码（如果有）。
10. 阅读条款和条件后，选中**我已阅读并同意第三方服务协议**复选框。
11. 单击**创建**。新的存储器分配会在几分钟后可用。

缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 卷。要增加卷的数量，请联系销售代表。请阅读[此处](managing-storage-limits.html)以了解有关增大限制的信息。<br/><br/>有关同时授权的限制，请参阅[常见问题](faqs.html#how-many-instances-can-share-the-use-of-a-block-storage-volume-)。
{:important}

## 订购使用定制 IOPS 的 {{site.data.keyword.blockstorageshort}}（性能）

1. 登录到 [IBM Cloud 目录](https://console.bluemix.net/catalog/){:new_window}，然后单击**存储**。接下来，选择 {{site.data.keyword.blockstorageshort}}，然后单击**创建**。

   也可以登录到 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}，然后单击**存储** > **{{site.data.keyword.blockstorageshort}}**。单击右上角的**订购 {{site.data.keyword.blockstorageshort}}**。
2. 单击**位置**，然后选择数据中心。
   - 确保将新存储器添加到您拥有的计算主机所在位置。
3. 计费。如果选择了具有改进功能的数据中心（标记有星号），那么可以选择“按月计费”或“按小时计费”。
     1. 对于**按小时**计费，在删除 LUN 时或在计费周期结束时（以先发生者为准），将计算块 LUN 在帐户上存在的小时数。对于使用了数天或不足一个月的存储器，按小时计费是不错的选择。按小时计费仅可用于在这些[精选数据中心](new-ibm-block-and-file-storage-location-and-features.html)内供应的存储器。
     2. 对于**按月**计费，将从创建日期一直到记帐周期结束按比例计算价格并立即记帐。如果在计费周期结束之前删除了块 LUN，那么不会有任何退款。对于所用数据需要长期（一个月或更长时间）存储和访问的生产工作负载，使用按月计费的存储器是不错的选择。

        如果数据中心由于**未**进行更新而不具有改进的功能，那么在缺省情况下，将对在其中供应的存储器使用按月计费类型。
        {:note}
4. 在**新存储器大小**字段中，输入存储器大小。
5. 在**存储器 IOPS 选项**部分中，选择**性能（分配的 IOPS）**。
6. 在**性能（分配的 IOPS）**字段中，输入 IOPS。
7. 单击**指定快照空间大小**，然后从列表中选择快照大小。这是除了可用空间以外的空间。有关快照空间注意事项和建议，请阅读[订购快照](ordering-snapshots.html)。
8. 从列表中选择**操作系统类型**。<br/>

   此选择基于主机所运行的操作系统，无法日后进行修改。例如，如果服务器为 Ubuntu 或 RHEL，请选择 Linux。如果主机为 Windows 2012 或 Windows 2016 服务器，请从列表中选择 Windows 2008+ 选项。有关各种 Windows 选项的更多信息，请参阅[常见问题解答](faqs.html#which-windows-version-should-i-choose-for-my-block-storage-lun-)。
   {:tip}
9. 在右侧，查看订单摘要，并应用促销码（如果有）。
10. 阅读条款和条件后，选中**我已阅读并同意第三方服务协议**复选框。
11. 单击**创建**。新的存储器分配会在几分钟后可用。

缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 卷。要增加卷的数量，请联系销售代表。请阅读[此处](managing-storage-limits.html)以了解有关增大限制的信息。<br/><br/>有关同时授权的限制，请参阅[常见问题](faqs.html#how-many-instances-can-share-the-use-of-a-block-storage-volume-)。
{:important}

## 连接新存储器

完成供应请求后，授权主机来访问新存储器并配置连接。根据主机的操作系统，访问相应的链接。
- [在 Linux 上连接到 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上连接到 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上连接到 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [使用 cPanel 配置 Block Storage 进行备份](configure-backup-cpanel.html)
- [使用 Plesk 配置 Block Storage 进行备份](configure-backup-plesk.html)

## 识别发票上的 {{site.data.keyword.blockstorageshort}}

所有 LUN 都将在发票上显示为行项。“耐久性”将显示为“耐久性存储服务”，“性能”将显示为“性能存储服务”。此费率根据您的存储级别而有所不同。可以展开“耐久性”或“性能”，以查看它是否为 {{site.data.keyword.blockstorageshort}}。
