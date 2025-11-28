---

copyright:
  years: 2014, 2025
lastupdated: "2025-11-28"

keywords: Block Storage for Classic, iSCSI LUN, secondary storage, SLCLI, API, provisioning, cloning, replication, duplicate volume

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}
{: ui-linked}


# Ordering {{site.data.keyword.blockstorageshort}}
{: #orderingBlockStorage}

You can provision {{site.data.keyword.blockstorageshort}} and fine-tune to meet your capacity and performance needs. Get the most out of your storage with two options for specifying performance.
{: shortdesc}

- You can provision with **Endurance** tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental-level file shares. The CLI and API responses show this tier as `LOW_INTENSITY_TIER`.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor. The CLI and API responses show this tier as `READHEAVY_TIER`.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases. The CLI and API responses show this tier as `WRITEHEAVY_TIER`.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available for storage that is provisioned up to 4 TB.The CLI and API responses show this tier as `10_IOPS_PER_GB`.

- You can fine-tune your storage to meet specific performance requirements and build a high-powered **Performance** environment by specifying the total number of input/output operations per second (IOPS). The available **custom** IOPS range depends on the volume capacity. The following table shows the available IOPS ranges based on volume size.

     | Volume size (GB) | IOPS range |
     |-------------|-----------------|
     | 10 - 39     | 100 - 1,000 |
     | 40 - 79     | 100 - 2,000 |
     | 80 - 99     | 100 - 4,000 |
     | 100 - 499   | 100 - 6,000 |
     | 500 - 999   | 100 - 10,000|
     | 1,000 - 1,999 | 100 - 20,000|
     | 2,000 - 2,999 | 200 - 40,000|
     | 3,000 - 3,999 | 200 - 48,000|
     | 4,000 - 7,999 | 300 - 48,000|
     | 8,000 - 9,999 | 500 - 48,000 |
     | 10,000 - 12,000 | 1,000 - 48,000 |
     {: caption="Available IOPS based on volume size" caption-side="bottom"}

By default, you can provision a combined total of 700 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. For more information about increasing limits, see [Managing Storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).
{: important}

## Ordering {{site.data.keyword.blockstorageshort}} in the console
{: #orderingthroughConsole}
{: ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](/catalog){: external}, and click **Storage**. Then, select **{{site.data.keyword.blockstorageshort}}**, and click **Create**.
2. Select your deployment location (region, location, zone).
   - Make sure that the new Storage is added in the same location as the Compute host or hosts that you have.
3. Billing. You can choose between Monthly or Hourly Billing.
   - With **hourly** billing, the number of hours the block volume existed on the account is calculated at the time the volume is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month.
   - With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. If a block volume is deleted before the end of the billing cycle, the difference is not refunded. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

4. Enter your storage size in the **Size** field.
5. Select the size of the Snapshot space from the list.

   This space is in addition to your usable space. For snapshot space considerations and recommendations, read [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).
   {: tip}

6. Choose your **OS Type** from the list.

   This selection is based on the operating system that your host is running on and it cannot be modified later. For example, if your server is Ubuntu or RHEL, select Linux&reg;. If your host is a Windows 2012 or Windows 2016 server, select the Windows 2008+ option from the list. For more information about various Windows options, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).
   {: tip}

7. Select your IOPS profile. You can choose between the predefined values of **Endurance (Tiers)** or enter your custom IOPS value for **Performance**.
8. In the side panel, review your order summary, and apply your Promo Code if you have one.

   Discounts are applied when the order is processed.
   {: note}

9. Acknowledge that you reviewed the terms and conditions by checking the appropriate box.
10. Click **Create**. Your new storage allocation is available in a few minutes.

## Ordering {{site.data.keyword.blockstorageshort}} from the CLI
{: #orderingthroughCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

Each order must have an associated location (data center). When you order {{site.data.keyword.blockstorageshort}}, make sure that it is provisioned in the same location as your Compute instances.
{: important}

### Provisioning from the IBMCLOUD CLI
{: #orderingthroughICCLI}

Use the `ibmcloud sl block volume-order` command to order a new block volume. The following example provisions a new 500-GB block volume in the DAL13 data center with a tiered performance profile (4 IOPS per GB) and 500 GB snapshot space.

```sh
$ ibmcloud sl block volume-order --storage-type endurance --size 500 --tier 4 -d dal13 --snapshot-size 500 --os-type LINUX
This action will incur charges on your account. Continue?> y
OK
Order 110758744 was placed.
 > Storage as a Service
 > Block Storage
 > 500 GBs
 > 4 IOPS per GB
 > 500 GB (Snapshot Space)
```
{: codeblock}

You can run `ibmcloud sl block volume-list` command with `--order 110758744` parameter to find this block volume when it is ready.

For more information about all of the parameters that are available for this command, see [ibmcloud sl block volume-order](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_order).

### Provisioning from the SLCLI
{: #orderingthroughSLCLI}

Use the `slcli block volume-order` command to provision the block volume. The following example shows how to order a 10 GB {{site.data.keyword.blockstorageshort}} volume with 100 IOPS per GB.

```sh
$ slcli block volume-order --storage-type performance --size 20 --location dal10 --iops 100 --os-type LINUX --snapshot-size 20
Order #32076317 placed successfully!
> Storage as a Service
> Block Storage
> 20 GBs
> 100 IOPS
> 20 GB (Snapshot Space)
```
{: screen}

For more information about Window OS types, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).

## Ordering {{site.data.keyword.blockstorageshort}} with the API
{: #orderingthroughAPI}
{: api}

The method `order_block_volume` (storage_type, location, size, os_type, iops=None, tier_level=None, snapshot_size=None, service_offering='storage_as_a_service', hourly_billing_flag=False) places an order for a block volume.

For a successful order, you must specify the following parameters:
- `storage_type` – ‘performance’ or ‘endurance’
- `location` – Datacenter in which to order iSCSI volume
- `size` – Size of the volume, in GB
- `os_type` – OS Type to use for volume alignment, see help for list
- `iops` – Number of IOPS for a “Performance” order
- `tier_level` – Tier level to use for an “Endurance” order
- `snapshot_size` – The size of optional snapshot space, if snapshot space is also ordered (None if not ordered)
- `service_offering` – Requested offering package to use in the order (‘storage_as_a_service’, ‘enterprise’, or ‘performance’)
- `hourly_billing_flag` – Billing type, monthly (False) or hourly (True), default to monthly.

For more information about ordering {{site.data.keyword.blockstorageshort}} through the API, see [BlockStorageManager](https://softlayer-python.readthedocs.io/en/latest/api/managers/SoftLayer.managers.BlockStorageManager/){: external}.

To be able to access all the new features, order `Storage-as-a-Service Package 759`.
{: tip}

## Ordering {{site.data.keyword.blockstorageshort}} with Terraform
{: #orderingthroughTerraform}
{: terraform}

To use Terraform, download the Terraform CLI and configure the {{site.data.keyword.cloud_notm}} Provider plug-in. For more information, see [Getting started with Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
{: requirement}

### Provision Endurance {{site.data.keyword.blockstorageshort}} with Terraform
{: #order-endurance-terraform}

You can use the following example to create a 20 GB block storage volume with 10 GB snapshot capacity and 0.25 IOPS/GB performance tier in the DAL09 data center.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal09"
        capacity = 20
        iops = 0.25
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        snapshot_capacity = 10
        hourly_billing = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

### Provision Performance {{site.data.keyword.blockstorageshort}} with Terraform
{: #order-performance-terraform}

You can use the following example to create a 20 GB block storage volume with custom 100 IOPS performance level.

```terraform
resource "ibm_storage_block" "test2" {
        type = "Performance"
        datacenter = "dal09"
        capacity = 20
        iops = 100
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        hourly_billing = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

## Connecting your new storage
{: #mountingnewLUN}

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [RHEL 9]{: tag-linux} [Mount iSCSI volume on Red Hat Enterprise Linux&reg; 9](/docs/BlockStorage?topic=BlockStorage-mountingRHEL).
- [CloudLinux 8]{: tag-linux} [Mount iSCSI volume on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8).
- [Ubuntu 24]{: tag-linux} [Mount iSCSI volume on Ubuntu OS](/docs/BlockStorage?topic=BlockStorage-mountingUbuntu).
- [Windows]{: tag-windows}[Mapping volumes on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows).
- [cPanel]{: tag-app}[Configuring {{site.data.keyword.blockstorageshort}} for backup with cPanel](/docs/BlockStorage?topic=BlockStorage-cPanelBackups).
- [Plesk]{: tag-app} [Configuring {{site.data.keyword.blockstorageshort}} for backup with Plesk](/docs/BlockStorage?topic=BlockStorage-PleskBackups).

## Disaster recovery considerations
{: #DRconsiderations}

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](/docs/BlockStorage?topic=BlockStorage-replication).

If you want to clone your volume and use it independently from the original volume, see [Creating and managing duplicate volumes](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).

## Identifying {{site.data.keyword.blockstorageshort}} on your invoice
{: #LUNonInvoice}

All volumes appear on your invoice as a line item. Endurance appears as “Endurance Storage Service” and Performance appears as "Performance Storage Service" The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.blockstorageshort}}.
