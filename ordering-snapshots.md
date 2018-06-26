---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}

# Ordering Snapshots

To create snapshots of your storage volume, either automated or manually, you need to purchase space to hold them. You can purchase capacity up to your storage volume amount (during the initial volume purchase or later by using the steps that are described here).

1. Access your Storage LUN through **Storage**, **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Click **Add Snapshot Space** in the Snapshots frame.
3. Select the amount of space that you need.
4. Click **Continue**.
5. Enter any **Promo Code** that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
6. Click the **I have read the Master Service Agreement…** check box and click **Place Order**. Your snapshot space is provisioned in a few minutes.

## Determining how much snapshot space to order

Generically speaking, snapshot space is used by snapshots based on two key factors:
- How much your active file system changes over time,
- How long you plan to retain snapshots.  

The way to calculate the amount of space that you need is **(Rate of Change)** x **(number of hours/days/weeks/months data is retained)**.  
**Note**: The first snapshot uses a negligible amount of space as it's just a copy of the metadata (pointers) that indicates the active file system blocks. 

A volume with numerous changes and a lengthy retention period needs more space than a volume with moderate change and a moderate retention schedule. An example for the first type is a high change rate database. An example for the second type is a VMware datastore.

If you take 12 hourly snapshots of 500 GB of actual data, and there's 1 percent of change between each snapshot, you end up with 60 GB for snapshots.

*(5 G Rate of Change) x (12 hourly snapshots) = (60 GB used space)*

Conversely, if that 500 GB of actual data, with 12 hourly snapshots, saw 10 percent of change every hour, the snapshot space that is used is 600 GB.

*(50 GB Rate of Change) x (12 hourly snapshots) = (600 GB used space)*

So when you determine how much Snapshot space you need, consider the rate of change carefully. It's a huge influence on how much snapshot space you need. A bigger volume is more likely to change more often. However, a 500 GB volume with 5 GB of change and a 10 TB volume with 5 GB of change use the same amount of snapshot space.

Additionally, for most workloads, the larger a volume is the less space needs to be set aside initially. It's primarily due to the underlying data efficiencies, and the nature of how snapshots work in the environment.
