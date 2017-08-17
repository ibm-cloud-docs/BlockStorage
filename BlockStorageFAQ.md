---

copyright:
  years: 2014, 2017
lastupdated: "2017-07-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Block Storage FAQ

## How many instances can share the use of a provisioned Block Storage volume?
The default limit for number of authorizations per block volume is 8. Please contact your sales representative to increase the limit.

## When provisioning Performance or Endurance Block/File Storage, are the allocated IOPS enforced by instance or by volume?
IOPS are enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## How are IOPS measured?
IOPS are measured based on a load profile of 16kb blocks with random 50% read and 50% writes. Workloads that differ from this profile may experience lower performance.

## What happens if I use a smaller block size when measuring performance?
Maximum IOPS can still be obtained when using smaller block sizes, however throughput will be lower. For example; a volume with 6000 IOPS would have the following throughput at various block sizes:

16KB * 6000 IOPS == ~93.75 MB/sec
8KB * 6000 IOPS == ~46.88 MB/sec
4KB * 6000 IOPS == ~23.44 MB/sec

## Does the volume need to be pre-warmed to achieve expected throughput?
There is no need for pre-warming. You will observe specified throughput immediately upon provisioning the volume.

## Why can I provision block and file storage with an Endurance 10 IOPS tier in some data centers and not in others?
The Block and File Endurance storage type 10 IOPS/GB tier is only available in select data centers, with new data centers being added soon.  You can find a full list of upgraded data centers and available features here.

## How can I tell which of my Block or File storage LUNs/volumes are encrypted?
When viewing your list of Block or File storage in the customer portal, you will see a lock icon to the right of LUN/volume name for those that are encrypted.

## How do I know if I am provisioning Block or File storage in an upgraded data center?
When provisioning Block or File storage, all upgraded data centers will be denoted with an asterisk (*) in the order form and an indication that you will be provisioning storage with encryption. Once the storage is provisioned, you will see an icon in the storage list that shows that volume or volume as encrypted. All encrypted volumes and volumes are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features here.

## Why can I provision block and file storage with an Endurance 10 IOPS tier in some data centers and not in others?
The File storage Endurance type 10 IOPS/GB tier is only available in select data centers, with new data centers being added soon.  You can find a full list of upgraded data centers and available features here.

## If I have non-encrypted Block storage provisioned in a data center that has been upgraded for encryption, can I encrypt my Block storage?
Block storage that is provisioned prior to a data center upgrade cannot be encrypted. New Block storage provisioned in upgraded data centers is automatically encrypted ; there is no encrypt setting to choose from, it’s automatic.. Data on non-encrypted storage in an upgraded data center can be encrypted by creating a new Block LUN, then copying the data to the new encrypted LUN with host-based migration. See this article for instructions on how to perform the migration.

## How many volumes can I provision?
By default, you can provision a combined total of 250 block and file storage volumes.  Please contact your sales representative to increase your volumes.

## Will I be able to achieve more throughput if I used a faster Ethernet connection?
Throughput limits are set at a per-volume/LUN level so using a faster Ethernet connection will not increase that set limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.
