---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, snapshot space, ordering snapshots,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Ordering Snapshots
{: #orderingsnapshots}

To create snapshots of your storage volume, either automated or manually, you need to purchase space to hold them. You can purchase capacity up to your storage volume amount (during the initial volume purchase or later by using the steps that are described here).

## Determining how much snapshot space to order

Generically speaking, snapshot space is used by snapshots based on two key factors
- How much your active file system changes over time,
- How long you plan to retain snapshots.  

The way to calculate the amount of space that you need is **(Rate of Change)** x **(number of hours/days/weeks/months data is retained)**.

The first snapshot uses a negligible amount of space as it's just a copy of the metadata (pointers) that indicates the active file system blocks.
{:note}

A volume with numerous changes and a lengthy retention period needs more space than a volume with moderate change and a moderate retention schedule. An example for the first type is a high change rate database. An example for the second type is a VMware datastore.

If you take 12 hourly snapshots of 500 GB of actual data, and there's 1 percent of change between each snapshot, you end up with 60 GB for snapshots.

*(5-GB Rate of Change) x (12 hourly snapshots) = (60 GB of used space)*

Conversely, if that 500 GB of actual data, with 12 hourly snapshots, saw 10 percent of change every hour, the snapshot space that is used is 600 GB.

*(50-GB Rate of Change) x (12 hourly snapshots) = (600 GB of used space)*

So when you determine how much Snapshot space you need, consider the rate of change carefully. It's a huge influence on how much snapshot space you need. A bigger volume is more likely to change more often. However, a 500-GB volume with 5 GB of change and a 10-TB volume with 5 GB of change use the same amount of snapshot space.

Additionally, for most workloads, the larger a volume is the less space needs to be set aside initially. It's primarily due to the underlying data efficiencies, and the nature of how snapshots work in the environment.

## Ordering Snapshot space through the {{site.data.keyword.cloud_notm}} console

1. Log in to the [{{site.data.keyword.cloud_notm}} console ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/catalog/){:new_window} and click the menu icon on the upper left. Select **Classic Infrastructure**.

   Alternatively, you can log in to the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
2. Access your Storage LUN through **Storage** >**{{site.data.keyword.blockstorageshort}}**.
2. Click **Change Snapshot Space** in the Snapshots frame.
3. Select the amount of space that you need and the payment method.
4. Click **Continue**.
5. Enter any **Promo Code** that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.

   Discounts are applied when the order is processed.
   {:note}
6. Check the **I have read the Master Service Agreement and agree to the terms therein** box and click **Place Order**. Your snapshot space is provisioned in a few minutes.

## Ordering Snapshot space through the SLCLI

```
# slcli block snapshot-order --help
Usage: slcli block snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the block
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
{codeblock}
