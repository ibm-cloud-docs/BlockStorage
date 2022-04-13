---

copyright:
  years: 2014, 2022
lastupdated: "2022-04-13"

keywords:  Block Storage, block storage, snapshot, snapshot space, snapshot schedule, create snapshot schedule, manual snapshot, view snapshot space, modify snapshot space, SLCLI, API, restore from snapshot

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Managing Snapshots
{: #managingSnapshots}

Snapshots are a feature of {{site.data.keyword.blockstoragefull}}. A snapshot represents a volume's contents at a particular point in time. With snapshots, you can protect your data with no performance impact and minimal consumption of space. Learn more about how to manage snapshots by reading the following instructions.
{: shortdesc}

## Adding a Snapshot schedule in the UI
{: #addscheduleUI}
{: ui}

You decide how often and when you want to create a point-in-time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage** > **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume. For more information, see [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).
{: important}

Snapshots schedules can be set up for hourly, daily, and weekly intervals, each with a distinct retention cycle. The maximum limit of snapshots is 50 per storage volume, which can be a mix of hourly, daily, and weekly schedules, and manual snapshots.

1. Click your storage volume, click **Actions**, and click **Edit Snapshot Schedule**.
2. In the Snapshot Schedule window, you can select from three different snapshot frequencies. Use any combination of the three to create a comprehensive snapshot schedule.
   - Hourly
      - Specify the minute each hour that a snapshot is to be taken. The default is the current minute.
      - Specify the number of hourly snapshots to be retained before the oldest is discarded.
   - Daily
      - Specify the hour and minute that a snapshot is to be taken. The default is the current hour and minute.
      - Select the number of hourly snapshots to be retained before the oldest is discarded.
   - Weekly
      - Specify the day of the week, hour, and minute that a snapshot is to be taken. The default is the current day, hour, and minute.
      - Select the number of weekly snapshots to be retained before the oldest is discarded.
3. Click **Save**. Then, you can create another schedule with a different frequency. If the total number of scheduled snapshots is over 50, you receive a warning message and you are not going to be able to save another snapshot.

The list of the snapshots is displayed as they're taken in the **Snapshots** section of the **{{site.data.keyword.blockstorageshort}} Detail** page.

## Adding a Snapshot schedule from the SLCLI
{: #addscheduleCLI}
{: cli}

You decide how often and when you want to create a point-in-time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage** > **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume. For more information, see [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).
{: important}

To set up a snapshot schedule, use the following command

```python
# slcli block snapshot-enable --help
Usage: slcli block snapshot-enable [OPTIONS] VOLUME_ID

  Enables snapshots for a given volume on the specified schedule

Options:
  --schedule-type TEXT    Snapshot schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                          [required]
  --retention-count TEXT  Number of snapshots to retain  [required]
  --minute INTEGER        Minute of the day when snapshots should be taken
  --hour INTEGER          Hour of the day when snapshots should be taken
  --day-of-week TEXT      Day of the week when snapshots should be taken
  -h, --help              Show this message and exit.

```

You can also see the list of your snapshot schedules through the SLCLI with the following command.
```python
# slcli block snapshot-schedule-list --help
Usage: slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```
{: codeblock}

## Taking a manual Snapshot in the UI
{: #takemanualsnapshotUI}
{: ui}

Manual snapshots can be taken at various points during an application upgrade or maintenance. You can also take snapshots across multiple servers that were temporarily deactivated at the application level.

The maximum limit of snapshots per storage volume is 50.

1. Click your storage volume.
2. Click **Actions**.
3. Click **Take Manual Snapshot**.

The snapshot is taken and displayed in the **Snapshots** section of the **{{site.data.keyword.blockstorageshort}} Detail** page. Its schedule appears Manual.

## Taking a manual Snapshot from the SLCLI
{: #takemanualsnapshotCLI}
{: cli}

You can use the following command to create a snapshot from the SLCLI.

```python
# slcli block snapshot-create --help
Usage: slcli block snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```
{: codeblock}

## Listing all Snapshots with Space Used Information and Management functions in the UI
{: #listsnapshotUI}
{: ui}

A list of retained snapshots and space that is used can be seen on the **{{site.data.keyword.blockstorageshort}} Detail** page.  Management functions (editing schedules and adding more space) are conducted on the **{{site.data.keyword.blockstorageshort}} Detail** page by using the **Actions** menu or links in the various sections on the page. The Snapshot page displays how much capacity the volume has and how much of it is used. 

You receive notifications when you reach space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. If you heed the warning and manually add space, or delete retained and unnecessary snapshots, or reduce the number of retained snapshots in the schedule, the system is reset and no auto-deletion occurs.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the snapshot data or increase the space, the warning system is reset and no auto-deletion occurs.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, automatic deletion starts so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are manually deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

By default, snapshot warning notifications are enabled for every customer. However, you can choose to disable them. When this feature is disabled, all ticket generation and notifications are stopped. You can disable and enable notifications for the volume at any time from the CLI.

If snapshot space utilization increases too rapidly, then you might receive one notification before auto-deletion of the oldest scheduled snapshot occurs. For example, if utilization jumps from 76% to 96% within 15 minutes, you receive one notification about exceeding 75% and one notification about exceeding 95%.
{: note}

## Listing all Snapshots with Space Used Information and Management functions from the SLCLI
{: #listsnapshotCLI}
{: cli}

You can accomplish this task from the SLCLI by using the following command.
```python
# slcli block snapshot-list --help
Usage: slcli block snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```

Notifications are sent when you reach three different space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. If you heed the warning and manually add space, or delete retained and unnecessary snapshots, or reduce the number of retained snapshots in the schedule, the system is reset and no auto-deletion occurs.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the snapshot data or increase the space, the warning system is reset and no auto-deletion occurs.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, automatic deletion starts so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are manually deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

If snapshot space utilization increases too rapidly, then you might receive one notification before auto-deletion of the oldest scheduled snapshot occurs. For example, if utilization jumps from 76% to 96% within 15 minutes, you receive one notification about exceeding 75% and one notification about exceeding 95%. The systems skips the 90%-exceeded warning.
{: note}

By default, snapshot warning notifications are enabled for every customer. However, you can choose to disable them. When this feature is disabled, all ticket generation and notifications are stopped. You can disable and enable notifications for the volume at any time.

To check if the notifications are enabled for the storage volume, use the following command.

```python
# slcli block snapshot-get-notification-status
Usage: slcli block snapshot-get-notification-status [OPTIONS] VOLUME_ID
  Get snapshots space usage threshold warning flag setting for a given volume

Options:
  -h, --help  Show this message and exit.
```

To change the status of the notification setting, you the following command.
```python
# slcli block snapshot-set-notification VOLUME_ID 
Usage: slcli block snapshot-set-notification VOLUME_ID [OPTIONS]

Options:
 --disable  Disable snapshot threshold warning notification for the storage volume
 --enable   Enable snapshot threshold warning notification for the storage volume
 -h, --help  Show this message and exit. 
```

## Increasing the amount of Snapshot space for a volume in the UI
{: #changesnapshotspaceUI}
{: ui}

You might need to add snapshot space to a volume that didn't previously have any or might require extra snapshot space. You can add 5 - 4,000 GB depending on your needs.

Snapshot space can be increased. It can't be reduced. You can select a smaller amount of space until you determine how much space you need. Remember, automated, and manual snapshots share the space.
{: note}

Snapshot space is changed through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click your storage volume, click **Actions**, and click **Add Snapshot Space**/**Change Snapshot Space**.
2. Select from a range of sizes from the prompt. Sizes typically range from 0 to the size of your volume.
3. Click **Continue**.
4. Enter any Promo Code that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
5. Click the **I have read the…** check box and click **Place Order**. Your additional snapshot space is provisioned in a few minutes.

## Deleting a snapshot schedule in the UI
{: #cancelnapshotscheduleUI}
{: ui}

Snapshot schedules can be canceled through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click the schedule to be deleted in the **Snapshot Schedules** section on the **{{site.data.keyword.blockstorageshort}} Detail** page.
2. Click the check box next to the schedule to be deleted so it becomes empty. Then, click **Save**. 

If you're using the replication feature, be sure that the schedule you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication).
{: important}

## Deleting a snapshot schedule from the SLCLI
{: #cancelnapshotscheduleCLI}
{: cli}

You can accomplish this task by using the following command.
```python
# slcli block snapshot-disable --help
Usage: slcli block snapshot-disable [OPTIONS] VOLUME_ID

  Disables snapshots on the specified schedule for a given volume

Options:
  --schedule-type TEXT  Snapshot schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                        [required]
  -h, --help            Show this message and exit.
```

If you're using the replication feature, be sure that the schedule you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication).
{: important}

## Deleting a snapshot in the UI
{: #deletesnapshotUI}
{: ui}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. Deletion is done through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click your storage volume and click **Snapshot** to see the list of existing snapshots.
2. Click **Actions** next to a particular snapshot and click **Delete**. Click the confirmation box that warns about possible data loss, then click **Delete**. This deletion doesn't affect any future or past snapshots on the same schedule as there's no dependency between snapshots.

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations (oldest first).

## Deleting a snapshot from the SLCLI
{: #deletesnapshotCLI}
{: cli}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. You can delete a snapshot from the SLCLI by using the following command.
```python
# slcli block snapshot-delete
Usage: slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```
{: codeblock}

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations. The oldest snapshot is deleted first.
{: note}

## Restoring storage volume to a specific point-in-time by using a snapshot in the UI
{: #restorefromsnapshotUI}
{: ui}

You might need to take your storage volume back to a specific point-in-time because of user-error or data corruption.

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}

1. Unmount and detach your storage volume from the host.
   - [Connecting iSCSI Volumes on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#unmountingLin)
   - [Connecting iSCSI Volumes on Microsoft&reg; Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows#unmountingWin)
2. Click **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}.
3. Scroll down and click your volume to be restored. The **Snapshots** section of the **{{site.data.keyword.blockstorageshort}} Detail** page displays the list of all saved snapshots along with their size and creation date.
4. Click **Actions** next to the snapshot to be used and click **Restore**.

   Completing the restore results in the loss of the data that was created or modified after the snapshot was taken. This data loss occurs because your storage volume returns to the same state it was in of the time of the snapshot.
   {: note}
   
5. Click **Yes** to start the restore.

   Expect a message across the page that states that the volume is being restored by using the selected snapshot. Additionally, an icon appears next to your volume on the {{site.data.keyword.blockstorageshort}} that indicates that an active transaction is in progress. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete.
   {: note}

6. Mount and reattach your storage volume to the host.
   - [Connecting iSCSI Volumes on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Connecting iSCSI Volumes on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Connecting iSCSI Volumes on Microsoft&reg; Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows)

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}

## Restoring storage volume to a specific point-in-time by using a snapshot from the SLCLI
{: #restorefromsnapshotCLI}
{: cli}

You might need to take your storage volume back to a specific point-in-time because of user-error or data corruption. First, unmount your volume.

Then, you can restore the volume with a snapshot from the SLCLI by using the following command.
```python
# slcli block snapshot-restore --help
Usage: slcli block snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which is to be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```
{: codeblock}  

After the restore is complete, mount and reattach your storage volume to the host.

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}
