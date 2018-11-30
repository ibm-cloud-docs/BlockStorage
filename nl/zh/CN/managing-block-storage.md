---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 管理 {{site.data.keyword.blockstorageshort}}

您可以通过 [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} 来管理 {{site.data.keyword.blockstoragefull}} 卷。

## 查看 {{site.data.keyword.blockstorageshort}} LUN 详细信息

您可以查看所选存储器 LUN 的关键信息摘要，包括已添加到存储器的额外快照和复制功能。

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 单击列表中相应的 LUN 名称。

## 授权主机访问 {{site.data.keyword.blockstorageshort}}

“已授权”主机是已向其授予特定 LUN 访问权的主机。如果没有对主机授权，那么您将无法从系统访问或使用该存储器。授权主机来访问 LUN 会生成用户名、密码和 iSCSI 限定名 (IQN)，这些是安装多路径 I/O (MPIO) iSCSI 连接所需的信息。

您可以授权和连接与存储器位于同一数据中心的主机。您可以有多个帐户，但不能授权一个帐户中的主机来访问其他帐户上的存储器。
{:important}

1. 单击**存储** -> **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。
2. 滚动到页面的**已授权主机**部分。
3. 在右侧，单击**授权主机**。选择可以访问该特定 LUN 的主机。



## 查看有权访问 {{site.data.keyword.blockstorageshort}} LUN 的主机的列表

1. 单击**存储** -> **{{site.data.keyword.blockstorageshort}}**，然后单击 LUN 名称。
2. 向下滚动到**已授权主机**部分。

在此，您可以看到当前已授权访问 LUN 的主机的列表。还可以看到建立连接所需的认证信息 - 用户名、密码和 IQN 主机。“目标地址”在**存储器详细信息**页面上列出。对于 NFS，会将“目标地址”描述为 DNS 名称，对于 iSCSI，会描述为“发现目标门户网站”的 IP 地址。



## 查看授权主机访问的 {{site.data.keyword.blockstorageshort}}

可以查看主机有权访问的 LUN，包括建立连接所需的信息 - LUN 名称、存储类型、目标地址、容量和位置：

1. 在 [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} 中，单击**设备** -> **设备列表**，然后单击相应的设备。
2. 选择**存储**选项卡。

这将向您显示此特定主机有权访问的存储器 LUN 的列表。此列表按存储类型（块、文件或其他）进行分组。您可以通过单击**操作**来授予对更多存储器的访问权或除去访问权。



## 安装和卸装 {{site.data.keyword.blockstorageshort}}

根据主机的操作系统，遵循相应的指示信息来执行操作。

- [在 Linux 上连接到 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上连接到 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上连接到 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [使用 cPanel 配置 Block Storage 进行备份](configure-backup-cpanel.html)
- [使用 Plesk 配置 Block Storage 进行备份](configure-backup-plesk.html)


## 撤销主机对 {{site.data.keyword.blockstorageshort}} 的访问权

如果要停止从主机访问特定存储器 LUN，可以撤销相应的访问权。撤销访问权后，将从 LUN 删除主机连接。该主机上的操作系统和应用程序都无法再与该 LUN 通信。

为了避免主机端问题，请在撤销访问权之前，先从操作系统中卸装存储器 LUN，以避免发生驱动器缺失或数据损坏的情况。
{:important}

可以在**设备列表**或**存储**视图中撤销访问权。

### 通过设备列表撤销访问权

1. 在 [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} 中，单击**设备** > **设备列表**，然后双击相应的设备。
2. 选择**存储**选项卡。
3. 这将向您显示此特定主机有权访问的存储器 LUN 的列表。此列表按存储类型（块、文件或其他）进行分组。在 LUN 名称旁边，选择**操作**，然后单击**撤销访问权**。
4. 确认是否要撤销对 LUN 的访问权，因为该操作无法撤销。单击**是**以撤销 LUN 访问权，或单击**否**以取消该操作。

如果要断开一个特定主机与多个 LUN 的连接，需要对每个 LUN 重复“撤销访问权”操作。
{:tip}


### 通过存储视图撤销访问权

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**，然后选择要从中撤销访问权的 LUN。
2. 滚动到**已授权主机**。
3. 单击要撤销其访问权的主机旁边的**操作**，然后选择**撤销访问权**。
4. 确认是否要撤销对 LUN 的访问权，因为该操作无法撤销。单击**是**以撤销 LUN 访问权，或单击**否**以取消该操作。

如果要断开一个特定 LUN 与多个主机的连接，需要对每个主机重复“撤销访问权”操作。
{:tip}



## 取消存储器 LUN

如果不再需要特定 LUN，可以将其取消。要取消存储器 LUN，首先需要撤销所有主机对它的访问权。
{:important}

1. 单击**存储** > **{{site.data.keyword.blockstorageshort}}**。
2. 单击要取消的 LUN 的**操作**，然后选择**取消 {{site.data.keyword.blockstorageshort}}**。
3. 确认是要立即取消 LUN，还是在供应 LUN 的周年日期取消。

   如果选择在 LUN 的周年日期取消 LUN 的选项，那么可以在 LUN 的周年日期之前使取消请求失效。
   {:tip}
4. 单击**继续**或**关闭**。

5. 单击**确认**复选框，然后单击**确认**。
