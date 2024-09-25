---

copyright:
  years: 2018, 2024
lastupdated: "2024-09-25"

keywords: Block Storage for Classic, expand size, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}


# Expanding {{site.data.keyword.blockstorageshort}} Capacity
{: #expandingcapacity}

With this feature, {{site.data.keyword.blockstoragefull}} users can expand the size of their existing {{site.data.keyword.blockstorageshort}} in GB increments up to 12 TB immediately. You don't need to create a duplicate or manually migrate data to a larger volume. This feature is available in all [data centers](/docs/overview?topic=overview-locations#data-centers).
{: shortdesc}

Billing for the volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.
 
The upgrade process is not instantaneous. You can expect to see the updated size in the console or through the API in a short while after you put in the modification request. Resizing does not cause an outage or lack of access to the storage, so you can continue your operations as normal while you wait. 

When the expansion is complete, the host Operating system must rescan the volume, and reload the multipath device map to reflect the change in size. You must resize the partition and the file system to allocate the new unused capacity.
{: important}

## Advantages of Expandable Storage
{: #advantageofresizing}

- **Cost management** – You might know of a potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand allows our customers to save on the cost of storage, and later grow to accommodate their needs.

- **Growing Storage needs** - Customers who experience rapid data growth need a way to quickly and easily increase the size of their storage to manage it.

## Effects of expanding storage capacity on Replication
{: #effectofcapacityincrease}

The expand action on the primary storage results in automatic resizing of the replica.

## Limitations
{: #limitsofexpandingstorage}

Storage that was provisioned before the release of this feature, during **April 2017 - 14 December 2017**, can be increased to 10 times its original size and no more. Storage that was provisioned after **14 December 2017** can be increased to 12 TB.

Existing size limitations for {{site.data.keyword.blockstorageshort}} that was provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

You can't change the block storage to a smaller size after you expand its capacity.
{: note}

## Resizing storage in the console
{: #resizingstepsUI}
{: ui}

1. From the {{site.data.keyword.cloud}} console, click the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu") icon. Then, click **Infrastructure**  ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the iSCSI volume from the list and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Modify volume**.
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click **Modify**.
6. Your new storage allocation is available in a few minutes.

The Operating system must rescan the storage and reload the multipath device map to reflect the expanded volume size. Resizing of the partition and file system are also required. For more information about expanding the file system, see your OS Documentation. For example, [RHEL 8 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external} or [Microsoft - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.
{: tip}

## Resizing storage from the CLI
{: #resizingstepsCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Resizing a block volume from the IBMCLOUD CLI
{: #resizingstepsICCLI}

You can increase the capacity of a volume by using the `ibmcloud sl block volume-modify` command. The following example modifies a block volume by specifying a new, bigger capacity.

```sh
ibmcloud sl block volume-modify 12345678 --new-size 1000
```
{: pre}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block volume-modify](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_modify){: external}.

### Resizing a block volume from the SLCLI
{: #resizingstepsSLCLI}

To increase your storage capacity, you can use the following command in SLCLI.

```sh
$ slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help                    Show this message and exit.
```
{: screen}

The Operating system must rescan the storage and reload the multipath device map to reflect the expanded volume size. Resizing of the partition and file system are also required. For more information about expanding the file system, see your OS Documentation. For example, [RHEL 8 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external} or [Microsoft - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.
{: tip}

## Resizing storage with the API
{: #resizingstepsAPI}
{: api}

You can adjust the IOPS by making an API call to the SOAP web service. The following sample API requests can be made from the scripting language of your choice.

For more information about the SLAPI, see the [SLDN](http://sldn.softlayer.com/reference/softlayerapi){: external}.
{: tip}

* The following example shows how to increase capacity on a Performance storage volume. `XXXXXXXX` is the ID of the volume that you want to increase to `2007` GBs. `189433`, `190233`, and `190293` are IDs of pricing information that is associated with the capacity and IOPS value of this volume.

   ```sh
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <SOAP-ENV:Header>
      <ns1:authenticate>
      </ns1:authenticate>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
      <ns1:placeOrder>
        <orderData xsi:type="ns1:SoftLayer_Container_Product_Order_Network_Storage_AsAService_Upgrade">
          <volume xsi:type="ns1:SoftLayer_Network_Storage">
              <id xsi:type="xsd:int">XXXXXXXX</id>
          </volume>
          <volumeSize xsi:type="xsd:int">2007</volumeSize>
          <packageId xsi:type="xsd:int">759</packageId>
          <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">189433</id>
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">190233</id>
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">190293</id>
              </item>
          </prices>
        </orderData>
      </ns1:placeOrder>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

* Increase capacity on an Endurance storage volume. `XXXXXXXX` is the ID of the volume that you want to increase to `250` GBs. `189433`, `196033`, and `196093` are IDs of pricing information that is associated with the capacity and IOPS value of this volume.

   ```sh
   <?xml version="1.0" encoding="UTF-8"?>
   <SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns1="http://api.service.softlayer.com/soap/v3.1/" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:SOAP-ENC="http://schemas.xmlsoap.org/soap/encoding/" SOAP-ENV:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/">
    <SOAP-ENV:Header>
      <ns1:authenticate>
      </ns1:authenticate>
    </SOAP-ENV:Header>
    <SOAP-ENV:Body>
      <ns1:placeOrder>
        <orderData xsi:type="ns1:SoftLayer_Container_Product_Order_Network_Storage_AsAService_Upgrade">
          <volume xsi:type="ns1:SoftLayer_Network_Storage">
              <id xsi:type="xsd:int">XXXXXXXX</id>
          </volume>
          <packageId xsi:type="xsd:int">759</packageId>
            <volumeSize xsi:type="xsd:int">250</volumeSize>
          <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">189433</id>
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">196033</id>
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">196093</id>
              </item>
          </prices>
        </orderData>
      </ns1:placeOrder>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

The Operating system must rescan the storage and reload the multipath device map to reflect the expanded volume size. Resizing of the partition and file system are also required. For more information about expanding the file system, see your OS Documentation. For example, [RHEL 8 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external} or [Microsoft - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.
{: tip}

## Resizing storage with Terraform
{: #resizingstepsTerraform}
{: terraform}

You can increase your storage capacity by using the `ibm_storage_block` resource, and specifying a larger number in the capacity argument. The following example increases the capacity of an Endurance volume to 40 GB.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal09"
        capacity = 40
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

The following example increases the capacity of a Performance volume to 40 GB.

```terraform
resource "ibm_storage_block" "test2" {
        type = "Performance"
        datacenter = "dal09"
        capacity = 40
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

The Operating system must rescan the storage and reload the multipath device map to reflect the expanded volume size. Resizing of the partition and file system are also required. For more information about expanding the file system, see your OS Documentation. For example, [RHEL 8 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external} or [Microsoft - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.
{: tip}

## Expanding Storage over 12 TB
{: #increasecapacityover12TB}
{: support}

If you need to increase your Storage volume capacity beyond 12 TB, you can request to be added to the allowlist by submitting a [support case](/unifiedsupport/cases/add){: external}. When the request is approved by the Offering Manager, you're going to be notified through the case process. You're also going to see the option to increase your storage up to 16 TB in the console.
{: preview}
