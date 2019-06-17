---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}

# FAQs
{: #block-storage-faqs}

## How many instances can share the use of a {{site.data.keyword.blockstorageshort}} volume?
{: faq}

The default limit for the number of authorizations per block volume is eight. This means that up to 8 hosts can be authorized to access the Block Storage LUN. To request a limit increase, contact your sales representative.

## How many volumes can be ordered?
{: faq}

By default, you can provision a combined total of 250 block and file storage. To increase your volume limit, contact your sales representative. For more information, see [Managing storage limits](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).

## How many {{site.data.keyword.blockstorageshort}} volumes can be mounted to a host?
{: faq}

That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.cloud}} limits. Refer to your OS documentation for limits on the number of volumes that can be mounted.

## Can I attach multiple LUNs with different OS settings?
{: faq}

No. A host cannot be authorized to access LUNs of differing OS types at the same time. A host can only be authorized to access LUNs of a single OS type. If you attempt to authorize access to multiple LUNs with different OS types, the operation results in an error.

## Which Windows version am I to choose for my Block Storage LUN?
{: #windowsOStypes}
{: faq}

When you create a LUN, you must specify the OS type. The OS type must be based on the operating system, which is used by the hosts that access the LUN. The OS Type can't be modified after the LUN is created. The actual size of the LUN might vary slightly based on the OS type of the LUN.

**Windows 2008+**
- The LUN stores Windows data for Windows 2008 and later versions. Use this OS option if your host operating system is Windows Server 2008, Windows Server 2012, Windows Server 2016. Both MBR and GPT partitioning methods are supported.

**Windows 2003**
- The LUN stores a raw disk type in a single-partition Windows disk that uses the Master Boot Record (MBR) partitioning style. Use this option only if your host operating system is Windows 2000 Server, Windows XP, or Windows Server 2003 that uses the MBR partitioning method.

**Windows GPT**
-  The LUN stores Windows data by using the GUID Partition Type (GPT) partitioning style. Use this option if you want to use the GPT partitioning method and your host can use it. Windows Server 2003, Service Pack 1 and later can use the GPT partitioning method, and all 64-bit versions of Windows support it.

## Is the allocated IOPS limit enforced by instance or by volume?
{: faq}

IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Measuring IOPS
{: faq}

IOPS is measured based on a load profile of 16-KB blocks with random 50 percent read and 50 percent writes. Workloads that differ from this profile can experience inferior performance.

## What happens when a smaller block size is used to measure performance?
{: faq}

Maximum IOPS can still be obtained when you use smaller block sizes. However, throughput becomes smaller. For example, a volume with 6000 IOPS would have the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}

There's no need for pre-warming. You can observe specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved by using a faster Ethernet connection?
{: faq}

Throughput limits are set at a per-LUN level so using a faster Ethernet connection doesn't increase that set limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls and security groups impact performance?
{: #isolatedstoragetraffic}
{: faq}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## What latency can be expected from the {{site.data.keyword.blockstorageshort}}?   
{: faq}

Target latency within the storage is <1 ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## Why can {{site.data.keyword.blockstorageshort}} with Endurance 10 IOPS/GB tier be ordered in some data centers and not in others?
{: faq}

The 10 IOPS/GB tier of Endurance type {{site.data.keyword.blockstorageshort}} is only available in select data centers, and new data centers are being added gradually. You can find a full list of upgraded data centers and available features [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

## How can we tell which {{site.data.keyword.blockstorageshort}} volumes are encrypted?
{: faq}

When you look at your list of {{site.data.keyword.blockstorageshort}} in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage){: external}, you can see a lock icon next to the volume name for the LUNs that are encrypted.

## How do we know when we're provisioning {{site.data.keyword.blockstorageshort}} in an upgraded data center?
{: faq}

When you order {{site.data.keyword.blockstorageshort}}, all upgraded data centers are denoted with an asterisk (`*`) in the order form and an indication that you're about to provision storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that storage as encrypted. All encrypted volumes and LUNs are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

## If we own non-encrypted {{site.data.keyword.blockstorageshort}} in a data center that was recently upgraded, can we encrypt that {{site.data.keyword.blockstorageshort}}?
{: faq}

{{site.data.keyword.blockstorageshort}} that is provisioned before the data center upgrade can't be encrypted.
New {{site.data.keyword.blockstorageshort}} that is provisioned in upgraded data centers is automatically encrypted. There's no encrypt setting to choose from, it’s automatic.
Data on non-encrypted storage in an upgraded data center can be encrypted by creating a new Block LUN, then copying the data to the new encrypted LUN with host-based migration. Click [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage#migratestorage) for instructions.

## Does {{site.data.keyword.blockstorageshort}} support SCSI-3 Persistent Reserve to implement I/O fencing for Db2 pureScale?
{: faq}

Yes, {{site.data.keyword.blockstorageshort}} supports both SCSI-2 and SCSI-3 persistent reservations.

## What happens to the data when {{site.data.keyword.blockstorageshort}} LUNs are deleted?
{: faq}

{{site.data.keyword.blockstoragefull}} presents Block volumes to customers on physical storage that is wiped before any reuse. Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization must perform the data sanitization procedure before they delete their storage.

## What happens to the drives that are decommissioned from the cloud data center?
{: faq}

When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.
