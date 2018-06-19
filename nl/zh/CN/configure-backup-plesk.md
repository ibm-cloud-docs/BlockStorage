---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# 使用 Plesk 配置 {{site.data.keyword.blockstorageshort}} 进行备份

本文将帮助您在 Plesk 中配置 {{site.data.keyword.blockstoragefull}} 进行备份。假定以 root 用户或 sudo 用户身份通过 SSH 登录到系统，并且有完整的管理级别 Plesk 访问权。指示信息基于 CentOS7 主机。

**注**：您可以在[此处](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}找到 Plesk 文档中有关备份和复原的内容。

1. 通过 SSH 连接到主机。

2. 确保存在安装点目标。<br />
   **注**：Plesk 有两个备份存储选项。一个选项是内部 Plesk 存储器（Plesk 服务器上的备份存储器），另一个选项是外部 FTP 存储器（Web 或本地网络中某个外部服务器上的备份存储器）。通常在 Plesk 框中，内部备份存储在 `/var/lib/psa/dumps` 中，并使用 `/tmp` 作为临时目录。在示例中，我们使临时目录保持为本地目录，但将 dumps 目录移至 STaaS 目标 (`/backup/psa/dumps`)。不需要 FTP 用户凭证。
   
3. 如[在 Linux 上连接到 MPIO iSCSI LUN](accessing_block_storage_linux.html) 中所述配置 {{site.data.keyword.blockstorageshort}}。将 {{site.data.keyword.blockstorageshort}} 安装到 `/backup`，并配置 `/etc/fstab` 以启用引导时安装。

4. **可选**：将现有备份复制到新存储器。可以使用 `rsync`：
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **注**：此命令传输压缩的数据以尽可能保留最多的数据（硬链接除外）。它提供有关正在传输什么文件的信息并在结尾提供简短摘要。
    
5. 编辑 `/etc/psa/psa.conf` 以指向新目标上的 `DUMP_D` 值。 
    - 此值应该显示为：`DUMP_D /backup/psa/dumps`。 

6. **可选**：根据特定用例和业务需求的指示，从服务器中除去旧存储器并从帐户中取消该存储器。


