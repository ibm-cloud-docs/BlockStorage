---

copyright:
  years: 2014, 2024
lastupdated: "2024-07-25"

keywords:  Block Storage, block storage, snapshot, snapshot space, snapshot schedule, create snapshot schedule, manual snapshot, view snapshot space, modify snapshot space, SLCLI, API, restore from snapshot

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Managing Snapshots
{: #managingSnapshots}

Snapshots are a feature of {{site.data.keyword.blockstoragefull}}. A snapshot represents a volume's contents at a particular point in time. With snapshots, you can protect your data with no performance impact and minimal consumption of space. Learn more about how to manage snapshots by reading the following instructions.
{: shortdesc}

## Adding a Snapshot schedule in the console
{: #addscheduleUI}
{: ui}

You can decide how often and when you want to create a point in time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage** > **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.cloud}} console](/classic-gen1){: external}.

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

## Adding a Snapshot schedule from the CLI
{: #addscheduleCLI}
{: cli}

You can decide how often and when you want to create a point in time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume. For more information, see [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).
{: important}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Adding a schedule from the IBMCLOUD CLI
{: #addscheduleICCLI}

Use the `ibmcloud sl block snapshot-enable` command to create a snapshot schedule. The following example creates a weekly schedule to take snapshots on every Sunday at 2:00 AM. In this example, up to 5 snapshots are retained.

```sh
$ ibmcloud sl block snapshot-enable 560156918 -s WEEKLY -c 5 -m 0 --hour 2 -d 0
OK
WEEKLY snapshots have been enabled for volume 560156918.
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-enable](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_enable){: external}.

### Adding a schedule from the SLCLI
{: #addscheduleSLCLI}

To create a snapshot schedule, use the following command.

```sh
$ slcli block snapshot-enable --help
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
{: screen}

You can also see the list of your snapshot schedules through the SLCLI with the following command.
```sh
$ slcli block snapshot-schedule-list --help
Usage: slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```
{: screen}

## Managing a Snapshot schedule with Terraform
{: #addscheduleTerraform}
{: terraform}

To create a snapshot schedule, use the `ibm_storage_block` resource and specify information in the `snapshot_schedule` argument. The following example defines two different schedules. One schedule is for weekly snapshots that are taken on Sundays at 1:20 PM. Twenty snapshots are kept before the oldest one is deleted to make space for a new one. The second schedule is for hourly snapshots.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal13"
        capacity = 20
        iops = 0.25
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        snapshot_capacity = 10
        hourly_billing = true

        # Optional fields for snapshot
        snapshot_schedule {
        schedule_type   = "WEEKLY"
        retention_count = 20
        minute          = 20
        hour            = 13
        day_of_week     = "SUNDAY"
        enable          = true
        }
        snapshot_schedule {
        schedule_type   = "HOURLY"
        retention_count = 20
        minute          = 2
        enable          = true
  }
}
```
{: screen}

If you want to update the schedule, change these values and apply them to your resources. If you want to delete the schedule, remove its details from the `ibm_storage_block` resource definition, and apply your changes.

## Taking a manual Snapshot in the console
{: #takemanualsnapshotUI}
{: ui}

Manual snapshots can be taken at various points during an application upgrade or maintenance. You can also take snapshots across multiple servers that were temporarily deactivated at the application level.

The maximum limit of snapshots per storage volume is 50.

1. Click your storage volume.
2. Click **Actions**.
3. Click **Take Manual Snapshot**.

The snapshot is taken and displayed in the **Snapshots** section of the **{{site.data.keyword.blockstorageshort}} Detail** page. Its schedule appears Manual.

## Taking a manual Snapshot from the CLI
{: #takemanualsnapshotCLI}
{: cli}

### Taking a Snapshot from the IBMCLOUD CLI
{: #takemanualsnapshotICCLI}

You can use the `ibmcloud sl block snapshot-create` command to take a snapshot of a specific volume.

```sh
$ ibmcloud sl block snapshot-create 562193766
OK
New snapshot 562208096 was created.
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-create](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_create){: external}.

### Taking a Snapshot from the SLCLI
{: #takemanualsnapshotSLCLI}

You can use the following command to create a snapshot from the CLI.

```sh
$ slcli block snapshot-create --help
Usage: slcli block snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```
{: screen}

## Listing all Snapshots with Space Used Information and Management functions in the console
{: #listsnapshotUI}
{: ui}

A list of retained snapshots and space that is used can be seen on the **{{site.data.keyword.blockstorageshort}} Detail** page. Management functions (editing schedules and adding more space) are conducted on the **{{site.data.keyword.blockstorageshort}} Detail** page by using the **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") menu or links in the various sections on the page. The Snapshot page displays how much capacity the volume has and how much of it is used.

You receive notifications when you reach space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. To remediate this issue, you can manually add space, or delete retained unnecessary snapshots. You can reduce the number of retained snapshots in the schedule. If you reduce the snapshot data or increase the space, the warning system is reset, and no autodeletion occurs.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the snapshot data or increase the space, the warning system is reset and no autodeletion occurs.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, automatic deletion starts so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are manually deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

By default, snapshot warning notifications are enabled for every customer. However, you can choose to disable them. When this feature is disabled, all ticket generation and notifications are stopped. You can disable and enable notifications for the volume at any time from the CLI.

If snapshot space utilization increases too rapidly, then you might receive one notification before autodeletion of the oldest scheduled snapshot occurs. For example, if utilization jumps from 76% to 96% within 15 minutes, you receive one notification about exceeding 75% and one notification about exceeding 95%.
{: note}

## Listing all Snapshots with Space Used Information from the CLI
{: #listsnapshotCLI}
{: cli}

### Listing all snapshots from the IBMCLOUD CLI
{: #listsnapshotICCLI}

You can use the `ibmcloud sl block snapshot-list` command to list the snapshots of a specific volume.

```sh
$ ibmcloud sl block snapshot-list 562193766
id          user_name             created                     size_bytes   notes
562208096   SL02SEVC1414935_651   2023-10-25T12:09:34-05:00   24000        -
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-list](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_list){: external}.

### Listing all snapshots from the SLCLI
{: #listsnapshotSLCLI}

You can accomplish this task from the CLI by using the following command.
```sh
$ slcli block snapshot-list --help
Usage: slcli block snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```

## Check and update notification status from the CLI
{: #notificationstatusCLI}
{: cli}

Notifications are sent when you reach three different space thresholds – 75 percent, 90 percent, and 95 percent.

- At **75 percent capacity**, a warning is sent that snapshot space usage exceeded 75 percent. To remediate this issue, you can manually add space, or delete retained unnecessary snapshots. You can reduce the number of retained snapshots in the schedule. If you reduce the snapshot data or increase the space, the warning system is reset, and no autodeletion occurs.
- At **90 percent capacity**, a second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the snapshot data or increase the space, the warning system is reset and no autodeletion occurs.
- At **95 percent capacity**, a final warning is sent. If no action is taken to bring your space usage under the threshold, automatic deletion starts so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops under 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops under the threshold. If the space is manually increased or snapshots are manually deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

If snapshot space utilization increases too rapidly, then you might receive one notification before autodeletion of the oldest scheduled snapshot occurs. For example, if utilization jumps from 76% to 96% within 15 minutes, you receive one notification about exceeding 75% and one notification about exceeding 95%. The system skips the 90%-exceeded warning.
{: note}

By default, snapshot warning notifications are enabled for every customer. However, you can choose to disable them. When this feature is disabled, all ticket generation and notifications are stopped. You can disable and enable notifications for the volume at any time.

### Checking notification status in the IBMCLOUD CLI
{: #notificationstatusICCLI}

To check the notificatio status, use the `ibmcloud sl block snapshot-get-notification-status` command.

```sh
$ ibmcloud sl block snapshot-get-notification-status 562193766
Enabled: Snapshots space usage threshold is enabled for volume '562193766'.
```
{: screen}

To change the status, use the `snapshot-set-notification` command with the `--disable` option.

```sh
$ ibmcloud sl block snapshot-set-notification --disable  562193766
OK
Snapshots space usage threshold warning notification has been set to 'false' for volume '562193766'.
```
{: screen}

For more information about all of the parameters that are available for these commands, see [ibmcloud sl block snapshot-get-notification-status](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_get_notification_status){: external} and [bmcloud sl block snapshot-set-notification](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_set_notification).{external}

### Checking notification status in the SLCLI
{: #notificationstatusSLCLI}

To check whether the notifications are enabled for the storage volume, use the following command.

```sh
$ slcli block snapshot-get-notification-status
Usage: slcli block snapshot-get-notification-status [OPTIONS] VOLUME_ID
  Get snapshots space usage threshold warning flag setting for a given volume

Options:
  -h, --help  Show this message and exit.
```
{: screen}

To change the status of the notification setting, use the following command.
```sh
$ slcli block snapshot-set-notification VOLUME_ID
Usage: slcli block snapshot-set-notification VOLUME_ID [OPTIONS]

Options:
 --disable  Disable snapshot threshold warning notification for the storage volume
 --enable   Enable snapshot threshold warning notification for the storage volume
 -h, --help  Show this message and exit.
```
{: screen}

## Increasing the amount of Snapshot space for a volume in the console
{: #changesnapshotspaceUI}
{: ui}

You might need to add snapshot space to a volume that didn't previously have any or might require extra snapshot space.

Snapshot space can be increased. It can't be reduced. You can select a smaller amount of space until you determine how much space you need. Remember, automated and manual snapshots share the space.
{: note}

Snapshot space is changed through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click your storage volume, click **Actions**, and click **Change Snapshot Space**.
2. Select from a range of sizes from the prompt. For more information about available snapshot capacity allotments, see [Ordering snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots&interface=ui#snapshotspaceforvolume).
3. Click **Continue**.
4. Enter any Promo Code that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
5. Read the service agreement, and if you agree with the terms click checkbox, and click **Place Order**. Your additional snapshot space is provisioned in a few minutes.

## Deleting a snapshot schedule in the console
{: #cancelnapshotscheduleUI}
{: ui}

Snapshot schedules can be canceled through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click the volume ID to display its related information.
1. Click Snapshots.
1. Click the schedule to be deleted in the **Snapshot Schedules** frame.
1. Click the checkbox next to the schedule to be deleted and click **Save**.

If you're using the replication feature, be sure that the schedule you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication).
{: important}

## Deleting a snapshot schedule from the CLI
{: #cancelnapshotscheduleCLI}
{: cli}

### Deleting a snapshot schedule from the IBMCLOUD CLI
{: #cancelnapshotscheduleICCLI}

You can use the `ibmcloud sl block snapshot-disable` command to delete a snapshot schedule of a specific volume.

```sh
$ ibmcloud sl block snapshot-disable 562193766 -s DAILY
OK
DAILY snapshots have been disabled for volume 562193766.
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-disable](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_disable){: external}.

### Deleting a snapshot schedule from the SLCLI
{: #cancelnapshotscheduleSLCLI}

You can accomplish this task by using the following command.
```sh
$ slcli block snapshot-disable --help
Usage: slcli block snapshot-disable [OPTIONS] VOLUME_ID

  Disables snapshots on the specified schedule for a given volume

Options:
  --schedule-type TEXT  Snapshot schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                        [required]
  -h, --help            Show this message and exit.
```
{: screen}

If you're using the replication feature, be sure that the schedule you're deleting isn't the schedule that is used by replication. For more information about deleting a replication schedule, see [Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication).
{: important}

## Deleting a snapshot in the console
{: #deletesnapshotUI}
{: ui}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. Deletion is done through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click your storage volume and click **Snapshot** to see the list of existing snapshots.
2. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to a particular snapshot and click **Delete**. Click the confirmation box that warns about possible data loss, then click **Delete**. This deletion doesn't affect any future or past snapshots on the same schedule as snapshots don't depend on each other.

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations (oldest first).

## Deleting a snapshot from the CLI
{: #deletesnapshotCLI}
{: cli}

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. 

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations (oldest first).
{: note}

### Deleting a snapshot from the IBMCLOUD CLI
{: #deletesnapshotICCLI}

You can use the `ibmcloud sl block snapshot-delete` command to delete a specific snapshot.

```sh
$ ibmcloud sl block snapshot-delete 562208096
OK
Snapshot 562208096 was deleted.
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-delete](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_delete){: external}.

### Deleting a snapshot from the SLCLI
{: #deletesnapshotSLCLI}

You can delete a snapshot from the CLI by using the following command.

```sh
$ slcli block snapshot-delete
Usage: slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```
{: screen}

Manual snapshots that aren't deleted in the portal manually, are automatically deleted when you reach space limitations. The oldest snapshot is deleted first.
{: note}

## Restoring storage volume to a specific point in time by using a snapshot in the console
{: #restorefromsnapshotUI}
{: ui}

You might need to take your storage volume back to a specific point in time because of user-error or data corruption.

1. Unmount and detach your storage volume from the host to ensure the host is not connecting to the volume during the restore for any reason.
   - [Unmounting {{site.data.keyword.blockstorageshort}} volumes on a Linux&reg; server](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8#unmountingLin)
   - [Unmounting {{site.data.keyword.blockstorageshort}} volumes on a Microsoft server](/docs/BlockStorage?topic=BlockStorage-mountingWindows#unmountingWin)
2. Go to the [{{site.data.keyword.cloud}} console](/login){: external}. From the menu, select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
3. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
3. Scroll on the list, and click your volume to be restored. The **Snapshots** page displays the list of all saved snapshots along with their size and creation date.
4. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the snapshot to be used and click **Restore**.

   Completing the restore results in the loss of the data that was created or modified after the snapshot was taken. This data loss occurs because your storage volume returns to the same state that it was in of the time of the snapshot.
   {: note}

5. Click **Yes** to start the restore. The restoration is going to take a while, and your storage volume is locked during the restoration.

   When you return to the volume list, a clock icon appears next to your volume that indicates that an active transaction is in progress. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete.
   {: note}

6. Mount and reattach your storage volume to the host.
   - [Mount iSCSI LUN on Red Hat Enterprise Linux&reg; 8](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8).
   - [Mount iSCSI LUN on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8).
   - [Mount iSCSI LUN on Ubuntu 20](/docs/BlockStorage?topic=BlockStorage-mountingUbu20).
   - [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows).

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}

## Restoring storage volume to a specific point in time by using a snapshot from the CLI
{: #restorefromsnapshotCLI}
{: cli}

You might need to take your storage volume back to a specific point in time because of user-error or data corruption. 

1. Unmount your volume. You must ensure that the host is not trying to connect to the volume during the restore.
   - [Unmounting {{site.data.keyword.blockstorageshort}} volumes on a Linux&reg; server](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8#unmountingLin)
   - [Unmounting {{site.data.keyword.blockstorageshort}} volumes on Microsoft](/docs/BlockStorage?topic=BlockStorage-mountingWindows#unmountingWin)

2. Then, you can restore the volume with a snapshot by using one of the following commands.
   - From the IBMCLOUD CLI:
     ```sh
     $ ibmcloud sl block snapshot-restore 562193766 562211890
     OK
     Block volume 562193766 is being restored using snapshot 562211890.
     ```
     {: screen}

     For more information about all of the parameters that are available for this command, see [ibmcloud sl block snapshot-restore](/docs/cli?topic=cli-sl-block-storage#sl_block_snapshot_restore){: external}.

   - From the SLCLI:
     ```sh
     $ slcli block snapshot-restore --help
     Usage: slcli block snapshot-restore [OPTIONS] VOLUME_ID
   
     Options:
      -s, --snapshot-id TEXT  The id of the snapshot which is to be used to restore
                              the block volume
      -h, --help              Show this message and exit.
      ```
     {: screen}

3. Lastly, mount and reattach your storage volume to the host.
   - [Mount iSCSI LUN on Red Hat Enterprise Linux&reg; 8](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8).
   - [Mount iSCSI LUN on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8).
   - [Mount iSCSI LUN on Ubuntu 20](/docs/BlockStorage?topic=BlockStorage-mountingUbu20).
   - [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows).

Restoring a volume results in deleting all snapshots that were taken after the snapshot that was used for the restore.
{: important}
