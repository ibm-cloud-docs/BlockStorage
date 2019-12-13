---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-14"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# Expanding Block Storage Capacity
{: #expandingcapacity}

With this feature, current {{site.data.keyword.blockstoragefull}} users can expand the size of their existing {{site.data.keyword.blockstorageshort}} in GB increments up to 12 TB immediately. They don't need to create a duplicate or manually migrate data to a larger volume. There's no outage or lack of access to the storage while the resize is taking place.
{:shortdesc}

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

This feature is available in [most data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

## Advantages of Expandable Storage

- **Cost management** – You might know that there's potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand allows our customers to save on cost of storage, and later grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid data growth need a way to quickly and easily increase the size of their storage to manage it.

## Effects of expanding storage capacity on Replication

The expand action on the primary storage results in automatic resizing of the replica.

## Limitations
{: #limitsofexpandingstorage}

This feature is available for storage that is provisioned in [most data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

Storage that was provisioned in these data centers before the release of this feature, during **April 2017 - 14 December 2017**, can be increased to 10 times its original size and no more. Storage that was provisioned after **14 December 2017** can be increased up to 12 TB.

Existing size limitations for {{site.data.keyword.blockstorageshort}} that was provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## Resizing storage
{: #resizingsteps}

1. From the {{site.data.keyword.cloud}} console, click the **menu** icon. Then, click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the iSCSI volume from the list and click the ellipsis (**...**) > **Modify LUN**.
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** check box and click **Place Order**.
6. Your new storage allocation is available in a few minutes.

Alternatively, you can resize your volume through the SLCLI.

```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help                    Show this message and exit.
```
{:codeblock}

For more information about expanding the file system (and partitions, if any) on the volume to use the new space, check your OS documentation.
{:tip}
