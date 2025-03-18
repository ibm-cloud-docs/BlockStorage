---

copyright:
  years: 2018, 2025
lastupdated: "2025-03-18"

keywords: Block Storage for Classic, cPanel, backups, mountpoint, iSCSI

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Configuring {{site.data.keyword.blockstorageshort}} for backup with cPanel
{: #cPanelBackups}

You can use the following instructions to configure your backups in cPanel to be stored in {{site.data.keyword.blockstoragefull}}. The assumption is that root or sudo SSH and full WebHost Manager (WHM) access are available.
{: shortdesc}

While you can store a backup directly to a remote filesystem, cPanel and WHM do **not** support this configuration. For more information, see the [cPanel documentation for backup](https://docs.cpanel.net/knowledge-base/backup/how-to-run-backups-on-locally-mounted-remoted-file-systems/){: external}.
{: attention}

1. Connect to the host through SSH.

2. Make sure that a mount point target exists.
   By default, the cPanel system saves backup files locally, to the `/backup` directory. For this document, it is assumed that the folder `/backup` exists and contains backups, and `/backup2` is used as the new mount point.
   {: note}

3. Configure your {{site.data.keyword.blockstorageshort}} as described in one of the following tutorials. Make sure that you mount it to `/backup2` and configure it in `/etc/fstab` to enable mounting on start.
    - [Mount iSCSI volume on Red Hat Enterprise Linux&reg; 8](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8).
    - [Mount iSCSI volume on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8).
    - [Mount iSCSI volume on Ubuntu 20](/docs/BlockStorage?topic=BlockStorage-mountingUbu20).

4. **Optional**: Copy existing backups to the new storage. You can use `rsync`.
   ```sh
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

   This command compresses and transmits your data, while it preserves as much as possible, except for hard links. It provides information about what files are being transferred plus a brief summary at the end.
   {: tip}

5. Log in to WHM and go to the backup configuration by clicking **Home** > **Backup** > **Backup Configuration**.

6. Edit the configuration to save the backups in the new mount point.
    - Change the default backup directory by entering the absolute path to the new location in place of the `/backup` directory.
    - Select **Enable to mount a backup drive**. This setting causes the backup configuration process to check the `/etc/fstab` file for a backup mount (`/backup2`).

    If a mount exists with the same name as the staging directory, the backup configuration process mounts the drive and backs up the information to the drive. After the backup process finishes, it dismounts the drive.
    {: note}

7. Apply the changes by clicking **Save Configuration**.

8. **Optional**: As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.
