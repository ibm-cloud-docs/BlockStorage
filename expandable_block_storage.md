---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-13"

---
{:new_window: target="_blank"}

# Expanding Block Storage Capacity

With this new feature, current {{site.data.keyword.blockstoragefull}} users can expand the size of their existing {{site.data.keyword.blockstorageshort}} in GB increments up to 12 TB immediately. They don't need to create a duplicate or manually migrate data to a larger volume. There's no outage or lack of access to the storage while the resize is taking place.

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

This feature is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).

## Advantages of Expandable Storage

- **Cost management** – You might know that there's potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand allows our customers to save on costs of storage and then grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid data growth need a way to quickly and easily increase the size of their storage to manage it.

## Effects of expanding storage capacity on Replication

The expand action on the primary storage results in automatic resizing of the replica.

## Limitations

This feature is available for storage that is provisioned in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).

Storage that was provisioned in these data centers before the release of this feature, during **April 2017 - 14 December 2017**, can be increased to 10 times its original size and no more. Storage that was provisioned after **14 December 2017** can be increased up to 12 TB.

Existing size limitations for {{site.data.keyword.blockstorageshort}} that was provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## Resizing storage

1. From the {{site.data.keyword.slportal}}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the LUN from the list and click **Actions** > **Modify LUN**
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** check box and click **Place Order**.
6. Your new storage allocation is available in a few minutes.

For more information about expanding the filesystem (and partitions, if any) on the volume to use the new space, check your OS documentation.
{:tip}
