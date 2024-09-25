---

copyright:
  years: 2014, 2024
lastupdated: "2024-09-25"

keywords: Block Storage for Classic, snapshot space, ordering snapshots,

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Ordering Snapshots
{: #orderingsnapshots}

To create snapshots of your storage volume, you need to purchase space to hold them. You can purchase snapshot capacity during the initial volume purchase or later by using these steps.
{: shortdesc}

## Determining how much snapshot space is needed
{: #determinesnapshotspace}

Generically speaking, snapshot space is used by snapshots based on two key factors:
- How much your active file system changes over time.
- How long you plan to retain snapshots.

The way to calculate the amount of space that you need is **(Rate of Change)** x **(number of hours/days/weeks/months data is kept)**.

The first snapshot uses a negligible amount of space as it's just a copy of the metadata (pointers) that indicates the active file system blocks.
{: note}

A volume with numerous changes and a lengthy retention period needs more space than a volume with moderate changes and a moderate retention schedule. An example for the first type is a high change rate database. An example for the second type is a VMware&reg; datastore.

If you take 12 hourly snapshots of 500 GB of actual data, and the change rate is 1 percent between each snapshot, you end up with 60 GB for snapshots.

    *(5-GB Rate of Change) x (12 hourly snapshots) = (60 GB of used space)*

Conversely, if that 500 GB of actual data, with 12 hourly snapshots, saw 10 percent of change every hour, the snapshot space that is used is 600 GB.

    *(50-GB Rate of Change) x (12 hourly snapshots) = (600 GB of used space)*

So when you determine how much Snapshot space that you need, consider the rate of change carefully. It's a huge influence on how much snapshot space is needed. A bigger volume is more likely to change more often. However, a 500-GB volume with 5 GB of change and a 10-TB volume with 5 GB of change use the same amount of snapshot space.

Additionally, for most workloads, the larger a volume is the less space needs to be set aside initially. It's primarily due to the underlying data efficiencies, and the nature of how snapshots work in the environment.

## Available snapshot space capacity 
{: #snapshotspaceforvolume}

The size of the snapshot space that you can order depends on the size of your volume. The size of the snapshot space cannot exceed the size of the volume. The following table shows the snapshot capacity that is available for specific volume sizes.

| Volume capacity | Available Snapshot capacity         |
|----------------------|------------------------------------------|
|   10 GB | 5 GB, 10 GB  |
|   50 GB | 5 GB, 10 GB, 20 GB, 40 GB |
|  100 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB |
|  200 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB |
|  300 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB |
|  400 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB |
|  500 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB  |
|  600 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB |
|  700 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB |
|  800 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB |
|  900 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB |
| 1000 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB, 1000 GB |
| 2000 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB, 1000 GB, 2000 GB |
| 3000 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB, 1000 GB, 2000 GB |
| 4000 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB, 1000 GB, 2000 GB, 4000 GB |
| 5000 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB, 1000 GB, 2000 GB, 4000 GB |
|12000 GB | 5 GB, 10 GB, 20 GB, 40 GB, 60 GB, 80 GB, 100 GB 150 GB, 200 GB, 250 GB, 300 GB, \n 350 GB, 400 GB, 450 GB, 500 GB, 600 GB, 700 GB, 1000 GB, 2000 GB, 4000 GB |
{: caption="Table 1 - This table shows the available snapshot space in increments for different volume capacities." caption-side="bottom"}


## Ordering Snapshot space in the console
{: #ordersnapshotUI}
{: ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/catalog){: external}, and click the menu icon ![Menu icon](../icons/icon_hamburger.svg "Menu"). Then, select **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure**.
2. Access your Storage LUN through **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions"), then click **Change Snapshot Space**.
4. Select the storage size that you need.
5. Click **Continue**.
6. Enter any **Promo Code** that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.

   Discounts are applied when the order is processed.
   {: note}

7. Review your order, and read the service agreement. If you agree with the terms, check the box. Cick **Place Order**. Your snapshot space is provisioned in a few minutes.

## Ordering Snapshot space from the CLI
{: #ordersnapshotCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Provisioning from the IBMCLOUD CLI
{: #orderingsnapthroughICCLI}

Use the `ibmcloud sl block snapshot-order` command to provision snapshot space for your volume. The following example creates a 1000 GB snapshot space for the volume, which is identified by its ID `560156918`.

```sh
ibmcloud sl block snapshot-order 560156918 -s 1000 -t 4
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-order](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_order){: external}.

### Provisioning from the SLCLI
{: #orderingsnapthroughSLCLI}

Use the `slcli block snapshot-order` command to provision snapshot space for your volume. 

```sh
$ slcli block snapshot-order --help
Usage: slcli block snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the block
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
{: codeblock}

## Ordering Snapshot space with Terraform
{: #ordersnapshotTerraform}
{: terraform}

When you provision a storage volume with Terraform, use the `ibm_storage_block` resource and specify the `snapshot_capacity` argument. The following example defines 10 GB snapshot space.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal13"
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
