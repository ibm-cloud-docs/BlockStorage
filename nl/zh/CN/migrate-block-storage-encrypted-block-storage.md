---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 将现有 {{site.data.keyword.blockstorageshort}} 升级到增强型 {{site.data.keyword.blockstorageshort}}

现在，增强型 {{site.data.keyword.blockstoragefull}} 在精选数据中心内可用。要查看已升级的数据中心和可用功能（例如，可调整 IOPS 速率和可扩展卷）的列表，请单击[此处](new-ibm-block-and-file-storage-location-and-features.html)。有关提供者管理的加密存储器的更多信息，请参阅 [{{site.data.keyword.blockstorageshort}} 静态加密](block-file-storage-encryption-rest.html)文章。

首选迁移路径是同时连接两个 LUN，并将数据从一个 LUN 直接传输到另一个 LUN。具体操作取决于您的操作系统以及在复制操作期间数据是否会更改。 

假定您已将非加密 LUN 连接到主机。如果尚未连接，请按照最适合您操作系统的指示信息来完成此任务：

- [在 Linux 上访问 {{site.data.keyword.blockstorageshort}}](accessing_block_storage_linux.html)
- [在 Windows 上访问 {{site.data.keyword.blockstorageshort}}](accessing-block-storage-windows.html)

 
## 创建新的 {{site.data.keyword.blockstorageshort}}

**重要信息**：使用 API 下单时，请指定“存储即服务”包，以确保获取新存储器的更新功能。

以下指示信息用于通过 UI 订购增强型 LUN。新 LUN 的大小应该等于或大于原始卷的大小，以便于迁移。

### 订购耐久性 LUN

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**存储** > **{{site.data.keyword.blockstorageshort}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构 > 存储 > {{site.data.keyword.blockstorageshort}}**。
2. 在右上角，单击**订购 {{site.data.keyword.blockstorageshort}}**。
3. 从**选择存储器类型**列表中，选择**耐久性**。
4. 选择部署**位置**（数据中心）。
   - 确保将新存储器添加到先前卷所在的位置。
5. 选择您的记帐选项。可以选择“每小时计费”和“每月计费”。
6. 选择 IOPS 层。
7. 单击*选择存储器大小**，然后从列表中选择存储器大小。
8. 单击**指定快照空间大小**，然后从列表中选择快照大小。这是除可用空间以外的空间。有关快照空间注意事项和建议，请阅读[订购快照](ordering-snapshots.html)。
9. 从列表中选择**操作系统类型**。
10. 单击**继续**。这将显示每月费用和按比例的费用，此时您还有最后一次机会复查订单详细信息。
11. 单击**我已阅读主服务协议**复选框，然后单击**下单**。

### 订购性能 LUN

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**存储** > **{{site.data.keyword.blockstorageshort}}**，或者在 {{site.data.keyword.BluSoftlayer_full}}“目录”中，单击**基础架构 > 存储 > {{site.data.keyword.blockstorageshort}}**。
2. 在右上角，单击**订购 {{site.data.keyword.blockstorageshort}}**。
3. 从**选择存储器类型**下拉列表中，选择**性能**。
4. 单击**位置**下拉列表，然后选择数据中心。
   - 确保将新存储器添加到先前订购的主机所在的位置。
5. 选择您的记帐选项。可以选择“每小时计费”和“每月计费”。
6. 选择相应的**存储器大小**旁边的单选按钮。
7. 在**指定 IOPS** 字段中，输入 IOPS。
8. 单击**继续**。这将显示每月费用和按比例的费用，此时您还有最后一次机会复查订单详细信息。如果要更改订单，请单击**上一步**。
9. 单击**我已阅读主服务协议**复选框，然后单击**下单**按钮。


存储器将在不到一分钟的时间内进行供应，并且将在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.blockstorageshort}} 页面上显示。


 
## 将新的 {{site.data.keyword.blockstorageshort}} 连接到主机

“已授权”主机是已向其授予卷访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。授权主机来访问卷会生成用户名、密码和 iSCSI 限定名 (IQN)，这些是安装多路径 I/O (MPIO) iSCSI 连接所需的信息。

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。

2. 滚动到**已授权主机**。

3. 在右侧，单击**授权主机**。选择可以访问该卷的主机。

 
## 快照和复制

是否已为原始 LUN 建立快照和复制？如果是，那么需要使用与原始卷相同的设置，为新的 LUN 设置复制和快照空间以及创建快照安排。 

请注意，如果复制目标数据中心尚未升级用于加密，那么在该数据中心升级之前，您将无法建立对新卷的复制。

 
## 迁移数据

您应该同时连接到原始和新 {{site.data.keyword.blockstorageshort}} LUN。 
- 如果需要有关将这两个 LUN 连接到主机的帮助，请开具支持凭单。

### 数据注意事项

此时，您应该考虑原始 {{site.data.keyword.blockstorageshort}} LUN 上有什么类型的数据，以及如何以最佳方式将其复制到新 LUN。如果您有备份、静态内容以及在复制期间不会更改的内容，那么没有任何重大注意事项。

如果是在 {{site.data.keyword.blockstorageshort}} 上运行数据库或虚拟机，请确保数据在复制期间不会发生变更，以免发生数据损坏。如果您担心任何带宽问题，那么应该在非高峰时段执行迁移。如果需要有关这些注意事项的帮助，请开具支持凭单。
 
### Microsoft Windows

要将数据从原始 {{site.data.keyword.blockstorageshort}} LUN 复制到新 LUN，请设置新存储器的格式并使用 Windows 资源管理器复制文件。

 
### Linux

您可以考虑使用“rsync”来复制数据。下面是示例命令：

```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

建议将上面的命令与 `--dry-run` 标志一起使用，以确保路径正确排列。如果此过程中断，您可能需要删除正在复制的最后一个目标文件，以确保该文件从头开始复制到新位置。

完成不带 `--dry-run` 标志的此命令后，数据应该会复制到新 {{site.data.keyword.blockstorageshort}} LUN。向上滚动并再次运行该命令，以确保没有漏掉任何内容。您还可以手动复查这两个位置，以查找是否有任何可能漏掉的内容。

迁移完成后，您将能够将生产移至新 LUN。然后，可以从配置中拆离原始 LUN 并将其删除。请注意，删除操作还将除去目标站点上与原始 LUN 关联的任何快照或副本。
