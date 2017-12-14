---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Introducing Expandable Storage

With this new feature, our current {{site.data.keyword.blockstoragefull}} users are able to expand the size of their existing {{site.data.keyword.blockstorageshort}} in GB increments up to 12 TB on the fly, without the need to create a duplicate or manually  migrate data to a larger volume.  There is no outage or lack of access to the storage while the resize is taking place. 

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle and then billed the full new amount in the next billing cycle.

This feature is only available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 

## Why Take Advantage of Expandable Storage?

- **Cost management** – You may know that there is potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand, allows our customers to save on costs of storage and then grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid growth need a way to quickly and easily increase the size of their storage to manage that growth.

## How Does Expanding Storage Capacity Affect Replication?

Expand action on the primary storage will result automatic resize of the replica. 

## Are There Any Limitations?

This feature is only avaialble for storage that is provisioned in [datacenters](new-ibm-block-and-file-storage-location-and-features.html) with enhanced capabilities. 

Storage that is provisioned on updated storage in these data centers prior to release (insert date here) of this feature can only be increased to 10x it's original size.  All other storage provisioned, after that date can be increased up to the max 12TB size. 

Existing size limitations for {{site.data.keyword.blockstorageshort}} provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## How Can I Tell if My Provisioned Storage is Expandable?

Storage provisioned with enhanced capabilites is always encryped-at-rest.  You can easily tell that your storage is eligible if it has a "lock" icon next to it in the portal UI. 

## How Can I Resize My Storage?

1. From the {{site.data.keyword.slportal}}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from {{site.data.keyword.BluSoftlayer_full}} Catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the LUN from the list and click **Actions** > **Modify LUN**
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** checkbox and click **Place Order**.
6. Your new storage allocation should be available in a few minutes.
  
