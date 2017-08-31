---

copyright:
  years: 2014, 2017
lastupdated: "2017-08-31"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# How to Create a Duplicate Block Volume

We provide the ability to create a duplicate of an existing Block or File storage volume. The duplicate volume will inherit the capacity and performance options of the original LUN/volume by default and will have a copy of the data up to the point in time of a snapshot.   

Because the duplicate is based on the data in a point in time snapshot, snapshot space is required on the original volume before you can create a duplicate.  To learn more about snapshots and how to order snapshot space, refer to [Snapshot documentation](snapshots.html).  

Duplicates can be created from both primary and replica volumes, with the new duplicate being created in the same data center as the original volume.  For example, if you create a duplicate from a replica volume, the new volume will be created in the same data center as the replica volume.    

Duplicate volumes can be accessed by a host for read/write as soon as the storage is provisioned.  Snapshots and replication will not be allowed until the data copy from the original to the duplicate is complete. 

Once the data copy is complete, the duplicate can be managed and used as a completely independent volume from the original. 

This feature is only available for storage that is provisioned with encryption. Click [here](new-ibm-block-and-file-storage-location-and-features.html) for a list of available data centers. 

Some common uses for a duplicate volume:
- **Disaster Recovery Testing**: Create a duplicate of your replica volume to verify the data is intact and can be used in the event of a disaster, without interrupting the replication. 
- **Golden Copy**: Use a storage volume as golden copy that you can create multiple instances from for various uses. 
- **Data refresh**: Create a copy of your production data to mount to your non-production environment for testing. 
- **Restore from Snapshot**: Restore data on the original volume with specific files/date from a snapshot without overwriting the entire original volume with a snapshot restore function. 
- **Dev/Test**: Create up to 4 simultaneous duplicates of a volume at one time to create volumes with duplicate data for development and test. 
- **Storage Resize**: Create a new volume with the new size and/or IOPS without having to perform a host based migration of your data.  
	

There are a couple of ways to create a duplicate volume via the customer portal: 

## How to Create a Duplicate From a Specific Volume in the Storage List

Navigate to your list of Block:

From the customer portal, click **Storage**, **Block Storage** OR from Bluemix Catalog click **Infrastructure->Storage->Block Storage**. 


1. Select a LUN from the list and click **Actions** -> **Duplicate LUN (Volume)** 
2. Choose your snapshot option: 
    - If ordering from a non-replica volume:
      - Select **Create from new snapshot** – this will create a new snapshot that will be used for the duplicate. Use this option if there are currently no snapshots for your volume or if you want to create a duplicate at that point in time.
    
      OR 
      - Select **Create from latest snapshot** – this will create a duplicate from whatever the latest snapshot that exists for this volume. 
    - If ordering from a replica volume – the only option for snapshot is to use the latest snapshot available. 
3. Storage Type (Endurance or Performance) and Location will remain the same as the original volume.
4. If desired you can specify IOPs or IOPs Tier for the new volume. The IOPs designation of the original volume is set by default. 
    - If your original volume is 0.25 IOPs Endurance tier, you will not be able to make a new selection. 
    - If you original volume is 2, 4, or 10 IOPs Endurance tier, you can move anywhere between those tiers for the new volume. 
    - Available Performance and size combinations will be displayed. 
5. If desired you can update the size of the new volume so that it is larger than the original.  The size of the original volume is set by default. 
    - **Note**: Block storage can only be resized to 10x the original size of the volume. 
6. If desired you can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume will be set by default. 
7. Click **Continue** to place your order for the duplicate. 



## How to Create a Duplicate From a Specific Snapshot

Navigate to your list of Block or File storage:

1. Click on a **LUN/volume** from the list to view the details page. (Can either be a replica or non-replica volume) 
2. Scroll down and select an existing snapshot from the list on the details page and click **Actions ->Duplicate**.   
3. Storage Type (Endurance or Performance) and Location will remain the same as the original volume. 
4. If desired, you can specify IOPs or IOPs Tier for the new volume. The IOPs designation of the original volume is set by default. 
    - If your original volume is 0.25 IOPs Endurance tier, you will not be able to make a new selection. 
    - If you original volume is 2, 4, or 10 IOPs Endurance tier, you can move anywhere between those tiers for the new volume. 
    - Available Performance and size combinations will be displayed. 
5. If desired, you can update the size of the new volume so that it is larger than the original.  The size of the original volume is set by default. 
    - **Note**: Block storage can only be resized to 10x the original size of the volume. 
6. If desired, you can update the snapshot space for the new volume to add more, less, or no snapshot space. The snapshot space of the original volume will be set by default. 
7. Click **Continue** to place your order for the duplicate. 


## How to Manage Your Duplicate Volume

While data is being copied from the original volume to the duplicate, you will see a status on the details page that states the duplication is in progress. During this time you can attach to a host and read/write to the volume, but you cannot create snapshot schedules. Once the duplication process is complete, the new volume will be completely independent of the original volume and can be managed with snapshots and replication, etc. as normal. 
