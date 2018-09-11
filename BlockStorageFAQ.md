---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-10"

---
{:new_window: target="_blank"}

# FAQs

## How many instances can share the use of a {{site.data.keyword.blockstorageshort}} volume?
The default limit for the number of authorizations per block volume is eight. This means that up to 8 hosts can be authorized to access the Block Storage LUN. To request a limit increase, contact your sales representative.

## How many volumes can be ordered?
By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} volumes. To increase your volume limit, contact your sales representative.

## How many {{site.data.keyword.blockstorageshort}} volumes can be mounted to a host?
That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.BluSoftlayer_full}} limits. Refer to your OS documentation for limits on the number of volumes that can be mounted.

## Is the allocated IOPS limit enforced by instance or by volume?
IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Measuring IOPS
IOPS is measured based on a load profile of 16 KB blocks with random 50 percent read and 50 percent writes. Workloads that differ from this profile can experience inferior performance.

## What happens when a smaller block size is used to measure performance?
Maximum IOPS can still be obtained when you use smaller block sizes. However, throughput becomes smaller. For example, a volume with 6000 IOPS would have the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec 
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## Does the volume need to be pre-warmed to achieve expected throughput?
There's no need for pre-warming. You can observe specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved by using a faster Ethernet connection?
Throughput limits are set at a per-volume/LUN level so using a faster Ethernet connection doesn't increase that set limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls/security groups impact performance?
It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## What latency can be expected from the {{site.data.keyword.blockstorageshort}}?   
Target latency within the storage is <1 ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

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

## Does {{site.data.keyword.blockstorageshort}} support SCSI-3 Persistent Reserve to implement I/O fencing for Db2 pureScale?
Yes, {{site.data.keyword.blockstorageshort}} supports both SCSI-2 and SCSI-3 persistent reservations.

## What happens to the data when {{site.data.keyword.blockstorageshort}} LUNs are deleted?
{{site.data.keyword.blockstoragefull}} presents Block volumes to customers on physical storage that is wiped before any re-use. Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization must perform the data sanitization procedure before they delete their storage.

## What happens to the drives that are decommissioned from the cloud data center?
When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.
