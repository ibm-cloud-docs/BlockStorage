---

copyright:
  years: 2014, 2017
lastupdated: "2017-09-27"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Working with Snapshots

Snapshots are a feature of {{site.data.keyword.blockstoragefull}}. A snapshot represents a volume's contents at a particular point in time. Snapshots enable you to protect your data with no performance impact, minimal consumption of space, and are considered your first line of defense for data protection. Data can be easily and quickly restored from a snapshot copy if a user accidently modifies or deletes crucial data from a volume with the snapshot feature.

{{site.data.keyword.blockstorageshort}} and File Storage provide you with two ways to take your snapshots – through a configurable snapshot schedule that creates and deletes snapshot copies automatically for each storage volume. You can also create additional snapshot schedules, manually delete copies, and manage schedules based on your requirements. The second way is to take a manual snapshot.

Snapshots let users

- Non-disruptively create point-in-time recovery points
- Revert volumes to previous points-in-time
- You must purchase some amount of snapshot space for your volume in order to take snapshots of it. The snapshot space can be added during initial volume ordering or after initial provisioning via volume Details page and clicking the Actions drop-down button, and select Add Snapshot Space. Be aware that scheduled and manual snapshots share the snapshot space, so order your space accordingly.

**Note**: Unless otherwise noted, the steps are the same for both {{site.data.keyword.blockstorageshort}} and File Storage.

## How Do I Purchase Snapshot Space?

In order to create snapshots of your storage volume, either automated or manually, you need to purchase space to hold them. You can purchase capacity up to your storage volume amount (during the initial volume purchase or later using the below steps).

1. Click the **Purchase snapshot space now** link in the Snapshots frame.
2. Select the amount of space you need by clicking the radio button next to the appropriate amount.
3. Click **Continue**.
4. Enter any Promo Code you have and click **Recalculate**. The Charges for this order and Order Review will have default values.
5. Click the **I have read the Master Service Agreement…** checkbox and click **Place Order**. Your snapshot space will be provisioned in a few minutes.

## How Do I Create a Snapshot Schedule?

