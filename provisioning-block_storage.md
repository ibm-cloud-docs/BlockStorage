---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Ordering {{site.data.keyword.blockstorageshort}} through the Console
{: #orderingthroughConsole}

You can provision {{site.data.keyword.blockstorageshort}} and fine-tune to meet your capacity and IOPS needs. Get the most out of your storage with two options for specifying performance.

- You can choose from Endurance IOPs tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
- You can fine-tune your storage to meet specific performance requirements by specifying the total number of IOPS with Performance.

## Ordering {{site.data.keyword.blockstorageshort}} with pre-defined IOPS Tiers (Endurance)

1. Log in to [The IBM Cloud catalog](https://{DomainName}/catalog/){:new_window}, and click **Storage**. Then, select **{{site.data.keyword.blockstorageshort}}**, and click **Create**.

   Alternatively, you can log in to the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.blockstorageshort}}**. In the upper right, click **Order {{site.data.keyword.blockstorageshort}}**.

2. Select your deployment **Location** (data center).
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a block LUN is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

        Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
        {:important}
4. Enter your storage size in the **New Storage Size** field.
5. Select **Endurance (tiered IOPS)** in the **Storage IOPS Options** section.
6. Select the IOPS tier that your application needs.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [select data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-news) for storage that is provisioned up to 4 TB.
7. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Choose your **OS Type** from the list.<br/>

   This selection is based on the operating system that your host is running on and it cannot be modified later. For example, your server is Ubuntu or RHEL, select Linux. If your host is a Windows 2012 or Windows 2016 server, select the Windows 2008+ option from the list. For more information about various Windows options, see the [FAQ](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
   {:tip}
9. On the right, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {:note}
10. After you reviewed the terms and conditions, check the I** have read and agree to the Third-Party Service Agreements** box.
11. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>For the limit on simultaneous authorizations, see the [FAQs](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Ordering {{site.data.keyword.blockstorageshort}} with Custom IOPS (Performance)

1. Log in to [The IBM Cloud catalog](https://{DomainName}/catalog/){:new_window}, and click **Storage**. Then, select {{site.data.keyword.blockstorageshort}}, and click **Create**.

   Alternatively, you can log in to the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}, click **Storage** > **{{site.data.keyword.blockstorageshort}}**. In the upper right, click **Order {{site.data.keyword.blockstorageshort}}**.
2. Click **Location** and select your data center.
   - Ensure that the new Storage is added in the same location as the compute host or hosts that you have.
3. Billing. If you selected a data center with improved capabilities (marked with an asterisk), you can choose between Monthly or Hourly Billing.
     1. With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage that is provisioned in these [select data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There's no refund if a block LUN is deleted before the end of the billing cycle. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

        Monthly billing type is used by default for storage that is provisioned in data centers that are **not** updated with improved capabilities.
        {:note}
4. Enter your storage size in the **New Storage Size** field.
5. Select **Performance (Allocated IOPS)** in the **Storage IOPS Options** section.
6. Enter the IOPS in the **Performance (Allocated IOPS)** field.
7. Click **Specify Snapshot Space Size** and select the snapshot size from the list. This space is in addition to your usable space. For snapshot space considerations and recommendation, read [Ordering Snapshots](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Choose your **OS Type** from the list.<br/>

   This selection is based on the operating system that your host is running on and it cannot be modified later. For example, your server is Ubuntu or RHEL, select Linux. If your host is a Windows 2012 or Windows 2016 server, select the Windows 2008+ option from the list. For more information about various Windows options, see the [FAQ](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
   {:tip}
9. On the right, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {:note}
10. After you reviewed the terms and conditions, check the I** have read and agree to the Third-Party Service Agreements** box.
11. Click **Create**. Your new storage allocation is available in a few minutes.

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase the number of your volumes, contact your sales representative. Read about increasing limits [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>For the limit on simultaneous authorizations, see the [FAQs](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Connecting your new storage
{: #mountingnewLUN}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Connecting to LUNs on Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connecting to LUNs on CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connecting to LUNS on Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Mounting a LUN in XenServer Shared Storage](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Configuring Block Storage for backup with cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuring Block Storage for backup with Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Disaster recovery considerations

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).

If you want to clone your volume and use it independently from the original volume, see [Creating a duplicate Block Volume](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Identifying {{site.data.keyword.blockstorageshort}} on your invoice

All LUNs appear on your invoice as a line item. Endurance appears as “Endurance Storage Service” and Performance appears as "Performance Storage Service" The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.blockstorageshort}}.
