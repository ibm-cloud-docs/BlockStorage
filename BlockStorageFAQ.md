---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} Frequently Asked Questions

## How many instances can share the use of a {{site.data.keyword.blockstorageshort}} volume?
The default limit for number of authorizations per block volume is 8. To increase the limit, contact your sales representative.

## Is the allocated IOPS enforced by instance or by volume?
IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## How is IOPS measured?
IOPS is measured based on a load profile of 16 KB blocks with random 50 percent read and 50 percent writes. Workloads that differ from this profile can experience inferior performance.

## What happens when a smaller block size is used to measure performance?
Maximum IOPS can still be obtained when you use smaller block sizes. However, throughput becomes smaller. For example, a volume with 6000 IOPS would have the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec 
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## Does the volume need to be pre-warmed to achieve expected throughput?
There's no need for pre-warming. You can observe specified throughput immediately upon provisioning the volume.

## Why can {{site.data.keyword.blockstorageshort}} with Endurance 10 IOPS/GB tier be ordered in some data centers and not in others?
The 10 IOPS/GB tier of Endurance type {{site.data.keyword.blockstorageshort}} is only available in select data centers, and new data centers are being added gradually. You can find a full list of upgraded data centers and available features [here](new-ibm-block-and-file-storage-location-and-features.html).

## How can we tell which {{site.data.keyword.blockstorageshort}} LUNs/volumes are encrypted?
When you look at your list of {{site.data.keyword.blockstorageshort}} in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, you can see a lock icon to the right of LUN/volume name for those LUNs/volumes that are encrypted.

## How do we know when we're provisioning {{site.data.keyword.blockstorageshort}} in an upgraded data center?
When you order {{site.data.keyword.blockstorageshort}}, all upgraded data centers are denoted with an asterisk (`*`) in the order form and an indication that you're about to provision storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that storage as encrypted. All encrypted volumes and LUNs are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](new-ibm-block-and-file-storage-location-and-features.html).

## If we own non-encrypted {{site.data.keyword.blockstorageshort}} in a data center that was recently upgraded, can we encrypt that {{site.data.keyword.blockstorageshort}}?

{{site.data.keyword.blockstorageshort}} that is provisioned before the data center upgrade can't be encrypted. 
New {{site.data.keyword.blockstorageshort}} provisioned in upgraded data centers is automatically encrypted. There's no encrypt setting to choose from, it’s automatic. 
Data on non-encrypted storage in an upgraded data center can be encrypted by creating a new Block LUN, then copying the data to the new encrypted LUN with host-based migration. Click [here](migrate-block-storage-encrypted-block-storage.html) for instructions.

## How many volumes can be ordered?

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase your volume, contact your sales representative to increase your volumes.

## Can more throughput be achieved by using a faster Ethernet connection?

Throughput limits are set at a per-volume/LUN level so using a faster Ethernet connection doesn't increase that set limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls/security groups impact performance?

We suggest running storage traffic on a VLAN, which bypasses the firewall as a best practice. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## What latency can be expected from the {{site.data.keyword.blockstorageshort}}?   

Target latency within the storage is <1 ms. Our storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## Does {{site.data.keyword.blockstorageshort}} support SCSI-3 Persistent Reserve to implement I/O fencing for Db2 pureScale?

Yes, {{site.data.keyword.blockstorageshort}} supports both SCSI-2 and SCSI-3 persistent reservations.

## What happens to the data when {{site.data.keyword.blockstorageshort}} LUNs are deleted?

When storage is deleted, any pointers to the data on that volume are removed thus the data becomes inaccessible. If the physical storage is reprovisioned to another account a new set of pointers are assigned. There’s no way for the new account to access any data that was on the physical storage. The new set of pointers shows all 0's. The new data overwrites any inaccessible data that existed on that physical storage.

## What happens to the drives that are decommissioned from the cloud data center?

When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.
