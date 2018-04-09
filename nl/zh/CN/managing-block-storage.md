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

# 管理 {{site.data.keyword.blockstorageshort}}

您可以通过 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 来管理 {{site.data.keyword.blockstoragefull}} 卷。本文提供了最常见任务的指示信息。

## 查看供应的 {{site.data.keyword.blockstorageshort}} LUN 详细信息

您可以查看所选存储器 LUN 的关键信息摘要，包括已添加到存储器的其他快照和复制功能。

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 单击列表中相应的 LUN 名称。

## 授权主机访问 {{site.data.keyword.blockstorageshort}}

“已授权”主机是已向其授予特定 LUN 访问权的主机。如果没有对主机授权，您将无法从系统访问或使用存储器。授权主机来访问 LUN 会生成用户名、密码和 iSCSI 限定名 (IQN)，这些是安装多路径 I/O (MPIO) iSCSI 连接所需的信息。

**注**：只能对位于存储器所在数据中心的主机进行授权和连接。如果您有多个帐户，那么不能授权一个帐户中的主机来访问位于其他帐户上的存储器。

1. 单击**存储** -> **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。
2. 滚动到页面的“已授权主机”部分。
3. 单击页面右侧的**授权主机**链接。选择可以访问该特定 LUN 的主机。

 

## 查看已授权访问 {{site.data.keyword.blockstorageshort}} LUN 的主机的列表

使用以下步骤来查看已授权访问 LUN 的主机的列表。

1. 单击**存储** -> **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。
2. 向下滚动到页面底部的“已授权主机”部分。

在此，您将看到当前已授权访问 LUN 的主机的列表，以及（特别是对于 iSCSI）建立连接所需的认证信息 - 用户名、密码和 IQN 主机。“目标地址”位于“存储器详细信息”页面的顶部。对于 NFS，会将其描述为 DNS 名称，对于 iSCSI，会描述为“发现目标门户网站”的 IP 地址。

 

## 查看授权主机访问的 {{site.data.keyword.blockstorageshort}}

可以查看主机有权访问的 LUN，包括建立连接所需的信息 - LUN 名称、存储类型、目标地址、容量和位置：

1. 在 [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} 中，单击**设备** -> **设备列表**，然后单击相应的设备。
2. 选择**存储**选项卡。

然后，将向您显示此特定主机有权访问的存储器 LUN 的列表，全部按存储类型（块、文件或其他）进行分组。通过相应的“操作”菜单，可以授权其他存储器访问权或除去访问权。

 

## 安装和卸装 {{site.data.keyword.blockstorageshort}}

请参阅以下文章中有关如何在主机上安装和卸装 {{site.data.keyword.blockstorageshort}} 的详细信息。

- [Linux 上的 {{site.data.keyword.blockstorageshort}}](accessing_block_storage_linux.html)

- [Microsoft Windows 上的 {{site.data.keyword.blockstorageshort}}](accessing-block-storage-windows.html)

 

## 撤销主机对 {{site.data.keyword.blockstorageshort}} 的访问权

如果要停止从主机访问特定存储 LUN，可以撤销相应的访问权。撤销访问权后，将从 LUN 删除主机连接，并且操作系统和应用程序都无法与该 LUN 通信。

**注**：为避免主机端问题，请在撤销访问权之前，先从操作系统中卸装存储器 LUN，以避免发生驱动器缺失或数据损坏问题。

可以在“设备列表”或“存储器”视图中撤销对任何存储器的访问权。

### 在“设备列表”中撤销访问权：

1. 在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，单击**设备** > **设备列表**，然后双击相应的设备。
2. 选择**存储**选项卡。
3. 然后，将向您显示此特定主机有权访问的存储器 LUN 的列表，全部按存储类型（块、文件或其他）进行分组。选择要从中撤销访问权的 LUN 旁边的相应“操作”菜单，然后单击**撤销访问权**。
4. 系统将询问您是否要撤销对 LUN 的访问权，因为该操作无法撤销。单击**是**以撤销 LUN 访问权，或单击**否**以取消该操作。

**注**：如果要断开一个特定主机与多个 LUN 的连接，需要对每个 LUN 重复“撤销访问权”操作。


### 在“存储器”视图中撤销访问权：

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**，然后选择要从中撤销访问权的 LUN。
2. 向下滚动到页面的**已授权主机**部分。
3. 单击要撤销其访问权的主机旁边的**操作**下拉箭头，然后选择**撤销访问权**。
4. 系统将询问您是否要撤销对 LUN 的访问权，因为该操作无法撤销。单击**是**以撤销 LUN 访问权，或单击**否**以取消该操作。

**注**：如果要断开多个主机与一个特定 LUN 的连接，需要对每个主机重复“撤销访问权”操作。

 

## 取消存储器 LUN

如果不再需要特定 LUN，可以将其取消。为了取消存储器 LUN，首先需要撤销所有主机对它的访问权。

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 单击要取消的 LUN 的**操作**下拉箭头，然后选择**取消 {{site.data.keyword.blockstorageshort}}**。
3. 系统将要求您确认是要立即取消 LUN，还是在供应 LUN 的周年日期取消。单击**继续**或**关闭**。
**注**：如果选择在LUN 的周年日期将其取消，可以在其周年日期前使取消请求失效。
4. 单击**确认**复选框，然后单击**确认**。

 

