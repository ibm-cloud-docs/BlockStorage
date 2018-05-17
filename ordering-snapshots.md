---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordering Snapshots

To create snapshots of your storage volume, either automated or manually, you need to purchase space to hold them. You can purchase capacity up to your storage volume amount (during the initial volume purchase or later using the steps described in this article).

1. Access your Storage LUN through **Storage**, **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Click **Add Snapshot Space** in the Snapshots frame.
3. Select the amount of space you need.
4. Click **Continue**.
5. Enter any **Promo Code** you have and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
6. Click the **I have read the Master Service Agreement…** check box and click **Place Order**. Your snapshot space will be provisioned in a few minutes.

## How to determine how much snapshot space to order

Generically speaking, snapshot space is used by snapshots based on two key pieces of information:
- how much change your active file system has,
- how long you plan to retain snapshots.  

Essentially, the way to calculate the amount of space needed is **(Rate of Change)** x **(number of hours/days/weeks/months data is retained)**.  
**Note**: The very first snapshot uses a negligible amount of space as it's just a copy of the metadata (pointers) indicating the active file system blocks. 

A volume with a lot of data change (like a high change rate database) and a lengthy snapshot retention period is going to need more space for snapshots than a volume with moderate change (like a VM datastore) and a more moderate snapshot retention schedule. 

If you were to take 12 hourly snapshots of a volume with 500 GB of actual data, and saw 1 percent of change between each snapshot, you would end up with 60 GB for snapshots.

*(5 G Rate of Change) x (12 hourly snapshots) = (60 GB space used)*

Conversely, if that 500 GB of actual data, with 12 hourly snapshots, saw 10 percent of change every hour, you would end up with 600 GB.

*(50 GB Rate of Change) x (12 hourly snapshots) = (600 GB space used)*

So when determining how much Snapshot space you need, consider the rate of change carefully. It's a huge influence on how much snapshot space you need. While the size of a volume is likely going to mean a higher amount of change, a 500 GB volume with 5 GB of change and a 10 TB volume with 5 GB of change would result in the same snapshot space usage.

Additionally, for most workloads, the larger a volume is the less space needs to be set aside initially for snapshots. It's primarily due to the underlying data efficiencies of our platform, and the nature of how snapshots work in our environment.



