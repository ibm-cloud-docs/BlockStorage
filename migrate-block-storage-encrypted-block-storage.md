---

copyright:
  years: 2014, 2025
lastupdated: "2025-01-14"

keywords: Block Storage for Classic, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Upgrading existing {{site.data.keyword.blockstorageshort}} to enhanced {{site.data.keyword.blockstorageshort}}
{: #migratestorage}

Enhanced {{site.data.keyword.blockstoragefull}} is now available in all [data centers](/docs/overview?topic=overview-locations#data-centers). The preferred migration path is to provision an enhanced {{site.data.keyword.blockstorageshort}}, connect both LUNs simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation.
{: shortdesc}

You don't need to follow this process if your Storage received an upgrade to the Storage-as-a-Service package as part of the ongoing hardware refresh process.
{: tip}

## Provisioning a {{site.data.keyword.blockstorageshort}}
{: #createencryptedLUN2}

You can order an enhanced LUN in the {{site.data.keyword.cloud}} console, through the CLI or the API. Your new LUN must be of the same size or greater than the original volume to facilitate the migration.

- [Ordering {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui#orderingthroughConsole)

When you place an order with API, specify the "Storag-as-a-Service" package to make sure that you're getting the updated features with your new storage.
{: important}

Your new storage is available to mount in a few minutes. You can view it in the Resource List and in the {{site.data.keyword.blockstorageshort}} list.

## Authorizing the host to access the new {{site.data.keyword.blockstorageshort}}
{: #authhosts2LUN}

Before you begin, make sure that the host that is to access the {{site.data.keyword.blockstorageshort}} volume is authorized. 

"Authorized" hosts are hosts that were given access to a volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the username, password, and iSCSI qualified name (IQN), which is needed to mount the multipath I/O (MPIO) iSCSI connection.

For more information, see [Authorizing the host in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}.

## Migrating your data
{: #copydataacross}

1. Connect to both your original and new {{site.data.keyword.blockstorageshort}} LUNs.
   - [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
   - [Mount iSCSI LUN on Red Hat Enterprise Linux&reg; 8](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8).
   - [Mount iSCSI LUN on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8).
   - [Mount iSCSI LUN on Ubuntu 20](/docs/BlockStorage?topic=BlockStorage-mountingUbu20).

   If you need assistance with connecting the two LUNs to your host, open a support case.
   {: tip}

2. Consider what type of data that you have on your original {{site.data.keyword.blockstorageshort}} LUN and how best to copy it to your new LUN.
   - If you have backups, static content, and things that aren't expected to change during the copy, you don't need to worry much.
   - If you're running a database or a virtual machine on your {{site.data.keyword.blockstorageshort}}, make sure that the data isn't altered during the copy to avoid data corruption.
   - If you have any bandwidth concerns, do the migration during off-peak times.
   - If you need assistance with these considerations, open a support case.

3. Copy your data across.
   - For **Microsoft Windows**, format the new storage, and copy the data from your original {{site.data.keyword.blockstorageshort}} LUN to your new LUN by using Windows Explorer.
   - For **Linux&reg;**, you can use `rsync` to copy over the data.
      ```sh
      [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
      ```

      It is advisable to use the previous command with the `--dry-run` flag once to make sure that the paths line up correctly. If this process is interrupted, you can delete the last destination file that was being copied to make sure that it is copied to the new location from the beginning.
      {: tip}

      When this command completes without the `--dry-run` flag, your data is copied to the new {{site.data.keyword.blockstorageshort}} LUN. Run the command again to make sure that nothing was missed. You can also manually review both locations to look for anything that might be missing.

      For more information about `rsync`, see the [`rsync` man page](https://download.samba.org/pub/rsync/rsync.html){: external}.
      {: note}

When your migration is complete, you can move production to the new LUN. Then, you can detach and delete your original LUN from your configuration. The deletion also removes any snapshot or replica on the target site that was associated with the original LUN.

## Snapshots and Replication
{: #setupreplicaonnewLUN}

Do you have snapshots and replication established for your original LUN? If yes, you need to set up replication, snapshot space and create snapshot schedules for the new LUN with the same settings as the original volume.