Snapshot schedules let you decide how often and when you want to create a point-in-time reference of your storage volume. You can have a maximum of 50 snapshots per storage volume. Schedules are managed via the **Storage**, **{{site.data.keyword.blockstorageshort}}** tab of the [Customer Portal](https://control.softlayer.com/){:new_window}.

Before you can set up your initial schedule, you must first purchase snapshot space if you did not purchase it during the initial provisioning of the storage volume.

### How Do I Add a Snapshot Schedule

Snapshots schedules can be set up for hourly, daily, and weekly intervals, each with a distinct retention cycle. There is a maximum of 50 scheduled snapshots, which can be a mix of hourly, daily, and weekly schedules, and manual snapshots per storage volume.

1. Click on your storage volume, click the **Actions** drop-down box, and click **Schedule Snapshot**.
2. In the New Schedule Snapshot dialog there are three different snapshot frequencies to select from. Use any combination of the three to create a comprehensive snapshot schedule.
   - Hourly
      - Specify the minute each hour that a snapshot should be performed; default is the current minute.
      - Specify the number of hourly snapshots to be retained before discarding the oldest.
   - Daily
      - Specify the hour and minute that a snapshot should be performed; default is the current hour and minute.
      - Select the number of hourly snapshots to be retained before discarding the oldest.
   - Weekly
      - Specify the day of the week, hour, and minute that a snapshot should be performed; default is the current day, hour, and minute.
      - Select the number of weekly snapshots to be retained before discarding the oldest.
3. Click **Save** and create another schedule with a different frequency. Note that you will receive a warning message and will not be able to save if the total number of scheduled snapshots is over 50.

A list of the snapshots will display as they’re taken in the Snapshots section of the Detail page.

## How Do I Take a Manual Snapshot?

Manual snapshots can be taken at various points during an application upgrade or maintenance. They also let you take snapshots across multiple machines that have been temporarily inactivated at the application level.

There is a maximum of 50 manual snapshots per storage volume.

1. Click on your storage volume.
2. Click the Actions drop-down box.
3. Click **Take Manual Snapshot**.
The snapshot will be taken and will display in the Snapshots section of the Detail page. Its schedule will be Manual.

## How Do I See a List of Snapshots with Space Consumed and Management Functions?

A list of retained snapshots and space consumed can be seen on the **Detail** page (Storage, {{site.data.keyword.blockstorageshort}}). Management functions (editing schedules and adding more space) are conducted on the Detail page using the **Actions** drop-down menu or links in the various sections on the page.

## How Do I View a List of Retained Snapshots?

Retained snapshots are based on the number you entered in the Keep the last field when setting up your schedules. You can view the snapshots that have been taken under the Snapshot section. Snapshots are listed by schedule.

## How Do I See How Much Snapshot Space Has Been Used?

The pie chart at the top of the Details page displays how much space has been used and how much space is left. You’ll receive notifications when you begin to reach space thresholds – 75%, 90%, and 95%.

## How Do I Change the Amount of Snapshot Space for My Volume?

You may need to add snapshot space to a volume that did not previously have any or may require additional snapshot space. You can add between 5 GB and 4,000 GB depending on your needs. 

**Note**: Snapshot space can only be increased and not reduced. You may want to select a smaller amount of space until you determine how much space you actually need. Remember, automated and manual snapshots share the same space.

Snapshot space is changed via **Storage, {{site.data.keyword.blockstorageshort}}**.

1. Click on your storage volumes, click the **Actions** drop-down box, and click **Add More Snapshot Space**.
2. Select from a range of sizes from the prompt. Sizes typically range from 0 to the size of your volume.
3. Click **Continue** to provision the additional space.
4. Enter any Promo Code you have and click **Recalculate**. The Charges for this order and Order Review will have default values.
5. Click the **I have read the Master Service Agreement…** checkbox and click **Place Order**. Your additional snapshot space will be provisioned in a few minutes.

## How Do I Receive Notifications When I'm Close to My Snapshot Space Limit and Snapshots Are Deleted?

Notifications are sent via support tickets from Support to the Master User on your account when you reach three different space thresholds – 75%, 90%, and 95%.

- **75% capacity**: A warning is sent that snapshot space utilization has exceeded 75%. If you heed the warning and manually add space or delete retained and unnecessary snapshots, the action is noted and the ticket’s closed. If you do nothing, you must manually acknowledge the ticket and then it’s closed.
- **90% capacity**: A second warning is sent when snapshot space utilization has exceeded 90%. Like with reaching 75% capacity, if you take the necessary actions to decrease the space used, the action is noted and the ticket’s closed. If you do nothing, you must manually acknowledge the ticket and then it’s closed.
- **95% capacity**: A final warning is sent. If no action is taken to bring your space below the threshold, a notification is generated and automatic deletion occurs so that future snapshots can be created. Scheduled snapshots are deleted, starting with the oldest, until usage drops below 95%, and will continue to be deleted each time utilization exceeds 95% until it drops below the threshold. If the space is manually increased or snapshots deleted, the warning will be reset and re-issued if the threshold is exceeded again. If no actions are taken, this will be the only warning that will be received.

## How Do I Delete a Snapshot Schedule?

Snapshot schedules can be cancelled via **Storage, {{site.data.keyword.blockstorageshort}}**.

1. Click on the schedule to be deleted in the **Snapshot Schedules** frame on the **Details** page.
2. Click in the checkbox next to the schedule to be deleted and click **Save**.
Caution: If you’re using the replication feature, be sure that the schedule you’re deleting is not the schedule that is used by replication. Click [here](replication.html) for more information on deleting a replication schedule.

## How do I Delete a Snapshot?

Snapshots that are no longer needed can be manually removed to free up space for future snapshots. Deletion is done via **Storage, {{site.data.keyword.blockstorageshort}}**.

1. Click on your storage volume and scroll down to the Snapshot section to see a list of existing snapshots.
2. Click the **Actions** drop-down list next to a particular snapshot and click **Delete** to delete the snapshot. This will not affect any future or past snapshots on the same schedule as there is no dependency between snapshots.

Manual snapshots that are not deleted in the way described above, are automatically deleted (oldest first) when you reach space limitations.

## How Do I Restore My Storage Volume to a Specific Point in Time Using a Snapshot?

You may need to take your storage volume back to a specific point in time because of user error or data corruption.

1. Unmount and detach your storage volume from the host.
   - Click [here](accessing_block_storage_linux.html)  for b{{site.data.keyword.blockstorageshort}} on Linux instructions.
   - Click [here](accessing-block-storage-windows.html) for {{site.data.keyword.blockstorageshort}} on Microsoft Windows instructions.
2. Click **Storage**, **{{site.data.keyword.blockstorageshort}}** in the customer portal.
3. Scroll down and click on your volume to be restored. The Snapshots section of the Detail page will display a list of all saved snapshots along with their size and creation date.
4. Click the **Actions** button for the snapshot to be used and click Restore. 
  **Note**: Performing a restore will result in a loss of data that has been created or modified since the snapshot being used was taken. Once complete, your storage volume will be returned to the same state it was in when the snapshot was taken. A prompt will appear to notify you of this.
5. Click **Yes** to initiate the restore. You will receive a message across the top of the page stating the volume was restored using the selected snapshot. Additionally, an icon will appear next to your volume on the {{site.data.keyword.blockstorageshort}} indicating that an active transaction is in progress. Hovering over the icon produces a dialog indicating the transaction. The icon will disappear once the transaction is complete.
6. Mount and re-attach your storage volume to the host.
   - Click [here](accessing_block_storage_linux.html) for {{site.data.keyword.blockstorageshort}} on Linux instructions.
   - Click [here](accessing-block-storage-windows.html) for {{site.data.keyword.blockstorageshort}}on Microsoft Windows instructions.
