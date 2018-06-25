---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Managing Snapshots

## Creating a Snapshot schedule

You decide how often and when you want to create a point-in-time reference of your storage volume with Snapshot schedules. You can have a maximum of 50 snapshots per storage volume. Schedules are managed through the **Storage**, **{{site.data.keyword.blockstorageshort}}** tab of the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Before you can set up your initial schedule, you must first purchase snapshot space if you didn't purchase it during the initial provisioning of the storage volume.

### Adding a Snapshot schedule

Snapshots schedules can be set up for hourly, daily, and weekly intervals, each with a distinct retention cycle. There is a maximum of 50 scheduled snapshots, which can be a mix of hourly, daily, and weekly schedules, and manual snapshots per storage volume.

1. Click your storage volume, click **Actions**, and click **Schedule Snapshot**.
2. In the New Schedule Snapshot window, there are three different snapshot frequencies to select from. Use any combination of the three to create a comprehensive snapshot schedule.
   - Hourly
      - Specify the minute each hour that a snapshot is to be taken. The default is the current minute.
      - Specify the number of hourly snapshots to be retained before the oldest is discarded.
   - Daily
      - Specify the hour and minute that a snapshot is to be taken. The default is the current hour and minute.
      - Select the number of hourly snapshots to be retained before the oldest is discarded.
   - Weekly
      - Specify the day of the week, hour, and minute that a snapshot is to be taken. The default is the current day, hour, and minute.
      - Select the number of weekly snapshots to be retained before the oldest is discarded.
3. Click **Save**, and create another schedule with a different frequency. If the total number of scheduled snapshots is over 50, you receive a warning message and won't be able to save.

The list of the snapshots are displayed as they're taken in the **Snapshots** section of the **Detail** page.

## Taking a manual Snapshot

Manual snapshots can be taken at various points during an application upgrade or maintenance. You can also take snapshots across multiple servers that were temporarily deactivated at the application level.

There's a maximum of 50 manual snapshots per storage volume.

1. Click your storage volume.
2. Click **Actions**.
3. Click **Take Manual Snapshot**.
The snapshot is taken and displayed in the **Snapshots** section of the **Detail** page. Its schedule appears Manual.

## Listing all Snapshots with Space Used Information and Management functions

A list of retained snapshots and space that is used can be seen on the **Detail** page (Storage, {{site.data.keyword.blockstorageshort}}). Management functions (editing schedules and adding more space) are conducted on the Detail page by using the **Actions** menu or links in the various sections on the page.

## Viewing the list of retained Snapshots

Retained snapshots are based on the number you entered in the **Keep the last** field when you set up your schedules. You can view the snapshots that were taken under the **Snapshot** section. Snapshots are listed by schedule.

## Viewing the amount of Snapshot space that is used

The pie chart at the top of the **Details** page displays how much space is used and how much space is left. You receive notifications when you reach space thresholds – 75 percent, 90 percent, and 95 percent.

## Changing the amount of Snapshot space for a volume

You might need to add snapshot space to a volume that didn't previously have any or might require extra snapshot space. You can add 5 - 4,000 GB depending on your needs. 

**Note**: Snapshot space can be increased only. It can't be reduced. You can select a smaller amount of space until you determine how much space you actually need. Remember, automated, and manual snapshots share the space.

Snapshot space is changed through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click your storage volumes, click **Actions**, and click **Add More Snapshot Space**.
2. Select from a range of sizes from the prompt. Sizes typically range from 0 to the size of your volume.
3. Click **Continue**.
4. Enter any Promo Code that you have, and click **Recalculate**. The Charges for this order and Order Review fields are completed by default.
5. Click the **I have read the Master Service Agreement…** check box and click **Place Order**. Your additional snapshot space is provisioned in a few minutes.

## Receiving notifications when snapshot space limit is reached and snapshots are deleted

Notifications are sent through the support tickets to the Master User on your account when you reach three different space thresholds – 75 percent, 90 percent, and 95 percent.

- **75 percent capacity**: A warning is sent that snapshot space usage exceeded 75 percent. If you heed the warning and manually add space or delete retained and unnecessary snapshots, the action is noted and the ticket is closed. If you do nothing, you must manually acknowledge the ticket and then it is closed.
- **90 percent capacity**: A second warning is sent when snapshot space usage exceeded 90 percent. Like with reaching 75 percent capacity, if you take the necessary actions to decrease the space that is used, the action is noted and the ticket is closed. If you do nothing, you must manually acknowledge the ticket and then it is closed.
- **95 percent capacity**: A final warning is sent. If no action is taken to bring your space usage below the threshold, a notification is generated and automatic deletion occurs so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops below 95 percent. Snapshots continue to be deleted each time usage exceeds 95 percent until it drops below the threshold. If the space is manually increased or snapshots are deleted, the warning is reset and reissued if the threshold is exceeded again. If no actions are taken, this notification is the only warning that you receive.

## Deleting a snapshot schedule

Snapshot schedules can be canceled through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click the schedule to be deleted in the **Snapshot Schedules** frame on the **Details** page.
2. Click the check box next to the schedule to be deleted and click **Save**.<br />
**Caution**: If you're using the replication feature, be sure that the schedule you're deleting isn't the schedule that is used by replication. Click [here](replication.html) for more information on deleting a replication schedule.

## Deleting a snapshot

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. Deletion is done through **Storage** > **{{site.data.keyword.blockstorageshort}}**.

1. Click your storage volume and scroll to the **Snapshot** section to see the list of existing snapshots.
2. Click **Actions** next to a particular snapshot and click **Delete** to delete the snapshot. This deletion won't affect any future or past snapshots on the same schedule as there's no dependency between snapshots.

Manual snapshots that aren't deleted in the way as described previously, are automatically deleted when you reach space limitations (oldest first).

## Restoring storage volume to a specific point-in-time by using a snapshot

You might need to take your storage volume back to a specific point-in-time because of user-error or data corruption.

1. Unmount and detach your storage volume from the host.
   - Click [here](accessing_block_storage_linux.html) for {{site.data.keyword.blockstorageshort}} on Linux instructions.
   - Click [here](accessing-block-storage-windows.html) for {{site.data.keyword.blockstorageshort}} on Microsoft Windows instructions.
2. Click **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Scroll down and click your volume to be restored. The **Snapshots** section of the **Detail** page displays the list of all saved snapshots along with their size and creation date.
4. Click **Actions** next to the snapshot to be used and click **Restore**. <br/>
  **Note**: Completing the restore results in the loss of the data that was created or modified after the snapshot was taken. This data loss occurs because your storage volume returns to the same state it was in of the time of the snapshot. 
5. Click **Yes** to start the restore. Expect a message across the top of the page that states that the volume is being restored by using the selected snapshot. Additionally, an icon appears next to your volume on the {{site.data.keyword.blockstorageshort}} that indicates that an active transaction is in progress. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete.
6. Mount and reattach your storage volume to the host.
   - Click [here](accessing_block_storage_linux.html) for {{site.data.keyword.blockstorageshort}} on Linux instructions.
   - Click [here](accessing-block-storage-windows.html) for {{site.data.keyword.blockstorageshort}}on Microsoft Windows instructions.
   
**Note**: Restoring a volume results in deleting all snapshots that were taken before the restored snapshot.
