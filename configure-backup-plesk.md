---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: Block storage, Plesk, backups, mountpoint, ISCSI

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Configuring {{site.data.keyword.blockstorageshort}} for backup with Plesk
{: #PleskBackups}

You can use the following instructions to configure {{site.data.keyword.blockstoragefull}} for your backups in Plesk. The assumption is that root or sudo SSH and full admin level Plesk access are available. These instructions are based on a CentOS7 host.
{: shortdesc}

For more information, see [Plesk's documentation for backing up and restoration](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){: external}.
{: tip}

1. Connect to the host through SSH.
2. Ensure that a mount point target exists.

   Plesk has two options for storing backups. One option is the internal Plesk storage (backup storage on your Plesk server). The other option is an external FTP storage (backup storage on some external server in the web or your local network). Commonly on Plesk boxes, internal backups are stored in `/var/lib/psa/dumps` and use `/tmp` as a temporary directory. In this example, the temporary directory is kept local, but the dumps directory is moved to the {{site.data.keyword.blockstorageshort}} target (`/backup/psa/dumps`). No FTP user credentials are required.
   {: note}

3. Configure your {{site.data.keyword.blockstorageshort}} as described in [Connecting to iSCSI LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Mount {{site.data.keyword.blockstorageshort}} to `/backup` and configure `/etc/fstab` to enable mounting on start.
4. **Optional**: Copy existing backups to the new storage. You can use `rsync`.
   ```zsh
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    This command compresses and transmits your data, and preserves as much as possible, except for hard links. It provides information about what files are being transferred plus a brief summary at the end.
    {: tip}

5. Edit `/etc/psa/psa.conf` to point the `DUMP_D` value at the new target.
    - It appears as: `DUMP_D /backup/psa/dumps`.
6. **Optional**: As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.
