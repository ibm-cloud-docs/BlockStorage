---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Upgrading existing {{site.data.keyword.blockstorageshort}} to enhanced {{site.data.keyword.blockstorageshort}}
{: #migratestorage}

Enhanced {{site.data.keyword.blockstoragefull}} is now available in select data centers. To see the list of upgraded data centers and available features such as adjustable IOPS rates and expandable volumes, click [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-news). For more information about provider-managed encrypted storage, see [{{site.data.keyword.blockstorageshort}} Encryption-At-Rest](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

The preferred migration path is to connect to both LUNs simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation.

The assumption is that you already have your non-encrypted LUN attached to your host. If not, follow the directions that fit your operating system the best to accomplish this task:

- [Connecting to LUNs on Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connecting to LUNs on CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connecting to LUNS on Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

All enhanced {{site.data.keyword.blockstorageshort}} volumes that are provisioned in these data centers have a different mount point than non-encrypted volumes. To ensure you're using the correct mount point for both storage volumes, you can view the mount point information in the **Volume Details** page in the console. You can also access the correct mount point through an API call: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

## Creating a {{site.data.keyword.blockstorageshort}}

When you place an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.
{:important}

You can order an enhanced LUN through the IBM Cloud Console and the {{site.data.keyword.slportal}}. Your new LUN must be of the same size or greater than the original volume to facilitate the migration.

- [Ordering {{site.data.keyword.blockstorageshort}} with pre-defined IOPS Tiers (Endurance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [Ordering {{site.data.keyword.blockstorageshort}} with custom IOPS (Performance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

Your new storage is available to mount in a few minutes. You can view it in the Resource List and in the {{site.data.keyword.blockstorageshort}} list.

## Connecting new {{site.data.keyword.blockstorageshort}} to host

"Authorized" hosts are hosts that were given access to a volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the user name, password, and iSCSI qualified name (IQN), which is needed to mount the multipath I/O (MPIO) iSCSI connection.

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and click your LUN Name.
2. Scroll to **Authorized Hosts**.
3. On the right, click **Authorize Host**. Select the hosts that can access the volume.


## Snapshots and Replication

Do you have snapshots and replication established for your original LUN? If yes, you need to set up replication, snapshot space and create snapshot schedules for the new LUN with the same settings as the original volume.

If your replication target data center is not upgraded yet, you can't establish replication for the new volume until that data center is upgraded.


## Migrating your data

1. Connect to both your original and new {{site.data.keyword.blockstorageshort}} LUNs.

   If you need assistance with connecting the two LUNs to your host, open a support case.
   {:tip}

2. Consider what type of data you have on your original {{site.data.keyword.blockstorageshort}} LUN and how best to copy it to your new LUN.
  - If you have backups, static content, and things that aren't expected to change during the copy, you don't need to worry much.
  - If you're running a database or a virtual machine on your {{site.data.keyword.blockstorageshort}}, make sure that the data isn't altered during the copy to avoid data corruption.
  - If you have any bandwidth concerns, do the migration during off peak times.
  - If you need assistance with these considerations, open a support case.

3. Copy your data across.
   - For **Microsoft Windows**, format the new storage, and copy the data from your original {{site.data.keyword.blockstorageshort}} LUN to your new LUN by using Windows Explorer.
   - For **Linux**, you can use `rsync` to copy over the data.
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   It's a good idea to use the previous command with the `--dry-run` flag once to make sure that the paths line up correctly. If this process is interrupted, you can delete the last destination file that was being copied to make sure that it is copied to the new location from the beginning.<br/>
   When this command completes without the `--dry-run` flag, your data is copied to the new {{site.data.keyword.blockstorageshort}} LUN. Run the command again to make sure that nothing was missed. You can also manually review both locations to look for anything that might be missing.<br/>
   When your migration is complete, you can move production to the new LUN. Then, you can detach and delete your original LUN from your configuration. The deletion also removes any snapshot or replica on the target site that was associated with the original LUN.
