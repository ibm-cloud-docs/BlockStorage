---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Creating a duplicate Block Volume

You can create a duplicate of an existing {{site.data.keyword.blockstoragefull}}. The duplicate volume inherits the capacity and performance options of the original LUN/volume by default and has a copy of the data up to the point-in-time of a snapshot.   

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. For more information about snapshots and how to order snapshot space, see the [Snapshot documentation](snapshots.html).  

Duplicates can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the new volume is created in the same data center as the replica volume.

Duplicate volumes can be accessed by a host for read/write as soon as the storage is provisioned. However, snapshots and replication aren't allowed until the data copy from the original to the duplicate is complete. 

When the data copy is complete, the duplicate can be managed and used as a completely independent volume. 

This feature is available in most locations. Click [here](new-ibm-block-and-file-storage-location-and-features.html) for the list of available data centers.

Some common uses for a duplicate volume:
- **Disaster Recovery Testing**: Create a duplicate of your replica volume to verify that the data is intact and can be used if a disaster occurs, without interrupting the replication. 
- **Golden Copy**: Use a storage volume as golden copy that you can create multiple instances from for various uses. 
- **Data refreshes**: Create a copy of your production data to mount to your non-production environment for testing. 
- **Restore from Snapshot**: Restore data on the original volume with specific files/date from a snapshot without overwriting the entire original volume with the snapshot restore function. 
- **Development and Testing (dev/test)**: Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing. 
- **Storage Resize**: Create a volume with new size, IOPS rate or both without needing to move your data.  
	
You can create a duplicate volume through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} in a couple of ways.


## Creating a duplicate from a specific volume in the Storage List

1. Go to your list of {{site.data.keyword.blockstorageshort}}
    - From the customer portal, click **Storage** > **{{site.data.keyword.blockstorageshort}}** OR
    - From the {{site.data.keyword.BluSoftlayer_full}} catalog click **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**. 
2. Select a volume from the list and click **Actions** > **Duplicate LUN (Volume)** 
3. Choose your snapshot option: 
    - If you order from a **non-replica** volume,
      - Select **Create from new snapshot** – this action creates a snapshot to be used for the duplicate. Use this option if your volume doesn't have current snapshots or if you want to create a duplicate right then.<br/>
      - Select **Create from latest snapshot** – this action creates a duplicate from the most recent snapshot that exists for this volume. 
    - If you order from a **replica** volume the only option for snapshot is to use the most recent snapshot available. 
4. Storage Type and Location remains the same as the original volume.
5. Hourly or Monthly Billing – you can choose to provision the duplicate LUN with hourly or monthly billing. The billing type for the original volume is automatically selected. If you want to choose a different billing type for your duplicate storage, you can make that selection here. 
5. You can specify IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection. 
    - If your original volume is 2, 4, or 10 IOPS Endurance tier, you can move anywhere between those tiers for the new volume. 
6. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default. 
    - **Note**: {{site.data.keyword.blockstorageshort}} can be resized to 10 times the original size of the volume. 
7. You can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume is set by default. 
8. Click **Continue** to place your order. 



## Creating a duplicate from a specific Snapshot

1. Go to your list of {{site.data.keyword.blockstorageshort}}
2. Click a **LUN/volume** from the list to view the details page. (It can either be a replica or non-replica volume.) 
3. Scroll down and select an existing snapshot from the list on the details page and click **Actions** > **Duplicate**.   
4. Storage Type (Endurance or Performance) and Location remain the same as the original volume. 
5. Available Performance and size combinations are displayed. The IOPs designation of the original volume is set by default. You can specify IOPS or IOPS Tier for the new volume. 
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection. 
    - If your original volume is 2, 4, or 10 IOPS Endurance tier, you can move anywhere between those tiers for the new volume. 
6. You can update the size of the new volume so that it is larger than the original. The size of the original volume is set by default. 
    - **Note**: {{site.data.keyword.blockstorageshort}} can be resized to 10 times the original size of the volume. 
7. You can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume is set by default. 
8. Click **Continue** to place your order for the duplicate. 


## Managing your duplicate volume

While data is being copied from the original volume to the duplicate, you can see a status on the details page that shows the duplication is in progress. During this time, you can attach to a host and read/write to the volume, but you can't create snapshot schedules. When the duplication process is complete, the new volume is independent from the original and can be managed with snapshots and replication as normal. 
