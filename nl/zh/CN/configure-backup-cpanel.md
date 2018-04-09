---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# 使用 cPanel 配置 {{site.data.keyword.blockstorageshort}} 进行备份

在本文中，我们的目的是提供有关通过 cPanel 配置要在 {{site.data.keyword.blockstoragefull}} 中存储的备份的指示信息。假定以 root 用户或 sudo 用户身份通过 SSH 登录到系统，并且有完整的 WebHost Manager (WHM) 访问权。此示例基于 **CentOS 7** 主机。

**注**：您可以在[此处](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}找到 cPanel 文档中有关“配置备份目录”的内容。

1. 通过 SSH 连接到主机。

2. 确保存在安装点目标。<br />
   **注**：缺省情况下，cPanel 系统会将备份文件本地保存到 `/backup` 目录。在本文档中，假定 `/backup` 已存在并包含备份，因此我们将使用 `/backup2` 作为新的安装点。
   
3. 如[在 Linux 上连接到 MPIO iSCSI LUN](accessing_block_storage_linux.html) 中所述配置 {{site.data.keyword.blockstorageshort}}。确保将其安装到 `/backup2`，并在 `/etc/fstab` 中对其进行配置，以启用引导时安装。

4. **可选**：将现有备份复制到新存储器。例如，使用 `rsync`：
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **注**：此命令传输压缩的数据以尽可能保留最多的数据（硬链接除外），提供有关正在传输的文件的信息并在结束时提供简短摘要。
    
5.  登录到 WebHost Manager，然后通过**主页** > **备份** > **备份配置**浏览至备份配置。

6.  编辑配置以将备份保存在新的安装点中。 
    - 通过输入新位置的绝对路径来取代 /backup/ 目录，从而更改缺省备份目录。 
    - 选择**启用以安装备份驱动器**。此设置会使“备份配置”过程检查 `/etc/fstab` 文件中是否有备份安装 (`/backup2`)。<br /> **注**：如果存在与编译打包目录同名的安装，那么“备份配置”过程会安装驱动器并将信息备份到该安装。备份过程完成后，会卸装该驱动器。 

7. 通过单击**备份配置**界面底部的**保存配置**来应用更改。

8. **可选**：根据特定用例和业务需求的指示，从服务器中除去旧存储器并从帐户中取消该存储器。

