---

copyright:
  years: 2018, 2021
lastupdated: "2021-04-29"

keywords: Block storage, new feature, adjusting IOPS, modify IOPS, increase IOPS, decrease IOPS,

subcollection: BlockStorage

---
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:external: target="_blank" .external}

# Adjusting IOPS
{: #adjustingIOPS}

With this feature, {{site.data.keyword.blockstoragefull}} storage users can adjust the IOPS of their existing {{site.data.keyword.blockstorageshort}} immediately. They don't need to create a duplicate or manually copy data to new storage. Users don't experience any kind of outage or lack of access to the storage while the adjustment is taking place.
{: shortdesc}

Billing for the storage is updated to add the pro-rated difference of the new price to the current billing cycle. The full new amount is billed in the next billing cycle.


## Advantages of Adjustable IOPS
{: #advantagesofresizingiops}

- Cost management – Some clients might need high IOPS only during peak usage times. For example, a large retail store has peak usage during the holidays and might need higher IOPS rate on their storage then. However, they don't need higher IOPS in the middle of the summer. This feature allows them to manage their costs and pay for higher IOPS when they need it.

## Limitations
{: #limitsofIOPSadjustment}

This feature is available in [most data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

Clients can't switch between Endurance and Performance when they adjust their IOPS. However, they can specify a new IOPS tier or IOPS level for their storage based on the following criteria and restrictions:

- If original volume is Endurance 0.25 tier, IOPS tier can’t be updated.
- If original volume is Performance with less than or equal to 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in less than or equal to 0.30 IOPS/GB.
- If original volume is Performance with more than 0.30 IOPS/GB, options available include only the size and IOPS combinations that result in more than 0.30 IOPS/GB.

## Effect of IOPS adjustment on replication
{: #iopschangeeffectonreplicas}

If the volume has replication in place, the replica is automatically updated to match the IOPS selection of the primary.

## Adjusting the IOPS on your Storage in the UI
{: #adjustingstepsUI}
{: ui}

1. Go to your list of {{site.data.keyword.blockstorageshort}}. From the {{site.data.keyword.cloud}} console, click the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu") icon, then click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the iSCSI volume from the list and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Modify Volume**.
3. Under **Adjust Storage IOPS**, make a new selection:
    - For Endurance (Tiered IOPS), select an IOPS Tier greater than 0.25 IOPS/GB of your storage. You can increase the IOPS tier at any time. However, decreasing is available only once a month.
    - For Performance (Allocated IOPS), specify new IOPS option for your storage by entering a value in the range 100-48,000 IOPS.

    Be sure to look at any specific boundaries that are required by size in the order form.
    {: tip}

4. Review your selection and the new pricing.
5. Click **Modify**.
6. Your new storage allocation is available in a few minutes.


## Adjusting the IOPS on your Storage from the SLCLI
{: #adjustingstepsCLI}
{: cli}

By using the following command, you can adjust the IOPS through the SLCLI.
```zsh
# slcli block volume-modify --help
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
{: codeblock}

## Adjusting the IOPS on your Storage with the API
{: #adjustingstepsAPI}
{: api}

You can adjust the IOPS by using an API call to the SOAP web service. The following sample API calls can be called from the scripting language of your choice.

For more information about the SLAPI, see the [SLDN](http://sldn.softlayer.com/reference/softlayerapi){: external}.
{: tip}

* Adjust IOPS on Performance storage volume.

   ```zsh
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
              <id xsi:type="xsd:int">XXXXXXXXX</id><!-- where XXXXXXXXX is the Volume Id -->
          </volume>
          <iops xsi:type="xsd:int">2007</iops> <!-- This is the upgraded amount -->
          <packageId xsi:type="xsd:int">759</packageId>
          <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">189433</id> <!-- Top level price -->
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">190233</id> <!-- 2000 - 2999 GBs storage price-->
              </item>
              <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                  <id xsi:type="xsd:int">190293</id> <!-- 200 - 40000 IOPS price-->
              </item>
          </prices>
        </orderData>
      </ns1:placeOrder>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}

* Adjust IOPS on Endurance storage volume.

   ```zsh
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
              <id xsi:type="xsd:int">XXXXXXXX</id> <!--Where XXXXXXXX is the VolumeID -->
          </volume>
          <packageId xsi:type="xsd:int">759</packageId>
          <volumeSize xsi:type="xsd:int">24</volumeSize>
          <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">189433</id> <!-- Top level price -->
            </item>
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">193373</id> <!-- New Performance tier price -->
            </item>
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">193433</id> <!-- Storage space price for the new tier -->
            </item>
          </prices>
        </orderData>
      </ns1:placeOrder>
    </SOAP-ENV:Body>
   </SOAP-ENV:Envelope>
   ```
   {: codeblock}
