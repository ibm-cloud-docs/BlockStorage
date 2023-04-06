---

copyright:
  years: 2014, 2023
lastupdated: "2023-04-06"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Replicating Data
{: #replication}

Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event occurs or your data becomes corrupted.
{: shortdesc}

Replication keeps your data in sync in two different locations. If you want to clone your volume and use it independently from the original volume, see [Creating a duplicate Block Volume](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).
{: tip}

Before you can replicate, you must create a snapshot schedule. The option to **Order Replica** does not appear unless this condition is met.
{: important}


## Determining the remote data center for my replicated storage volume in the UI
{: #determinereplocationUI}
{: ui}

{{site.data.keyword.cloud}} data centers are paired into primary and remote combinations in every region worldwide. When you replicate data, consider the local data residency laws because moving data across borders can have legal implications. Replication across regions is not allowed.

See Table 1 for the complete list of data center availability and replication targets within each region.

| US 1 | US 2 | Latin America | Canada  | Europe  | Asia-Pacific  | Australia  |
|-----|-----|-----|-----|-----|-----|-----|
| - DAL05 \n - SJC01 \n - WDC01 | - SJC03 \n - SJC04 \n - WDC04 \n - WDC06 \n - WDC07 \n - DAL09 \n - DAL10 \n - DAL12 \n - DAL13 | - SAO01 \n - SAO04 \n - SAO05 | - TOR01 \n - TOR04 \n - TOR05 \n - MON01 | - AMS03 \n - FRA02 \n - FRA04 \n - FRA05 \n - LON02 \n - LON04 \n - LON05 \n - LON06 \n - PAR01 \n - MIL01 | - TOK02 \n - TOK04 \n - TOK05 \n - OSA21 \n - OSA22 \n - OSA23 \n - SNG01 \n - CHE01 | - SYD01 \n - SYD04 \n - SYD05 \n |
{: caption="Table 1 - This table shows the complete list of data centers with enhanced capabilities in each region. Every region is a separate column. Some cities, such as Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London, and Sydney have multiple data centers." caption-side="top"}

Data centers in US 1 region can replicate with only each other. Data centers in US 2 region cannot start replication with US 1 data centers.
{: note}

As part of the data center modernization strategy for {{site.data.keyword.cloud}}, the data centers in US 1 region are scheduled to consolidate in 2023. For more information, see [Data center consolidations](/docs/get-support?topic=get-support-dc-closure){: external}.
{: note}

## Determining the remote data center for my replicated storage volume from the SLCLI
{: #determinereplocationCLI}
{: cli}

{{site.data.keyword.cloud}} data centers are paired into primary and remote combinations in every region worldwide. When you replicate data, consider the local data residency laws because moving data across borders can have legal implications. Replication across regions is not permitted.

To list suitable replication data centers for a specific volume, use the following command.
```sh
# slcli block replica-locations --help
Usage: slcli block replica-locations [OPTIONS] VOLUME_ID

Options:
--sortby TEXT   Column to sort by
--columns TEXT  Columns to display. Options: ID, Long Name, Short Name
-h, --help      Show this message and exit.
```
{: codeblock}

As part of the data center modernization strategy for {{site.data.keyword.cloud}}, several data centers are scheduled to consolidate in 2023. For more information, see [Data center consolidations](/docs/get-support?topic=get-support-dc-closure){: external}.
{: note}

## Creating the initial replica in the UI
{: #enablerepUI}
{: ui}

Replications work based on a snapshot schedule. You must first have snapshot space and a snapshot schedule for the source volume before you can replicate. If you try to configure replication and one or the other isn't in place, you are going to be prompted to purchase more space or set up a schedule. Replications are managed under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.cloud}} console](/classic){: external}.

1. Click the name of your storage volume to display its details.
2. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") and click **Order Replica**.
3. Select the existing snapshot schedule that you want your replication to follow. The list contains all of your active snapshot schedules.

   You can select only one schedule even if you have a mix of hourly, daily, and weekly. All snapshots that were captured since the previous replication cycle, are replicated regardless of the schedule that originated them.
   For more information, see [Working with Snapshots](/docs/BlockStorage?topic=BlockStorage-snapshots). Replication starts 5 minutes after the snapshot is taken to ensure that the most up-to-date data is copied to the replica volume.
   {: tip}

4. Select a **Location** for the replica volume.
5. Click **Continue**.
6. Enter in a **Promo Code** if you have one, and click **Recalculate**. The other fields in the window are completed by default.

   Discounts are applied when the order is processed.
   {: note}

7. Review your order, and read the service agreement. If you agree with the terms, check the box.
8. Click **Place Order**.

## Creating the initial replica from the SLCLI
{: #enablerepCLI}
{: cli}

Replications work based on a snapshot schedule. You must first have snapshot space and a snapshot schedule for the source volume before you can replicate. Then, you can use the following command to order a replica volume.

```sh
# slcli block replica-order --help
Usage: slcli block replica-order [OPTIONS] VOLUME_ID

Options:
-s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                Snapshot schedule to use for replication,
                                (INTERVAL | HOURLY | DAILY | WEEKLY)
                                [required]
-l, --location TEXT             Short name of the data center for the
                                replicant (e.g.: dal09)  [required]
--tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                primary volume for which a replicant is
                                ordered [optional]
-h, --help                      Show this message and exit.
```

## Viewing the replica volumes in the Volume List in the UI
{: #replicalistUI}
{: ui}

You can view your replication volumes on the {{site.data.keyword.blockstorageshort}} page under **Storage > {{site.data.keyword.blockstorageshort}}**. The **Volume Name** shows the primary volume's name followed by REP. The **Type** is Endurance or Performance – Replica.

## Viewing the replica volumes from the SLCLI
{: #replicalistCLI}
{: cli}

List existing replicant volumes for a file volume with the following command.
```sh
  # slcli block replica-partners --help
  Usage: slcli block replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
```

## Viewing the replication history in the UI
{: #replicationhistoryUI}
{: ui}

To view the Replication history, click Manage on the main menu bar. Select **Account**, and scroll to the Audit Log. The Storage Replication Events list contains the names of the volume, a description of the replication event and the timestamp of the event.

## Editing the Replication Schedule in the UI
{: #editreplicaschedule}
{: ui}

The replication schedule is based on an existing snapshot schedule. To change the replica schedule from Hourly to Daily or Weekly or vice versa, you must cancel the replica volume and set up a new one.

However, if you want to change the time of day when your **Daily** replication occurs, you can adjust the existing schedule on the active volume.

1. On the active volume details page, click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions").
2. Select **Edit Snapshot Schedule**.
3. Look in the **Snapshot** frame under **Schedule** to determine which Daily schedule you're using for replication. Change the schedule that you want.
4. Click **Save**.

## Deleting an existing replica file share in the UI
{: #cancelreplicaUI}
{: ui}

You can cancel replication either immediately or on the anniversary date, which causes billing to end.

1. Click the volume from the **{{site.data.keyword.filestorage_short}}** page.
2. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions").
3. Select **Delete Replica**.
4. Select when to cancel. Choose **Immediately** or **Anniversary Date**, and click **Continue**.
5. This operation deletes the replica volume with all its data. Click the checkbox to acknowledge information, and click **Delete**.

## Canceling replication when the primary volume is deleted in the UI
{: #cancelprimaryUI}
{: ui}

When a primary volume is canceled, the replication schedule and the volume in the replica data center are deleted. Replicas are canceled from the {{site.data.keyword.blockstorageshort}} page.

You can expect the volume to remain visible in your Storage list for at least 24 hours (immediate cancellation) or until the anniversary date. Certain features aren't going to be available any longer, but the volume remains visible until it is reclaimed. However, billing is stopped immediately after you click Delete/Cancel Replica.

Active replicas can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, and replication is canceled before you attempt to cancel the original volume.
{: important}

## Creating a duplicate of a replica
{: #cloneareplica}

You can create a duplicate of an existing {{site.data.keyword.blockstoragefull}}. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot.

Duplicates can be created from both primary and replica volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the new volume is created in the same data center as the replica volume.

Duplicate volumes can be accessed by a host for read/write as soon as the storage is provisioned. However, snapshots and replication aren't allowed until the data copy from the original to the duplicate is complete.

For more information, see [Creating a duplicate {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).

## Using replicas to failover when disaster strikes
{: #replicatotherescureDR}

When you fail over, you’re "flipping the switch" from your storage volume in your primary data center to the destination volume in your remote data center. For example, your primary data center is London and your secondary data center is Amsterdam. If a failure event occurs, you’d fail over to Amsterdam – connecting to the now-primary volume from a compute instance in Amsterdam. After your volume in London is repaired, a snapshot is taken of the Amsterdam volume to fail back to London and the once-again primary volume from a compute instance in London.

* If the primary location is in imminent danger or severely impacted, see [Failover with an accessible primary volume](/docs/BlockStorage?topic=BlockStorage-dr-accessible).
* If the primary location is down, see [Failover with an inaccessible primary volume](/docs/BlockStorage?topic=BlockStorage-dr-inaccessible).
