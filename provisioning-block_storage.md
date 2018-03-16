---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Ordering {{site.data.keyword.blockstorageshort}}

There are two different types of storage you can provision based on your needs and preferences. The two options of {{site.data.keyword.blockstorageshort}} volumes are: 

- **Endurance**: provision Endurance tiers featuring pre-defined performance levels and features like snapshots and replication. 
- **Performance**: build a high powered Performance environment where you can allocate the desired input/output operations per second (IOPS).

## How to order Endurance for {{site.data.keyword.blockstorageshort}}

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} Catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In the top right corner, click on **Order {{site.data.keyword.blockstorageshort}}** link.
3. Select **Endurance** from the Select Storage Type drop-down list.
4. Click the drop-down list and select your deployment **Location** (data center).
   - Ensure that the new Storage will be added in the same location as the previously ordered host(s).
   - If you selected a data center with improved capabilities (denoted with an * in the drop-down), you will have the option to choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the calculation of the number of hours the block LUN existed on the account is performed at the time the LUN is deleted or at the end of the billing cycle, which ever comes first.  Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There is no refund If a block LUN is deleted before the end of the billing cycle.  Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
     **NOTE**: Monthly billing type is used by default for storage provisioned in data centers that are **not** updated with improved capabilities.
5. Select the radio button next to the IOPS tier needed for your application(s).
    - *0.25 IOPS per GB* is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a given time. Example applications include storing mailboxes or departmental level file shares.
    - *2 IOPS per GB* is designed for most general purpose usage. Example applications include hosting small databases backing web applications or virtual machine disk images for a hypervisor.
    - *4 IOPS per GB* is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a given time. Example applications include transactional and other performance-sensitive databases.
    - *10 IOPS per GB* is designed for the most demanding workloads such as those created by NoSQL Databases, and data processing for Analytics.  This tier is available in [select datacenters](new-ibm-block-and-file-storage-location-and-features.html) for storage provisioned up to 4TB in size.
6. Click the **Select Storage Size** drop-down list and select your storage size.
7. Click the **Specify Snapshot Space Size** drop-down list and select the snapshot size (in addition to your usable space). For spanshot space considerations and recommendation, please read [Ordering Snapshots](ordering-snapshots.html).
8. Choose your **OS Type** from the drop-down list.
9. Click the drop-down list and select your deployment Location (data center).
10. Click **Continue**. You’re shown the monthly and prorated charges with a final chance to review order details.
11. Click the **I have read the Master Service Agreement** checkbox and click the **Place Order** button.
12. Your new storage allocation should be available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes please contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations please see our [FAQs](BlockStorageFAQ.html)
 
## How to order Performance for Block Storage

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} Catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. Click on **Order {{site.data.keyword.blockstorageshort}}** in the top right corner of the screen.
3. Select **Performance** from the **Select Storage Type** drop-down list.
4. Click the **Location** drop-down list and select your data center.
   - Ensure that the new Storage will be added in the same location as the previously ordered host(s).
   - If you selected a data center with improved capabilities (denoted with an * in the drop-down), you will have the option to choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the calculation of the number of hours the block LUN existed on the account is performed at the time the LUN is deleted or at the end of the billing cycle, which ever comes first.  Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There is no refund If a block LUN is deleted before the end of the billing cycle.  Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
     **NOTE**: Monthly billing type is used by default for storage provisioned in data centers that are **not** updated with improved capabilities.
5. Select the radio button next to the appropriate **Storage Size**.
6. Enter the IOPS in the **Specify IOPS** field.
7. Click **Continue**. You are shown the monthly and prorated charges with a final chance to review order details. Click **Previous** if you want to change your order.
8. Click the **I have read the Master Service Agreement** checkbox and click the **Place Order Button**.
9. Your new storage allocation should be available in a few minutes.

**Note**: By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes please contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).

For the limit on simultaneous authorizations please see our [FAQs](BlockStorageFAQ.html)

## How to identify my {{site.data.keyword.blockstorageshort}} on my invoice

All LUNs will appear on your invoice as a line item; Endurance will appear as “Endurance Storage Service” and Performance will appear as "Performance Storage Service" The rate will vary based on your storage level. You can then expand on Endurance or Performance to see that it is {{site.data.keyword.blockstorageshort}}.
