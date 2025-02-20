---

copyright:
  years: 2018, 2025
lastupdated: "2025-02-20"

keywords: Block Storage for Classic, new feature, adjusting IOPS, modify IOPS, increase IOPS, decrease IOPS,

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Adjusting IOPS
{: #adjustingIOPS}

With this feature, you can adjust the IOPS of your existing {{site.data.keyword.blockstorageshort}} immediately. You don't need to create a duplicate or manually copy data to new storage. The adjustment does not cause any kind of outage or lack of access to the storage.
{: shortdesc}

Billing for the storage is updated to add the prorated difference of the new price to the current billing cycle. The full new amount is billed in the next billing cycle.

## Advantages of Adjustable IOPS
{: #advantagesofresizingiops}

- Cost management – Some clients might need high IOPS only during peak usage times. For example, a large retail store has peak usage during the holidays and might need a higher IOPS rate on the storage then. However, the retail stores don't need higher IOPS in the middle of the summer. With this feature, you can manage your costs and pay for higher IOPS when you need it.

## Limitations
{: #limitsofIOPSadjustment}

You can't switch between Endurance and Performance when you adjust their IOPS. However, you can specify a new IOPS for the storage based on the following criteria:

| Volume size (GB) |  IOPS range |
|------------------|-------------|
| 10 - 39          | 100 - 1,000 |
| 40 - 79          | 100 - 2,000 |
| 80 - 99          | 100 - 4,000 |
| 100 - 499        | 100 - 6,000 |
| 500 - 999        | 100 - 10,000|
| 1,000 - 1,999    | 100 - 20,000|
| 2,000 - 2,999    | 200 - 40,000|
| 3,000 - 3,999    | 200 - 48,000|
| 4,000 - 7,999    | 300 - 48,000|
| 8,000 - 9,999    | 500 - 48,000|
| 10,000 - 12,000| 1,000 - 48,000|
{: caption="The list of available IOPS ranges based on volume size." caption-side="bottom"}

Maximum IOPS for block storage volumes varies based on volume size. The maximum IOPS for a volume is 48,000 IOPS.

## Effect of IOPS adjustment on replication
{: #iopschangeeffectonreplicas}

If the volume has replication in place, the replica is automatically updated to match the IOPS selection of the primary.

## Adjusting the IOPS on your Storage in the console
{: #adjustingstepsUI}
{: ui}

1. Go to your list of {{site.data.keyword.blockstorageshort}}. Click the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu") icon, then click **Infrastructure**  ![Classic icon](../icons/classic.svg "Classic") > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the iSCSI volume from the list and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Modify Volume**.
3. Under **Adjust Storage IOPS**, make a new selection:
    - For Endurance (Tiered IOPS), select a different tier. You can increase the IOPS tier at any time. However, decreasing is available only once a month.
    - For Performance (Allocated IOPS), specify a new IOPS option for your storage by entering a value in the range 100-48,000 IOPS. The UI notifies you if the selection is not valid for your volume size.

4. Review your selection and the new pricing.
5. Click **Modify**.
6. Your new storage allocation is available in a few minutes.

## Adjusting the IOPS on your Storage from the CLI
{: #adjustingstepsCLI}
{: cli}

Before you can begin the process, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Adjusting the IOPS from the IBMCLOUD CLI
{: #adjustIOPSICCLI}

You can update the IOPS by using the `ibmcloud sl block volume-modify` command. The following example modifies a Performance volume by specifying a new IOPS value.

```sh
ibmcloud sl block volume-modify 12345678 --new-iops 4000
```
{: codeblock}

The following example modifies an Endurance file share by specifying a new IOPS tier.

```sh
ibmcloud sl block volume-modify 12345678 --new-tier 4
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block volume-modify](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_modify){: external}.

### Adjusting the IOPS from the SLCLI
{: #adjustIOPSSLCLI}

You can update the IOPS by using the following command.

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
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
  -h, --help                    Show this message and exit.
```
{: screen}

## Adjusting the IOPS on your Storage with the API
{: #adjustingstepsAPI}
{: api}

You can adjust the IOPS by making an API call to the SOAP web service. The following sample API requests can be made from the scripting language of your choice.

For more information about the SLAPI, see the [SLDN](http://sldn.softlayer.com/reference/softlayerapi){: external}.
{: tip}

* The following example shows how to request IOPS change on a Performance storage volume. `XXXXXXXXX` is the ID of the volume that you want to modify. `3000` is the new IOPS value that you want your volume to have. `189433` is the ID of the max level price. `190233` is the ID of 2000 - 2999 GBs capacity range. `190293` is the ID for the 200 - 40000 IOPS range.

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
              <id xsi:type="xsd:int">XXXXXXXXX</id>
          </volume>
          <iops xsi:type="xsd:int">3000</iops>
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

* The following example shows how to adjust IOPS on Endurance storage volume. `XXXXXXXXX` is the ID of the volume that you want to modify. `4` is the new IOPS tier value that you want your volume to have. `189433` is the ID of the max level price. `193373` and`193433` are the IDs for the price ranges for the capacity and IOPS that the volume has.

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
          <iops xsi:type="xsd:int">4</iops>
          <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">189433</id>
            </item>
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">193373</id>
            </item>
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">193433</id>
            </item>
          </prices>
        </orderData>
      </ns1:placeOrder>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

## Adjusting the IOPS on your Storage with Terraform
{: #adjustingstepsTerraform}
{: terraform}

You can adjust the IOPS by using the `ibm_storage_block` resource, and specifying a different number in the `iops` argument. The following example increases the performance tier of an Endurance volume to the 4 IOPS/GB tier.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal09"
        capacity = 40
        iops = 4
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        snapshot_capacity = 10
        hourly_billing = true
}
```
{: codeblock}

The following example changes the performance level of a Performance volume to 150 IOPS.

```terraform
resource "ibm_storage_block" "test2" {
        type = "Performance"
        datacenter = "dal09"
        capacity = 40
        iops = 150
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        hourly_billing = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.
