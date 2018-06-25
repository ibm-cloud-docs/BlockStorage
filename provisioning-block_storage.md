---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordering {{site.data.keyword.blockstorageshort}}

There are two different ways you can provision {{site.data.keyword.blockstorageshort}} based on your needs and preferences. The two options are: 

- **Endurance**: provision Endurance tiers that feature pre-defined performance levels and features like snapshots and replication. 
- **Performance**: build a high powered Performance environment where you can allocate the specific input/output operations per second (IOPS) rate you want.

## Ordering Endurance for {{site.data.keyword.blockstorageshort}}

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In the upper right corner, click **Order {{site.data.keyword.blockstorageshort}}**.
3. Select **Endurance** from the **Select Storage Type** list.
4. Select your deployment **Location** (data center).
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
   - If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle, which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a block LUN is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
     **NOTE**: Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
5. Select the IOPS tier that your application needs.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) for storage that is provisioned up to 4 TB.
6. Click **Select Storage Size** and select your storage size from the list.
7. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](ordering-snapshots.html).
8. Choose your **OS Type** from the list.
9. Click **Continue**. You’re shown the monthly and prorated charges with a final chance to review order details.
10. Click the **I have read the Master Service Agreement** check box and click **Place Order**.
11. Your new storage allocation is available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. Contact your sales representative to increase the number of your volumes. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations, see our [FAQs](BlockStorageFAQ.html)
 
## Ordering Performance for Block Storage

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
3. Select **Performance** from the **Select Storage Type** list.
4. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
   - If you selected a data center with improved capabilities (denoted with an `*` ), you can choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle, which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a block LUN is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
     **NOTE**: Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
5. Select the appropriate **Storage Size**.
6. Enter the IOPS in the **Specify IOPS** field.
7. Click **Continue**. You are shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
8. Click the **I have read the Master Service Agreement** check box and click **Place Order**.
9. Your new storage allocation is available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations, see our [FAQs](BlockStorageFAQ.html)

## Identifing my {{site.data.keyword.blockstorageshort}} on my invoice

All LUNs appear on your invoice as a line item. Endurance appears as “Endurance Storage Service” and Performance appears as "Performance Storage Service" The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.blockstorageshort}}.
