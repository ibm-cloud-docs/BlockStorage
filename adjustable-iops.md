---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Adjusting IOPS

With this new feature, our {{site.data.keyword.blockstoragefull}} storage users can adjust the IOPS of their existing {{site.data.keyword.blockstorageshort}} immediately, without the need to create a duplicate or manually migrate data to new storage. Users won't experience any kind of outage or lack of access to the storage while the adjustment is taking place. 

Billing for the storage will be updated to add the pro-rated difference of the new price to the current billing cycle. The full new amount is billed in the next billing cycle.

This feature is only available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 

## Why Take Advantage of Adjustable IOPS?

- Cost management – Some of our clients may only need high IOPS during peak usage times. For example, a large retail store has peak usage during the holidays and may need higher IOPS rate on their storage then versus the middle of the summer. This feature allows them to manage their costs and pay for higher IOPS only when they really need it.

## Are There Any Limitations?

This feature is only available for storage that is provisioned in [data centers](new-ibm-block-and-file-storage-location-and-features.html) with enhanced capabilities. 

Clients can't switch between Endurance and Performance when they adjust their IOPS. Users can specify a new IOPS tier or IOPS level for their storage based on the following criteria/restrictions: 

- If original volume is Endurance 0.25 tier, IOPS tier cannot be updated.
- If original volume is Performance with < 0.30 IOPS/GB, options available should only include size and IOPS combinations that result in < 0.30 IOPS/GB. 
- If original volume is Performance with >= 0.30 IOPS/GB, options available should only include size and iops combinations that result in >= 0.30 IOPS/GB. Size (greater than or equal to original volume)



## How Does Adjusting IOPS Affect Replication?

If the volume has replication in place, the replica will be automatically updated to match the IOPS selection of the primary. 

## How Can I Adjust the IOPS on My Storage?

1. From the {{site.data.keyword.slportal}}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the LUN from the list and click **Actions** > **Modify LUN**
3. Under **Storage IOPS Options**, make a new selection:
    - Endurance (Tiered IOPS): select an IOPS Tier greater than 0.25 IOPS/GB of your storage. You may increase the IOPS tier at any time. However, decreasing is only available once a month.
    - Performance (Allocated IOPS): specify new IOPS option for your storage by entering a value between 100-48,000 IOPS. (Be sure to look at any specific boundaries required by size in the order form.)
4. Review your selection and the new pricing.
5. Click the **I have read the Master Service Agreement...** check box and click **Place Order**.
6. Your new storage allocation should be available in a few minutes.
