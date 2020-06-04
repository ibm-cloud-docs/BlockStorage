---

copyright:
  years: 2014, 2020
lastupdated: "2020-06-06"

keywords: Block Storage, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# FAQs
{: #block-storage-faqs}

## How many instances can share the use of a {{site.data.keyword.blockstorageshort}} volume?
{: #authorizationlimit}
{: faq}
{: #authlimit}
{: support}

The default limit for the number of authorizations per block volume is eight. This means that up to 8 hosts can be authorized to access the Block Storage LUN. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware deployment can request the authorization limit to be increased to 64. To request a limit increase, contact your sales representative or raise a [Support case](https://{DomainName}/unifiedsupport/cases/add){: external}.

## In our VMWare deployment, the hosts have multiple network cards with different IP addresses for network redundancy and expanded bandwidth. How can we authorize them all to access the same Storage volume?
{: #authsubnets}
{: faq}
{: help}
{: support}

It is possible to authorize a subnet of IP addresses to access a specific {{site.data.keyword.blockstorageshort}} volume through the console, SLCLI, or API. To authorize a host to connect from multiple IP addresses on a subnet, complete the following steps.

**Console UI**
1. Go to [Classic Infrastructure](https://{DomainName}/classic/devices){: external}.
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Locate the volume and click the ellipsis (**...**).
3. Click **Authorize Host**.
4. To see the list of available IP addresses, select **IP address** as the host type. Then, select the subnet where your host resides.
5. From the filtered list, select one or more IP addresses that can access the volume and click **Save**.

**SLCLI**
```
# slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```

## How many volumes can be ordered?
{: #provisioninglimit}
{: faq}
{: #orderlimit}
{: support}

By default, you can provision a combined total of 250 block and file storage. To increase your volume limit, contact your sales representative. For more information, see [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).

## How many {{site.data.keyword.blockstorageshort}} volumes can be mounted to a host?
{: #mountinglimit}
{: faq}
{: #volumelimit}
{: support}

That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.cloud}} limits. Refer to your OS documentation for limits on the number of volumes that can be mounted.

## Can I attach multiple LUNs with different OS settings?
{: #nomultipleOS}
{: faq}
{: #multiplelun}
{: support}

No. A host cannot be authorized to access LUNs of differing OS types at the same time. A host can be authorized to access LUNs of a **single** OS type. If you attempt to authorize a host to access multiple LUNs with different OS types, the operation results in an error.

## Which Windows version am I to choose for my Block Storage LUN?
{: #windowsOStypes}
{: faq}
{: support}

When you create a LUN, you must specify the OS type. The OS type must be based on the operating system, which is used by the hosts that access the LUN. The OS Type can't be modified after the LUN is created. The actual size of the LUN might vary slightly based on the OS type of the LUN.

**Windows GPT**
-  The LUN stores Windows data by using the GUID Partition Type (GPT) partitioning style. Use this option if you want to use the GPT partitioning method and your host can use it. Windows Server 2003, Service Pack 1 and later can use the GPT partitioning method, and all 64-bit versions of Windows support it.

**Windows 2003**
- The LUN stores a raw disk type in a single-partition Windows disk that uses the Master Boot Record (MBR) partitioning style. Use this option only if your host operating system is Windows 2000 Server, Windows XP, or Windows Server 2003 that uses the MBR partitioning method.

**Windows 2008+**
- The LUN stores Windows data for Windows 2008 and later versions. Use this OS option if your host operating system is Windows Server 2008, Windows Server 2012, Windows Server 2016. Both MBR and GPT partitioning methods are supported.


## Is the allocated IOPS limit enforced by instance or by volume?
{: #iopslimit}
{: faq}
{: #iopslimit}
{: support}

IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Measuring IOPS
{: #measuringIOPS}
{: faq}
{: #iopsmeasure}
{: support}

IOPS is measured based on a load profile of 16-KB blocks with random 50 percent read and 50 percent writes. Workloads that differ from this profile can experience inferior performance.

## What happens when a smaller block size is used to measure performance?
{: #blocksizeIOPS}
{: faq}
{: #smallblock}
{: support}

Maximum IOPS can still be obtained when you use smaller block sizes. However, throughput becomes smaller. For example, a volume with 6000 IOPS would have the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## Does the volume need to be pre-warmed to achieve expected throughput?
{: #prewarm}
{: faq}
{: support}

There's no need for pre-warming. You can observe specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved by using a faster Ethernet connection?
{: #bandwidthIOPS}
{: faq}
{: #ethernet}
{: support}

There are limits set at the LUN level and a faster Ethernet connection doesn't increase that limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls and security groups impact performance?
{: #isolatedstoragetraffic}
{: faq}
{: support}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## How do I route block storage traffic to its own VLAN interface and bypass a firewall?
{: #howtoisolatedstorage}
{: faq}
{: support}

To enact this best practice, complete the following steps.
1. Provision a VLAN in the same data center as the host and the block storage device. For more information, see [Getting started with VLANs](/docs/vlans?topic=vlans-getting-started){: external}.
1. Provision a secondary private subnet to the new VLAN.
2. Trunk the new VLAN to the private interface of the host.  
   This action momentarily disrupts the network traffic on the host while the VLAN is being trunked to the host.
   {:note}
3. Create a network interface on the host.
   * In Linux or Windows, create an 802.11q interface. Choose one of the unused secondary IP addresses from the newly trunked VLAN and assign that IP address, subnet mask, and gateway to the new 802.11q interface that you created.
  * In VMware, create a VMkernel network interface (vmk) and assign the unused secondary IP address, subnet mask, and gateway from the newly trunked VLAN to the new vmk interface.
4. Add a new persistent static route on the host to the target iSCSI subnet.

## What latency can be expected from the {{site.data.keyword.blockstorageshort}}?   
{: #latency}  
{: faq}
{: support}

Target latency within the storage is <1 ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## I ordered a {{site.data.keyword.blockstorageshort}} LUN in the wrong data center. Is it possible to move or migrate storage to another data center?
{: faq}
{: #movedatacenter}

You need to order new {{site.data.keyword.blockstorageshort}} in the right data center, and then cancel the {{site.data.keyword.blockstorageshort}} device you ordered in an incorrect location.

## Why can {{site.data.keyword.blockstorageshort}} with Endurance 10 IOPS/GB tier be ordered in some data centers and not in others?
{: #orderendurance}
{: faq}
{: support}

The 10 IOPS/GB tier of Endurance type {{site.data.keyword.blockstorageshort}} is available in most [data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

## How can we tell which {{site.data.keyword.blockstorageshort}} volumes are encrypted?
{: faq}
{: #volumeencrypt}
{: support}

When you look at your list of {{site.data.keyword.blockstorageshort}} in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/block){: external}, you can see a lock icon next to the volume name for the LUNs that are encrypted.

## How do we know when we're provisioning {{site.data.keyword.blockstorageshort}} in an upgraded data center?
{: faq}
{: #upgradedcenter}
{: support}

When you order {{site.data.keyword.blockstorageshort}}, all upgraded data centers are denoted with an asterisk (`*`) in the order form and an indication that you're about to provision storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that storage as encrypted. All encrypted volumes and LUNs are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs/BlockStorage?topic=BlockStorage-selectDC).

## If we own non-encrypted {{site.data.keyword.blockstorageshort}} in a data center that was recently upgraded, can we encrypt that {{site.data.keyword.blockstorageshort}}?
{: faq}
{: #encryptupgrade}
{: support}

{{site.data.keyword.blockstorageshort}} that is provisioned before the data center upgrade can't be encrypted.
New {{site.data.keyword.blockstorageshort}} that is provisioned in upgraded data centers is automatically encrypted. There's no encrypt setting to choose from, it’s automatic.
Data on non-encrypted storage in an upgraded data center can be encrypted by creating a LUN, then copying the data to the new encrypted LUN with host-based migration. For more information, see [Upgrading existing {{site.data.keyword.blockstorageshort}} to enhanced {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-migratestorage#migratestorage).

## Does {{site.data.keyword.blockstorageshort}} support SCSI-3 Persistent Reserve to implement I/O fencing for Db2 pureScale?
{: faq}
{: #scsi}
{: support}

Yes, {{site.data.keyword.blockstorageshort}} supports both SCSI-2 and SCSI-3 persistent reservations.

## What happens to the data when {{site.data.keyword.blockstorageshort}} LUNs are deleted?
{: #deleted}
{: faq}
{: support}

{{site.data.keyword.blockstoragefull}} presents Block volumes to customers on physical storage that is wiped before any reuse. Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization must perform the data sanitization procedure before they delete their storage.

## What happens to the drives that are decommissioned from the cloud data center?
{: faq}
{: #decommission}
{: support}

When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.

## I cannot cancel a Block Storage volume because the Cancel action in the Cloud console is unavailable or grayed out. What’s happening?
{: faq}
{: #cancelstorage}

The cancellation process for this storage device is in progress so the Cancel action is no longer available.  The volume remains visible for at least 24 hours until it’s reclaimed, with an hourglass or clock icon next to the device name to indicate that it’s in a waiting period.  The minimum 24-hour waiting period gives you a chance to void the cancel request if needed.

## My Windows 2012 host is supposed to have access to multiple Storage LUNs, but I can't see them in Disk Manager. How do I fix it?
{: faq}
{: #diskmanager}
{: help}
{: support}

If you use more than two iSCSI LUNs with the same host, and if all the iSCSI connections are from the same Storage device, you might find that you can see only two devices in Disk Manager. When this happens, you need to manually connect to each device in the iSCSI Initiator. For more information, see [troubleshooting Windows 2012 R2 - multiple iSCSI devices](/docs/BlockStorage?topic=BlockStorage-troubleshootingWin12).

## My storage appears offline or read-only. Why did it happen and how do I fix it?
{: #StorageOffline}
{: faq}
{: help}
{: support}

There are a couple of scenarios where a host (bare metal or VM) loses connection to the storage however briefly and as a result, the host considers that storage read-only to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read-only from the host's perspective even when the network connection is restored. A reboot of the host solves the read-only state issue.

This issue can be observed with hosts that do not have properly configured MPIO settings. If MPIO is not configured correctly, the host loses connection to the storage and might not be able to reconnect to the storage when the connectivity issue is resolved.

## How can I configure and validate multipath connections to the {{site.data.keyword.blockstorageshort}} volume?
{: #correctMPIO}
{: faq}
{: help}
{: support}

If MPIO is configured right, then when an unplanned disruption or a planned maintenance occurs, and one of the routes is taken down, the host can still access the attached storage through the second path. For more information about the MPIO settings, see the following articles.
- [Mounting LUNs on Linux](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verifying MPIO on Linux](/docs/BlockStorage?topic=BlockStorage-mountingLinux#verifyMPIOLinux)
- [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
- [Verifying MPIO on MS Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows#verifyMPIOWindows)

## I expanded the volume size of my block storage using the Cloud console, but the size on my server is still the same.  How do I fix it?
{: faq}
{: #expandsize}

To see the new expanded LUN size, you need to configure your existing block storage disk on the server.  Check your operating system documentation for steps.
