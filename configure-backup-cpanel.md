---

copyright:
  years: 2018, 2019
lastupdated: "2019-12-06"

keywords: Block storage, cPanel, backups, mountpoint, ISCSI

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Configuring {{site.data.keyword.blockstorageshort}} for backup with cPanel
{: #cPanelBackups}

You can use the following instructions to configure your backups in cPanel to be stored in {{site.data.keyword.blockstoragefull}}. The assumption is that root or sudo SSH and full WebHost Manager (WHM) access are available. These instructions are based on a **CentOS 7** host.
{: shortdesc}

For more information, see the [cPanel documentation for backup](https://docs.cpanel.net/knowledge-base/backup/){: external}.
{: tip}

1. Connect to the host through SSH.

2. Ensure that a mount point target exists.
   By default, the cPanel system saves backup files locally, to the `/backup` directory. For the purpose of this document, it is assumed that `/backup` exists and contains backups, and `/backup2` is used as the new mount point.
   {: note}

3. Configure your {{site.data.keyword.blockstorageshort}} as described in [Connecting to iSCSI LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Make sure that you mount it to `/backup2` and configure it in `/etc/fstab` to enable mounting on start.

4. **Optional**: Copy existing backups to the new storage. You can use `rsync`.
   ```zsh
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

   This command compresses and transmits your data, while it preserves as much as possible, except for hard links. It provides information about what files are being transferred plus a brief summary at the end.
   {: tip}

5. Log in to WHM and go to the backup configuration by clicking **Home** > **Backup** > **Backup Configuration**.

6. Edit the configuration to save the backups in the new mount point.
    - Change the default backup directory by entering the absolute path to the new location in place of the /backup/ directory.
    - Select **Enable to mount a backup drive**. This setting causes the backup configuration process to check the `/etc/fstab` file for a backup mount (`/backup2`).

    If a mount exists with the same name as the staging directory, the backup configuration process mounts the drive and backs up the information to the drive. After the backup process finishes, it dismounts the drive.
    {: note}

7. Apply the changes by clicking **Save Configuration**.

8. **Optional**: As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.
