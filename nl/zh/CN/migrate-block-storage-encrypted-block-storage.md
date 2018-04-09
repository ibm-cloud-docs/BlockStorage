---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 将 {{site.data.keyword.blockstorageshort}} 迁移到加密 {{site.data.keyword.blockstorageshort}}

现在精选数据中心内提供有针对耐久性或性能的加密 {{site.data.keyword.blockstoragefull}}。下面您将了解有关如何将 {{site.data.keyword.blockstorageshort}} 从未加密迁移到加密的信息。有关提供者管理的加密存储器的更多信息，请参阅 [{{site.data.keyword.blockstorageshort}} 静态加密](block-file-storage-encryption-rest.html)文章。要查看已升级的数据中心和可用功能的列表，请单击[此处](new-ibm-block-and-file-storage-location-and-features.html)。

首选迁移路径是同时连接两个 LUN，并将数据从一个文件 LUN 直接传输到另一个 LUN。具体操作取决于您的操作系统以及在复制操作期间数据是否会更改。

为方便起见，概述的是较为常见的场景。假定您已将非加密文件 LUN 连接到主机。如果尚未连接，请按照下面最适合您所运行操作系统的指示信息来完成此任务。

- [在 Linux 上访问 {{site.data.keyword.blockstorageshort}}](accessing_block_storage_linux.html)

- [在 Windows 上访问 {{site.data.keyword.blockstorageshort}}](accessing-block-storage-windows.html)

 
## 创建加密 LUN

使用以下步骤创建大小相同或更大的加密 LUN，以帮助执行迁移过程。订购加密耐久性存储器 LUN

1. 单击 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 主页中的**存储** > **{{site.data.keyword.blockstorageshort}}**，或者单击 {{site.data.keyword.BluSoftlayer_full}}“目录”中的**基础架构** > **存储** > **{{site.data.keyword.blockstorageshort}}**。

2. 单击 {{site.data.keyword.blockstorageshort}} 页面上的**订购 {{site.data.keyword.blockstorageshort}}** 链接。

3. 选择**耐久性**。

4. 选择原始 LUN 所在的数据中心。<br/> **注**：加密仅在精选数据中心内可用。

5. 选择所需的 IOPS 层。

6. 选择所需的存储空间量（以 GB 为单位）。对于 TB，1 TB 等于 1,000 GB，12 TB 等于 12,000 GB。

7. 输入用于快照的所需存储空间量（以 GB 为单位）。

8. 从下拉列表中选择 VMware 操作系统。

9. 提交订单。

## 订购加密性能存储器 LUN

1. 单击 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 主页中的**存储** > **{{site.data.keyword.blockstorageshort}}**，或者单击 {{site.data.keyword.BluSoftlayer_full}}“目录”中的**基础架构** > **存储** > **{{site.data.keyword.blockstorageshort}}**。

2. 单击**订购 {{site.data.keyword.blockstorageshort}}**。

3. 选择**性能**。

4. 选择原始 LUN 所在的数据中心。请注意，加密仅在带有星号 (`*`) 的数据中心内可用。

5. 选择所需的存储空间量（以 GB 为单位）。对于 TB，1 TB 等于 1,000 GB，12 TB 等于 12,000 GB。

6. 输入所需的 IOPS，以 100 为区间。

7. 从下拉列表中选择 VMware 操作系统。

8. 提交订单。

存储器将在不到一分钟的时间内进行供应，并且将在 {{site.data.keyword.slportal}} 的 {{site.data.keyword.blockstorageshort}} 页面上显示。

 
## 将新卷连接到主机

“已授权”主机是已向其授予某个卷的访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。授权主机来访问卷会生成用户名、密码和 iSCSI 限定名 (IQN)，这些是安装多路径 I/O (MPIO) iSCSI 连接所需的信息。

1. 单击**存储**  > **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。

2. 滚动到页面的**已授权主机**部分。

3. 单击页面右侧的**授权主机**链接。选择可以访问该卷的主机。

 
## 快照和复制

是否已为原始 LUN 建立快照和复制？如果是，那么需要使用与原始卷相同的设置，为新的加密 LUN 设置复制和快照空间以及创建快照安排。 

请注意，如果复制目标数据中心尚未升级用于加密，那么在该数据中心升级之前，您将无法建立对加密卷的复制。

 
## 迁移数据

您应该同时连接到原始和加密 {{site.data.keyword.blockstorageshort}} LUN。 
- 如果未这样做，请确保正确执行了上述步骤以及在其他帖子中引用的步骤。打开支持凭单以帮助连接这两个 LUN。

### 数据注意事项

此时，您需要考虑原始 {{site.data.keyword.blockstorageshort}} LUN 上有什么类型的数据，以及如何以最佳方式将其复制到加密 LUN。如果您有备份、静态内容以及在复制期间不会更改的内容，那么没有任何重大注意事项。

如果是在 {{site.data.keyword.blockstorageshort}} 上运行数据库或虚拟机，请确保原始 LUN 上的数据在复制期间不会发生变更，以免发生损坏。如果您担心任何带宽问题，那么应该在非高峰时段执行迁移。如果需要有关这些注意事项的帮助，请立即开具支持凭单。
 
### Microsoft Windows

要将数据从原始 {{site.data.keyword.blockstorageshort}} LUN 复制到加密 LUN，请设置新存储器的格式并使用 Windows 资源管理器复制文件。

 
### Linux

您可以考虑使用 rsync 来复制数据。下面是示例命令：

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

建议将上面的命令与 --dry-run 标志一起使用，以确保路径正确排列。如果此过程中断，您可能需要删除正在复制的最后一个目标文件，以确保该文件从头开始复制到新位置。

完成不带 --dry-run 标志的此命令后，应该立即将数据复制到加密 {{site.data.keyword.blockstorageshort}} LUN。您应该向上滚动并再次运行该命令，以确保没有漏掉任何内容。您还可以手动复查这两个位置，以查找是否有任何可能漏掉的内容。

迁移完成后，您将能够将生产移至加密 LUN，然后从配置中拆离原始 LUN 并将其删除。请注意，删除操作还将除去目标站点上与原始 LUN 关联的任何快照或副本。
