---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Upgrading existing {{site.data.keyword.blockstorageshort}} to encrypted {{site.data.keyword.blockstorageshort}} with updated features

Encrypted {{site.data.keyword.blockstoragefull}} is now available in select data centers. For more information on provider-managed encrypted storage, read [{{site.data.keyword.blockstorageshort}} Encryption-At-Rest article](block-file-storage-encryption-rest.html). To see the list of upgraded data centers and available features such as adjustable IOPS rates or expandable volumes click [here](new-ibm-block-and-file-storage-location-and-features.html).

The preferred migration path is to connect to both LUNs simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation. The more common scenarios have been outlined for your convenience. There's an assumption that you already have your non-encrypted file LUN attached to your host. If not, follow the directions below that best fits the operating system you're running to accomplish this task.

- [Accessing {{site.data.keyword.blockstorageshort}} on Linux](accessing_block_storage_linux.html)

- [Accessing {{site.data.keyword.blockstorageshort}} on Windows](accessing-block-storage-windows.html)

 
## Create a new, encrypted LUN with updated features

Use the following steps to create a LUN of the same size or larger than the original, that is encrypted to facilitate the migration process. 

### Order an Endurance storage LUN

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} home page OR Click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}** in the {{site.data.keyword.BluSoftlayer_full}} catalog.

2. Click **Order {{site.data.keyword.blockstorageshort}}**.

3. Select **Endurance**.

4. Select the data center where your original LUN is located. <br/> **Note**: Encryption is only available in select data centers.

5. Select the IOPS tier.

6. Select the amount of storage space in GBs. For TB, 1 TB equals 1,000 GB, and 12 TB equals 12,000 GB.

7. Enter the amount of storage space in GBs for snapshots.

8. Select the VMware OS from the drop-down list.

9. Submit the order.

## Order a Performance storage LUN

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}** from the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} home page OR Click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}** in the {{site.data.keyword.BluSoftlayer_full}} catalog.

2. Click **Order {{site.data.keyword.blockstorageshort}}**.

3. Select **Performance**.

4. Select the data center where your original LUN is located. <br/> **Note**: Encryption is only available in data centers marked with an asterisk (`*`).

5. Select the amount of storage space in GBs. For TB, 1 TB equals 1,000 GB, and 12 TB equals 12,000 GB.

6. Enter the amount of IOPS in intervals of 100.

7. Select the VMware OS from the drop-down list.

8. Submit the order.

Storage will be provisioned in less than a minute and will be visible on the {{site.data.keyword.blockstorageshort}} page of the {{site.data.keyword.slportal}}.

**IMPORTANT**: When placing an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.
 
## Connect new volume to host

"Authorized" hosts are hosts that have been given access rights to a volume. Without host authorization, you won't be able to access or use the storage from your system. Authorizing a host to access your volume generates the user name, password and iSCSI qualified name (IQN), which is needed to mount the multipath I/O (MPIO) iSCSI connection.

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}** and click your LUN Name.

2. Scroll to **Authorized Hosts**.

3. On the right, click **Authorize Host**. Select the hosts that can access the volume.

 
## Snapshots and Replication

Do you have snapshots and replication established for your original LUN? If yes, you'll need to set up replication, snapshot space and create snapshot schedules for the new LUN with the same settings as the original volume. 

Note that if your replication target data center hasn't been upgraded for encryption, you won't be able to establish replication for the new volume until that data center is upgraded.

 
## Migrate your data

You should be connected to both your original and new {{site.data.keyword.blockstorageshort}} LUNs. 
- If you need assistance with connecting the two LUNs to your host, open a support ticket.

### Data Considerations

At this point, you should consider what type of data you have on your original {{site.data.keyword.blockstorageshort}} LUN and how best to copy it to your new LUN. If you have backups, static content and things that aren't expected to change during the copy, there aren't any major considerations.

If you're running a database or a virtual machine on your {{site.data.keyword.blockstorageshort}}, make sure that the data isn't altered during the copy to avoid data corruption. If you have any bandwidth concerns, you should perform the migration during off peak times. If you need assistance with these considerations, open a support ticket.
 
### Microsoft Windows

To copy data from your original {{site.data.keyword.blockstorageshort}} LUN to your new LUN, format the new storage and copy the files over using Windows Explorer.

 
### Linux

You may consider using 'rsync' to copy over the data. Below is an example command:

```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

It's recommended that you use the above command with the `--dry-run` flag once to make sure the paths line up correctly. If this process is interrupted, you may want to delete the last destination file that was being copied to make sure that it`s copied to the new location from the beginning.

When this command completes without the `--dry-run` flag, your data should be copied to the new {{site.data.keyword.blockstorageshort}} LUN. Scroll up and run the command again to make sure nothing was missed. You may also manually review both locations to look for anything that might be missing.

When your migration is complete, you`ll be able to move production to the new LUN. Then, you can detach and delete your original LUN from your configuration. Note that the deletion will also remove any snapshot or replica on the target site that was associated with the original LUN.
