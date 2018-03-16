---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configuring {{site.data.keyword.blockstorageshort}} for Backup with Plesk

In this article we aim to provide instructions for configuring {{site.data.keyword.blockstoragefull}} for your backups in Plesk. The assumption is that root or sudo SSH and full admin level Plesk access are available. This example is based on a CentOS7 host.

**Note**: You can find Plesk's documentation for backing up and restoration [here](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Connect to the host via SSH.

2. Ensure that a mountpoint target exists. <br />
   **Note**: Plesk has two options for storing backups, one is the internal Plesk storage - a backup storage located on your Plesk server, the other is an external FTP storage - a backup storage located on some external server in the Web or your local network. Commonly on Plesk boxes, internal backups are stored in `/var/lib/psa/dumps` and use `/tmp` as a temporary directory. In our example we keep the temporary directory local, but move the dump directory to the STaaS target (`/backup/psa/dumps`). No FTP user credentials are required.
   
3. Configure your {{site.data.keyword.blockstorageshort}} as described in [Connecting to MPIO iSCSI LUNs on Linux](accessing_block_storage_linux.html). Mount {{site.data.keyword.blockstorageshort}} to `/backup` and configure `/etc/fstab` to enable mounting on boot.

4. **Optional**: Copy existing backups to the new storage. Use `rsync` for example:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Note**: This command transmits your data compressed while preserving as much as possible (except hardlinks), and provides information about what files are being transferred plus a brief summary at the end.
    
5. Edit `/etc/psa/psa.conf` to point the `DUMP_D` value at the new target. 
    -  Should appear as: `DUMP_D /backup/psa/dumps`. 

6. **Optional**: As dictated by your particular use case and business needs, remove the old storage from the server and cancel from the account.


