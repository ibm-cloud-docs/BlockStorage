---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}

# Upgrading existing {{site.data.keyword.blockstorageshort}} to enhanced {{site.data.keyword.blockstorageshort}}

Enhanced {{site.data.keyword.blockstoragefull}} is now available in select data centers. To see the list of upgraded data centers and available features such as adjustable IOPS rates and expandable volumes, click [here](new-ibm-block-and-file-storage-location-and-features.html). For more information on provider-managed encrypted storage, read the [{{site.data.keyword.blockstorageshort}} Encryption-At-Rest](block-file-storage-encryption-rest.html) article.

The preferred migration path is to connect to both LUNs simultaneously and transfer data directly from one LUN to another. The specifics depend on your operating system and whether the data is expected to change during the copy operation. 

There's an assumption that you already have your non-encrypted LUN attached to your host. If not, follow the directions that fit your operating system the best to accomplish this task:

- [Accessing {{site.data.keyword.blockstorageshort}} on Linux](accessing_block_storage_linux.html)
- [Accessing {{site.data.keyword.blockstorageshort}} on Windows](accessing-block-storage-windows.html)

 
## Creating new {{site.data.keyword.blockstorageshort}}

**IMPORTANT**: When you place an order with API, specify the "Storage as a Service" package to ensure you're getting the updated features with your new storage.

The following instructions are for ordering an enhanced LUN through the {{site.data.keyword.slportal}}. Your new LUN must be of the same size or greater than the original volume to facilitate the migration.

### Order an Endurance LUN

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In the upper right corner, click **Order {{site.data.keyword.blockstorageshort}}**.
3. Select **Endurance** from the **Select Storage Type** list.
4. Select your deployment **Location** (data center).
   - Ensure that the new Storage is added in the same location as the previous volume.
5. Select your billing option. You can choose between hourly and monthly billing.
6. Select the IOPS tier.
7. Click **Select Storage Size** and select your storage size from the list.
8. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](ordering-snapshots.html).
9. Choose your **OS Type** from the list.
10. Click **Continue**. You’re shown the monthly and prorated charges with a final chance to review order details.
11. Click the **I have read the Master Service Agreement** check box and click **Place Order**.

### Order a Performance LUN

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In the upper right corner, click **Order {{site.data.keyword.blockstorageshort}}**.
3. Select **Performance** from the **Select Storage Type** list.
4. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the host or hosts you ordered previously.
5. Select your billing option. You can choose between hourly and monthly billing.
6. Select the appropriate **Storage Size**.
7. Enter the IOPS in the **Specify IOPS** field.
8. Click **Continue**. You are shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
9. Click the **I have read the Master Service Agreement** check box, and click **Place Order**.

Storage is provisioned in less than a minute and is visible on the {{site.data.keyword.blockstorageshort}} page of the {{site.data.keyword.slportal}}.


 
## Connecting new {{site.data.keyword.blockstorageshort}} to host

"Authorized" hosts are hosts that were given access rights to a volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the user name, password, and iSCSI qualified name (IQN), which is needed to mount the multipath I/O (MPIO) iSCSI connection.

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and click your LUN Name.

2. Scroll to **Authorized Hosts**.

3. On the right, click **Authorize Host**. Select the hosts that can access the volume.

 
## Snapshots and Replication

Do you have snapshots and replication established for your original LUN? If yes, you need to set up replication, snapshot space and create snapshot schedules for the new LUN with the same settings as the original volume. 

Note that if your replication target data center is not upgraded yet, you won't be able to establish replication for the new volume until that data center is upgraded.

 
## Migrating your data

First, connect to both your original and new {{site.data.keyword.blockstorageshort}} LUNs. 
- If you need assistance with connecting the two LUNs to your host, open a support ticket.

Then, consider what type of data you have on your original {{site.data.keyword.blockstorageshort}} LUN and how best to copy it to your new LUN. If you have backups, static content, and things that aren't expected to change during the copy, there aren't any major concerns.

If you're running a database or a virtual machine on your {{site.data.keyword.blockstorageshort}}, make sure that the data isn't altered during the copy to avoid data corruption. If you have any bandwidth concerns, do the migration during off peak times. If you need assistance with these considerations, open a support ticket.
 
### Microsoft Windows

To copy data from your original {{site.data.keyword.blockstorageshort}} LUN to your new LUN, format the new storage, and copy the files over by using Windows Explorer.

 
### Linux

You might consider using 'rsync' to copy over the data. Below is an example command:

```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

It is recommended that you use the above command with the `--dry-run` flag once to make sure that the paths line up correctly. If this process is interrupted, you can delete the last destination file that was being copied to make sure that it is copied to the new location from the beginning.

When this command completes without the `--dry-run` flag, your data is copied to the new {{site.data.keyword.blockstorageshort}} LUN. Run the command again to make sure that nothing was missed. You can also manually review both locations to look for anything that might be missing.

When your migration is complete, you'll be able to move production to the new LUN. Then, you can detach and delete your original LUN from your configuration. Note that the deletion also removes any snapshot or replica on the target site that was associated with the original LUN.
