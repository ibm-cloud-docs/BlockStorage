---

copyright:
  years: 2014, 2023
lastupdated: "2023-10-10"

keywords: Block Storage, iSCSI LUN, secondary storage, SLCLI, API, provisioning, cloning, replication, duplicate volume

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}
{: ui-linked}


# Ordering {{site.data.keyword.blockstorageshort}}
{: #orderingBlockStorage}

You can provision {{site.data.keyword.blockstorageshort}} and fine-tune to meet your capacity and performance needs. Get the most out of your storage with two options for specifying performance.
{: shortdesc}

- You can provision with **Endurance** tiers that feature pre-defined performance levels to fit workloads that don't have well-defined performance requirements.
    - **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental-level file shares.
    - **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.
    - **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases.
    - **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available in [all data centers](/docs/BlockStorage?topic=BlockStorage-selectDC) for storage that is provisioned up to 4 TB.
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
     {: caption="Table 1. Available IOPS based on volume size" caption-side="bottom"}

By default, you can provision a combined total of 700 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. For more information about increasing limits, see [Managing Storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).
{: important}

## Ordering {{site.data.keyword.blockstorageshort}} in the UI
{: #orderingthroughConsole}
{: ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](/catalog){: external}, and click **Storage**. Then, select **{{site.data.keyword.blockstorageshort}}**, and click **Create**.
2. Select your deployment location (region, location, zone).
   - Ensure that the new Storage is added in the same location as the Compute host or hosts that you have.
3. Billing. You can choose between Monthly or Hourly Billing.
   - With **hourly** billing, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle. Which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month.
   - With **monthly** billing, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. If a block LUN is deleted before the end of the billing cycle, the difference is not refunded. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (month or longer).

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

## Ordering {{site.data.keyword.blockstorageshort}} through the SLCLI
{: #orderingthroughCLI}
{: cli}

You can use the SLCLI to place orders for products that are normally ordered through the [{{site.data.keyword.cloud_notm}} console](/login){: external}.

Each order must have an associated location (data center). When you order {{site.data.keyword.blockstorageshort}}, make sure that it is provisioned in the same location as your Compute instances.
{: important}

For more information about how to install and use the SLCLI, see [Python CLI Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

Use the `slcli block volume-order` command to provision the LUN.

```python
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Order a block storage volume.

Options:
 --storage-type [performance|endurance]
                                 Type of block storage volume  [required]
 --size INTEGER                  Size of block storage volume in GB.
                                 Permitted Sizes:
                                 20, 40, 80, 100, 250, 500,
                                 1000, 2000, 4000, 8000, 12000  [required]
 --iops INTEGER                  Performance Storage IOPS, between 100 and
                                 6000 in multiples of 100  [required for
                                 storage-type performance]
 --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                 [required for storage-type endurance]
 --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                 Operating System  [required]
 --location TEXT                 Datacenter short name (e.g.: dal09)
                                 [required]
 --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                 space along with endurance block storage;
                                 specifies the size (in GB) of snapshot space
                                 to order
 --service-offering [storage_as_a_service|enterprise|performance]
                                 The default is 'storage_as_a_service'
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

For more information about Window OS types, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).

### Example order
{: #exampleorder}
{: cli}

The following example shows how to order an 80 GB {{site.data.keyword.blockstorageshort}} volume with 20-GB Snapshot space and 0.25 IOPS per GB.

```python
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

For more information about ordering through the IBM Cloud CLI, see [Working with the Block Storage service (ibmcloud sl block)](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_order){: external}.
{: tip}

## Ordering {{site.data.keyword.blockstorageshort}} by using the API
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

For more information about ordering {{site.data.keyword.blockstorageshort}} through the API, see [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.

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
- [Connecting to LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connecting to LUNs on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connecting to LUNS on Microsoft&reg; Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
- [Mounting a LUN in XenServer Shared Storage](/docs/virtualization?topic=virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Configuring Block Storage for backup with cPanel](/docs/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuring Block Storage for backup with Plesk](/docs/BlockStorage?topic=BlockStorage-PleskBackups)

## Disaster recovery considerations
{: #DRconsiderations}

To avoid data-loss and to ensure business continuity, consider replicating your servers and storage in another data center. Replication keeps your data in sync in two different locations based on your snapshot schedule. For more information, see [Replicating data](/docs/BlockStorage?topic=BlockStorage-replication).

If you want to clone your volume and use it independently from the original volume, see [Creating and managing independent duplicate volumes](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).

If you want to clone your volume and refresh the duplicate on demand, see [Creating and managing dependent duplicate volumes](/docs/BlockStorage?topic=BlockStorage-dependentduplicate).

## Identifying {{site.data.keyword.blockstorageshort}} on your invoice
{: #LUNonInvoice}

All LUNs appear on your invoice as a line item. Endurance appears as “Endurance Storage Service” and Performance appears as "Performance Storage Service" The rate varies based on your storage level. You can expand on Endurance or Performance to see that it's {{site.data.keyword.blockstorageshort}}.
