---

copyright:
  years: 2018, 2021
lastupdated: "2021-04-29"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}


# Expanding {{site.data.keyword.blockstorageshort}} Capacity
{: #expandingcapacity}

With this feature, current {{site.data.keyword.blockstoragefull}} users can expand the size of their existing {{site.data.keyword.blockstorageshort}} in GB increments up to 12 TB immediately. They don't need to create a duplicate or manually migrate data to a larger volume.
{: shortdesc}

Billing for the volume is automatically updated to add the pro-rated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

This feature is available in [most data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

The upgrade process is not instantaneous. You can expect to see the updated size in the UI or through the API in a short while after you put in the modification request. There's no outage or lack of access to the storage while the resize is taking place, so you can continue your operations as normal while you wait.

## Advantages of Expandable Storage
{: #advantageofresizing}

- **Cost management** – You might know that there's potential for growth of your data, but you need a smaller amount of storage to start. The ability to expand allows our customers to save on cost of storage, and later grow to accommodate their needs.  

- **Growing Storage needs** - Customers who experience rapid data growth need a way to quickly and easily increase the size of their storage to manage it.

## Effects of expanding storage capacity on Replication

The expand action on the primary storage results in automatic resizing of the replica.

## Limitations
{: #limitsofexpandingstorage}

This feature is available for storage that is provisioned in [most data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

Storage that was provisioned in these data centers before the release of this feature, during **April 2017 - 14 December 2017**, can be increased to 10 times its original size and no more. Storage that was provisioned after **14 December 2017** can be increased up to 12 TB.

Existing size limitations for {{site.data.keyword.blockstorageshort}} that was provisioned with Endurance still apply (up to 4 TB for 10 IOPS tier and up to 12 TB for all other tiers).

## Resizing storage in the UI
{: #resizingstepsUI}
{: ui}

1. From the {{site.data.keyword.cloud}} console, click the **menu** icon. Then, click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the iSCSI volume from the list and click the ellipsis (**...**) > **Modify volume**.
3. Enter the new storage size in GB.
4. Review your selection and the new pricing.
5. Click **Modify**.
6. Your new storage allocation is available in a few minutes.

For more information about expanding the file system (and partitions, if any) on the volume to use the new space, check your OS documentation.
{: tip}

## Resizing storage from the CLI
{: #resizingstepsCLI}
{: cli}

By using the following command, you can increase your volume from the SLCLI.

```
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

For more information about expanding the file system (and partitions, if any) on the volume to use the new space, check your OS documentation.
{: tip}

## Resizing storage with the API
{: #resizingstepsAPI}
{: api}

You can increase your storage capacity by using an API call to the SOAP web service. The following sample API calls can be called from the scripting language of your choice.

For more information about the SLAPI, see the [SLDN](http://sldn.softlayer.com/reference/softlayerapi){: external}.
{: tip}

* Increase capacity on a Performance storage volume.
  ```
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
            <id xsi:type="xsd:int">XXXXXXXX</id> <!--where XXXXXXXXis the VolumeID-->
        </volume>
        <volumeSize xsi:type="xsd:int">2007</volumeSize> <!-- This is the new size we want to upgrade to -->
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


* Increase capacity on an Endurance storage volume.
  ```
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
        <volumeSize xsi:type="xsd:int">25</volumeSize> <!-- the new size -->
        <prices SOAP-ENC:arrayType="ns1:SoftLayer_Product_Item_Price[3]" xsi:type="SOAP-ENC:Array">
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">189433</id> <!-- Top level price -->
            </item>
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">196033</id> <!-- Performance tier price -->
            </item>
            <item xsi:type="ns1:SoftLayer_Product_Item_Price">
                <id xsi:type="xsd:int">196093</id> <!-- Storage space price -->
            </item>
        </prices>
      </orderData>
    </ns1:placeOrder>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
  ```
  {: codeblock}

For more information about expanding the file system (and partitions, if any) on the volume to use the new space, check your OS documentation.
{: tip}


## Expanding Storage over 12 TB
{: #increasecapacityover12TB}
{: help}
{: support}

If you need to increase your Storage volume capacity beyond 12 TB, you can request to be added to the allowlist by submitting a [support case](https://{DomainName}/unifiedsupport/cases/add){: external}. When the request is approved by the Offering Manager, you're going to be notified through the case process. You're also going to see the option to increase your storage up to 16 TB in the console.
{: preview}
