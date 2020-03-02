---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-27"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}
{:shortdesc: .shortdesc}

# Creating and managing independent duplicate volumes
{: #duplicatevolume}

You can create a duplicate of an existing {{site.data.keyword.blockstoragefull}}. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot. The duplicate volume is completely independent from the original volume.
{:shortdesc}

For information about dependent duplicates that can be refreshed from the original, primary volume, see [Creating and managing dependent duplicate volumes](/docs/BlockStorage?topic=BlockStorage-dependentduplicate).
{:note}

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. For more information about snapshots and how to order snapshot space, see the [Snapshot documentation](/docs/BlockStorage?topic=BlockStorage-snapshots).  

Duplicates can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the new volume is created in the same data center as the replica volume.

Duplicate volumes can be accessed by a host for read/write as soon as the storage is provisioned. However, snapshots and replication aren't allowed until the data copy from the original to the duplicate is complete.

When the data copy is complete, the duplicate can be managed and used as an independent volume.

This feature is available in most locations. For more information, see [the list of available data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

If you are a Dedicated account user of {{site.data.keyword.containerlong}}, see your options for duplicating a volume in the [{{site.data.keyword.containerlong_notm}} documentation](/docs/containers?topic=containers-block_storage#block_backup_restore).
{:tip}

Some common uses for a duplicate volume:
- **Disaster Recovery Testing**. Create a duplicate of your replica volume to verify that the data is intact and can be used if a disaster occurs, without interrupting the replication.
- **Golden Copy**. Use a storage volume as golden copy that you can create multiple instances from for various uses.
- **Data refreshes**. Create a copy of your production data to mount to your non-production environment for testing.
- **Restore from Snapshot**. Restore data on the original volume with specific files and date from a snapshot without overwriting the entire original volume with the snapshot restore function.
- **Development and Testing (dev/test)**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.
- **Storage Resize**. Create a volume with new size, IOPS rate or both without needing to move your data.  

You can create a duplicate volume through the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external} in a couple of ways.


## Creating a duplicate from a specific volume in the Storage List

1. Go to your list of {{site.data.keyword.blockstorageshort}} in the {{site.data.keyword.cloud_notm}} console by clicking **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select a volume from the list and click the ellipsis (**...**) > **Duplicate LUN**.
3. Choose your snapshot option:
    - If you order from a **non-replica** volume,
      - Select **Create a new snapshot to clone* – this action creates a snapshot to be used for the duplicate. Use this option if your volume doesn't have current snapshots or if you want to create a duplicate right then.<br/>
      - Select **Use latest snapshot to clone LUN** – this action creates a duplicate from the most recent snapshot that exists for this volume.
    - If you order from a **replica** volume the only option for snapshot is to use the most recent snapshot available.
4. Storage Type and Location entries remain the same as the original volume.
5. Hourly or Monthly Billing – you can choose to provision the duplicate LUN with hourly or monthly billing. The billing type for the original volume is automatically selected. If you want to choose a different billing type for your duplicate storage, you can make that selection here.
6. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.blockstorageshort}} can be resized to 10 times the original size of the volume.
   {:tip}
7. You can specify IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
       - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
       - If your original volume is 2, 4, or 10 IOPR Endurance tier, you can move anywhere between those tiers for the new volume.
8. You can update the snapshot space for the new volume to add more, less, or no snapshot space.
9. You can select the OS Type.
10. Check the box if you read and agreed to the Master Service Agreement.
11. Click **Place Order** to continue.

## Creating a duplicate from a specific Snapshot

1. Go to your list of {{site.data.keyword.blockstorageshort}}.
2. Click a LUN from the list to view the details page. (It can either be a replica or non-replica volume.)
3. Scroll down and select an existing snapshot from the list on the detail page and click **Actions** > **Duplicate**.   
4. The Location remains the same as the original volume.
5. You can choose to provision the duplicate LUN with hourly or monthly billing. The billing type for the original volume is automatically selected. If you want to choose a different billing type for your duplicate storage, you can change the selection.
6. You can update the size of the new volume so that it is larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.blockstorageshort}} can be resized to 10 times the original size of the volume.
   {:tip}
6. Available Performance and size combinations are displayed. The IOPS designation of the original volume is set by default. You can specify IOPS or IOPS Tier for the new volume.
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
    - If your original volume is 2, 4, or 10 IOPS Endurance tier, you can move anywhere between those tiers for the new volume.

7. You can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume is set by default.
8. Click **Continue** to place your order for the duplicate.


## Creating a duplicate through the SLCLI
You can use the following command in the SLCLI to create a duplicate {{site.data.keyword.blockstorageshort}} volume.

```
# slcli block volume-duplicate --help
Usage: slcli block volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate block volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Potential Sizes:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] Minimum: [the size of the
                                  origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin block volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```
{:codeblock}

## Managing your duplicate volume

While data is being copied from the original volume to the duplicate, you can see a status on the details page that shows the duplication is in progress. During this time, you can attach to a host, and read and write to the volume, but you can't create snapshot schedules. When the duplication process is complete, the new volume is independent from the original and can be managed with snapshots and replication as normal.
