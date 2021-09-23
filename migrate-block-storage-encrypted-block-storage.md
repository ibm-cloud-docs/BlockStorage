---

copyright:
  years: 2014, 2021
lastupdated: "2021-08-18"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Upgrading existing {{site.data.keyword.blockstorageshort}} to enhanced {{site.data.keyword.blockstorageshort}}
{: #migratestorage}

Enhanced {{site.data.keyword.blockstoragefull}} is now available in most [data centers](/docs/BlockStorage?topic=BlockStorage-selectDC). The preferred migration path is to provision an enhanced {{site.data.keyword.blockstorageshort}}, connect both LUNs simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation.
{: shortdesc}

## Provisioning a {{site.data.keyword.blockstorageshort}}

You can order an enhanced LUN in the {{site.data.keyword.cloud}} Console UI, through the CLI or the API. Your new LUN must be of the same size or greater than the original volume to facilitate the migration.

- [Ordering {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui#orderingthroughConsole)

When you place an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.
{: important}

Your new storage is available to mount in a few minutes. You can view it in the Resource List and in the {{site.data.keyword.blockstorageshort}} list.

## Authorizing the host to access the new {{site.data.keyword.blockstorageshort}}

"Authorized" hosts are hosts that were given access to a volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the user name, password, and iSCSI qualified name (IQN), which is needed to mount the multipath I/O (MPIO) iSCSI connection.

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}. From the **menu**, select **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume and click the ellipsis (**...**).
4. Select **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device types or subnets.
   - If you choose Devices, you can select from Bare Metal Servers or Virtual servers.
   - If you choose IP address, first, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that can access the volume and click **Save**.

When the host is authorized to access the new storage, you can mount or map the storage volume.

## Migrating your data

1. Connect to both your original and new {{site.data.keyword.blockstorageshort}} LUNs.
   - [Connecting to LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Connecting to LUNs on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Connecting to LUNS on Microsoft&reg; Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows)

   If you need assistance with connecting the two LUNs to your host, open a support case.
   {: tip}

2. Consider what type of data you have on your original {{site.data.keyword.blockstorageshort}} LUN and how best to copy it to your new LUN.
  - If you have backups, static content, and things that aren't expected to change during the copy, you don't need to worry much.
  - If you're running a database or a virtual machine on your {{site.data.keyword.blockstorageshort}}, make sure that the data isn't altered during the copy to avoid data corruption.
  - If you have any bandwidth concerns, do the migration during off-peak times.
  - If you need assistance with these considerations, open a support case.

3. Copy your data across.
   - For **Microsoft&reg; Windows&reg;**, format the new storage, and copy the data from your original {{site.data.keyword.blockstorageshort}} LUN to your new LUN by using Windows Explorer.
   - For **Linux&reg;**, you can use `rsync` to copy over the data.
      ```
      [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
      ```

      It is advisable to use the previous command with the `--dry-run` flag once to make sure that the paths line up correctly. If this process is interrupted, you can delete the last destination file that was being copied to make sure that it is copied to the new location from the beginning.
      {: tip}

      When this command completes without the `--dry-run` flag, your data is copied to the new {{site.data.keyword.blockstorageshort}} LUN. Run the command again to make sure that nothing was missed. You can also manually review both locations to look for anything that might be missing.

      For more information about `rsync`, see the [`rsync` man page](https://download.samba.org/pub/rsync/rsync.html){: external}.
      {: note}

When your migration is complete, you can move production to the new LUN. Then, you can detach and delete your original LUN from your configuration. The deletion also removes any snapshot or replica on the target site that was associated with the original LUN.

## Snapshots and Replication

Do you have snapshots and replication established for your original LUN? If yes, you need to set up replication, snapshot space and create snapshot schedules for the new LUN with the same settings as the original volume.

If your replication target data center is not upgraded yet, you can't establish replication for the new volume until that data center is upgraded.
{: important}
