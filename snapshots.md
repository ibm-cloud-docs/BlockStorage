---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, block storage, snapshot, snapshot space, snapshot best practices, snapshot usage,

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Snapshots
{: #snapshots}

Snapshots are a feature of {{site.data.keyword.blockstoragefull}}. A snapshot represents a volume's contents at a particular point in time. With snapshots, you can protect your data with no performance impact and minimal consumption of space. Snapshots are considered your first line of defense for data protection. If a user accidentally modifies or deletes crucial data from a volume, the data can be easily and quickly restored from a snapshot copy.
{: shortdesc}

{{site.data.keyword.blockstorageshort}} provides you with two ways to take your snapshots.

* First, through a configurable snapshot schedule that creates and deletes snapshot copies automatically for each storage volume. You can also create extra snapshot schedules, manually delete copies, and manage schedules based on your requirements.
* The second way is to take a manual snapshot.

A snapshot copy is a read-only image of a {{site.data.keyword.blockstorageshort}} LUN that captures the state of the volume at a point in time. Snapshot copies are efficient both in the time that is needed to create them and in storage space. A {{site.data.keyword.blockstorageshort}} snapshot copy takes only a few seconds to create. It's typically less than 1 second, regardless of the size of the volume or the level of activity on the storage. After a snapshot copy is created, changes to data objects are reflected in updates to the current version of the objects, as if Snapshot copies didn't exist. Meanwhile, the copy of the data remains stable.

A Snapshot copy incurs no performance decrease. Users can easily store up to 50 scheduled snapshots and 50 manual snapshots per {{site.data.keyword.blockstorageshort}} volume, all of which are accessible as read-only and online versions of the data.

With snapshots, you can:

- Non-disruptively create point-in-time recovery points,
- Revert volumes to previous points-in-time.

You must purchase some amount of snapshot space for your volume first so you can take snapshots of it. The snapshot space can be added during the initial order or afterward through the **Volume Details** page. Scheduled and manual snapshots share the snapshot space, so make sure you order enough Snapshot space. For more information, see [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots).

## Snapshot best practices
{: #snapshotbestpractices}

Snapshot design depends on the customer’s environment. The following design considerations can help you to plan and implement Snapshot copies:
- Up to 50 snapshots can be created through a schedule and up to 50 manually on each volume or LUN.
- Don't over snap. Make sure that your scheduled snapshot frequency meets your RTO and RPO needs and your application business requirements by scheduling hourly, daily, or weekly snapshots.
- Snapshot AutoDelete can be used to control the growth of storage consumption.

   The AutoDelete threshold is fixed at 95 percent.
   {: note}

Snapshots are not replacements for actual off-site Disaster Recovery replication or long-retention backup.
{: important}

## Security
{: #snapshotsecurity}

All snapshots and replicas of encrypted {{site.data.keyword.blockstorageshort}} are also encrypted by default. This feature can't be turned off on a volume basis. For more information about provider-managed encryption-at-rest, see [Securing your data](/docs/BlockStorage?topic=BlockStorage-mng-data).

## How Snapshots affect the disk space
{: #snapshotspaceuse}

Snapshot copies minimize disk consumption by preserving individual blocks rather than whole files. Snapshot copies use extra space only when files in the active file system are changed or deleted.

In the active file system, the changed blocks are rewritten to different locations on the disk or removed as active file blocks entirely. When files are changed or deleted, the original file blocks are preserved as part of one or more Snapshot copies. As a result, disk space that is used by the original blocks is still reserved to reflect the status of the active file system before the change. This space is reserved in addition to the disk space that is used by blocks in the modified active file system.


| Disc space usage |   |
|-----|-----|
| ![The space that is used before a snapshot copy is taken.](/images/bfcircle1.png "Before Snapshot Copy") | Before any Snapshot copy is created, disk space is used by the active file system only. |
| ![The space that is used when a snapshot copy is taken.](/images/bfcircle3.png "After Snapshot Copy") | After a Snapshot copy is created, the active file system and Snapshot copy point to the same disk blocks. The Snapshot copy doesn't use extra disk space.  |
| ![The space that is used when something changes after a snapshot copy was taken.](/images/bfcircle2.png "Changes after Snapshot Copy") | After `myfile.txt` is deleted from the active file system, the Snapshot copy still includes the file, and references its disk blocks. That's why deleting active file system data doesn't always free up disk space. |
{: caption="Table 1 shows how snapshots affect the space usage on the Storage." caption-side="top"}

For more information about snapshot space usage, see [Managing Snapshots](/docs/BlockStorage?topic=BlockStorage-managingSnapshots).
