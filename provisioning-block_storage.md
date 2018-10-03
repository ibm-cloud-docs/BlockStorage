---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-03"

---
{:new_window: target="_blank"}

# Ordering {{site.data.keyword.blockstorageshort}}

You can provision {{site.data.keyword.blockstorageshort}} and fine tune to meet your capacity and IOPS needs. Get the most out of your storage with two options for specifying performance.

- You can choose from Endurance IOPs tiers that feature pre-defined performance levels to fit workloads that don't have well defined performance requirements. 
- You can fine tune your storage to meet very specific performance requirements by specifying the total number of IOPS with Performance.

## Ordering {{site.data.keyword.blockstorageshort}} with pre-defined IOPS Tiers (Endurance)

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In the upper right, click **Order {{site.data.keyword.blockstorageshort}}**.
3. Select your deployment **Location** (data center).
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
4. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing. 
     1. With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a block LUN is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
        >**NOTE** - Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
5. Enter your storage size in the **New Storage Size** field.
6. Select **Endurance (tiered IOPS)** in the **Storage IOPS Options** section.
7. Select the IOPS tier that your application needs.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) for storage that is provisioned up to 4 TB.
8. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](ordering-snapshots.html).
9. Choose your **OS Type** from the list.
10. Select the checkboxes of **Terms and Conditions**, and click **Place Order**.
11. Your new storage allocation is available in a few minutes.

>**Note** - By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).<br/><br/>For the limit on simultaneous authorizations, see the [FAQs](BlockStorageFAQ.html).
 
## Ordering {{site.data.keyword.blockstorageshort}} with Custom IOPS (Performance)

1. From the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, click **Storage**, **{{site.data.keyword.blockstorageshort}}** OR from the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In the upper right, click **Order {{site.data.keyword.blockstorageshort}}**.
3. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
4. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a block LUN is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).
        >**NOTE** - Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
5. Enter your storage size in the **New Storage Size** field.
6. Select **Performance (Allocated IOPS)** in the **Storage IOPS Options** section.
7. Enter the IOPS in the **Performance (Allocated IOPS)** field.
8. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](ordering-snapshots.html).
9. Choose your **OS Type** from the list.
10. Your new storage allocation is available in a few minutes.

>**Note** - By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](managing-storage-limits.html).<br/><br/>For the limit on simultaneous authorizations, see the [FAQs](BlockStorageFAQ.html).

## Connecting your new storage

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Connecting to MPIO iSCSI LUNs on Linux](accessing_block_storage_linux.html)
- [Connecting to MPIO iSCSI LUNs on CloudLinux](configure-iscsi-cloudlinux.html)
- [Connecting to MPIO iSCSI LUNS on Microsoft Windows](accessing-block-storage-windows.html)
- [Configuring Block Storage for Backup with cPanel](configure-backup-cpanel.html)
- [Configuring Block Storage for Backup with Plesk](configure-backup-plesk.html)

## Identifying {{site.data.keyword.blockstorageshort}} on your invoice

All LUNs appear on your invoice as a line item. Endurance appears as “Endurance Storage Service” and Performance appears as "Performance Storage Service" The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.blockstorageshort}}.
