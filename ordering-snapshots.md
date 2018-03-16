---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ordering Snapshots

In order to create snapshots of your storage volume, either automated or manually, you need to purchase space to hold them. You can purchase capacity up to your storage volume amount (during the initial volume purchase or later using the steps below).

1. Access your Storage LUN via **Storage**, **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Click the **Add Snapshot Space** link in the Snapshots frame.
3. Select the amount of space you need by clicking the radio button next to the appropriate amount.
4. Click **Continue**.
5. Enter any Promo Code you have and click **Recalculate**. The Charges for this order and Order Review will have default values.
6. Click the **I have read the Master Service Agreement…** checkbox and click **Place Order**. Your snapshot space will be provisioned in a few minutes.

## How to Determine How Much Spanshot Space to Order

Unfortunately, there isn't a best practice so much as a best recommendation based on application. Generically speaking, snapshot space is consumed by snapshots based on two key pieces of information:
- how much change your active file system has, and 
- how long you plan to retain snapshots.  

Essentially, the way to calculate the amount of space needed is **(Rate of Change)** x **(number of hours/days/weeks/months retained)**.  
**Note**: The very first snapshot consumes a negligible amount of space as it is just a copy of the metadata (pointers) indicating the active file system blocks. 

A volume with a lot of data change (for example, a high change rate database) and a lengthy snapshot retention period is going to need more space for snapshots than a volume with moderate change (for example, a VM datastore) and a more moderate snapshot retention schedule. 

In the instance of a volume that is 500 GB of actual data, if you were to take 12 hourly snapshots, and saw 1% of change between each snapshot you would end up with (5 G Rate of Change) x (12 hourly snapshots) = 60 GB for snapshots.

Conversely, if that 500 GB of actual data, with 12 hourly snapshots, saw 10% of change every hour, you would end up with (50 GB Rate of Change) x (12 hourly snapshots) = 600 GB.

So when determining how much Snapshot space you need, consider the rate of change carefully. It is a huge influence on how much snapshot space you need.  While the size of a volume is likely going to mean a higher amount of change, a 500 G volume with 5 G of change and a 10 TB volume with 5 G of change would result in the same snapshot space usage.

Additionally, for most workloads, the larger a volume is the less space needs to be set aside initially for snapshots.  This is primarily due to the underlying data efficiencies of our platform, as well as due to the nature of how snapshots work in our environment.



