---

copyright:
  years: 2014, 2024
lastupdated: "2024-06-21"

keywords: Block Storage for Classic, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# FAQs for {{site.data.keyword.blockstorageshort}}
{: #block-storage-faqs}

## How many server instances can share the use of a {{site.data.keyword.blockstorageshort}} volume?
{: #authlimit}
{: faq}
{: support}

The default limit for the number of authorizations per block volume is eight. That means that up to eight hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} LUN. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware&reg; deployment can request the authorization limit to be increased to 64. To request a limit increase, contact Support by raising a [Support case](/unifiedsupport/cases/add){: external}.

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: important}

## Our Compute hosts have multiple network cards with different IP addresses for network redundancy and expanded bandwidth. How can we authorize them all to access the same Storage volume?
{: #authsubnets}
{: faq}
{: support}

It is possible to authorize a subnet of IP addresses to access a specific {{site.data.keyword.blockstorageshort}} volume through the console, SLCLI, or API. To authorize a host to connect from multiple IP addresses on a subnet, complete the following steps.

### Console UI
{: #authinUI}

1. Go to [Classic Infrastructure](/gen1/infrastructure/devices){: external}.
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
4. Click **Authorize Host**.
5. To see the list of available IP addresses, select the **IP address** as the host type. Then, select the subnet where your host resides.
6. From the filtered list, select one or more IP addresses that can access the volume and click **Save**.

### SLCLI
{: #authinSLCLI}

```sh
$ slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```
{: screen}

## How many volumes can be ordered?
{: #orderlimit}
{: faq}
{: support}

By default, you can provision a combined total of 700 block storage and file storage volumes. To increase your volume limit, contact Support. For more information, see [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).

## How many {{site.data.keyword.blockstorageshort}} volumes can be mounted to a host?
{: #volumelimit}
{: faq}
{: support}

That depends on what the host operating system can handle, but it’s not something that {{site.data.keyword.cloud}} limits. Refer to your OS Documentation for limits on the number of volumes that can be mounted.

## Can I attach multiple LUNs with different OS settings?
{: #multiplelun}
{: faq}
{: support}

No. A host cannot be authorized to access LUNs of differing OS types at the same time. A host can be authorized to access LUNs of a **single** OS type. If you attempt to authorize a host to access multiple LUNs with different OS types, the operation results in an error.

## Which Windows version am I to choose for my {{site.data.keyword.blockstorageshort}} LUN?
{: #windowsOStypes}
{: faq}
{: support}

When you create a LUN, you must specify the OS type. The OS type specifies the operating system of the host that's going to access the LUN. It also determines the layout of data on the LUN, the geometry that is used to access that data, and the minimum and maximum size of the LUN. The OS Type can't be modified after the LUN is created. The actual size of the LUN might vary slightly based on the OS type of the LUN. Choosing the correct type for your Windows OS helps to prevent mis-aligned IO operations.

If the LUN is being presented as a raw block device to a guest, select the OS type of the guest's OS. If the LUN is being presented to the hypervisor to serve Virtual hard disk (VHD) files, choose Hyper-V.

### Windows GPT
{: #winGPT}

-  The LUN stores Windows data by using the GUID Partition Type (GPT) partitioning style. Use this option if you want to use the GPT partitioning method and your host can use it. Windows Server 2003, Service Pack 1 and later can use the GPT partitioning method, and all 64-bit versions of Windows support it.

### Windows 2003
{: #win2003}

- The LUN stores a raw disk type in a single-partition Windows disk that uses the Master Boot Record (MBR) partitioning style. Use this option only if your host operating system is Windows 2000 Server, Windows XP, or Windows Server 2003 that uses the MBR partitioning method.

### Windows 2008+
{: #win2008}

- The LUN stores Windows data for Windows 2008 and later versions. Use this OS option if your host operating system is Windows Server 2008, Windows Server 2012, Windows Server 2016. Both MBR and GPT partitioning methods are supported.

### Hyper-V
{: #Hyper-V}

- VHDX is the virtual hard disk format that was introduced in Windows Server 2012 to create resilient high-performance virtual disks. The format has many benefits, such as supporting larger virtual disk sizes and larger block sizes. It provides protection against data corruption during power failures by logging updates to the VHDX metadata structures. If the LUN is being presented to the hypervisor to serve VHD files, choose Hyper-V for its OS type.

## Is the allocated IOPS limit enforced by instance or by volume?
{: #iopslimit}
{: faq}
{: support}

IOPS is enforced at the volume level. In other words, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

The number of hosts that are accessing the volume is important because when only a single host is accessing the volume, it can be difficult to realize the maximum IOPS available. 

## Measuring IOPS
{: #iopsmeasure}
{: faq}
{: support}

IOPS is measured based on a load profile of 16-KB blocks with random 50 percent read and 50 percent writes. Workloads that differ from this profile can experience inferior performance. To improve performance, you can try [adjusting the host queue depth settings](/docs/BlockStorage?topic=BlockStorage-hostqueuesettings) or [enabling Jumbo frames](/docs/FileStorage?topic=FileStorage-jumboframes).

## What happens when a smaller block size is used to measure performance?
{: #smallblock}
{: faq}
{: support}

Maximum IOPS can still be obtained when you use smaller block sizes. However, throughput becomes smaller. For example, a volume with 6000 IOPS would have the following throughput at various block sizes:

- 16 KB * 6000 IOPS == ~93.75 MB/sec
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## How can I tell how much of the Storage is being used? Why are the usage details of {{site.data.keyword.blockstorageshort}} not displayed in the UI?
{: #blockstoruse}
{: faq}

{{site.data.keyword.blockstorageshort}} is yours to format and manage the way that you want to. {{site.data.keyword.cloud}} can't see the contents of the LUN, and so the UI can't provide information about the disk space usage. You can obtain more information about the volume, such as how much disk space is taken and how much is available, from your Compute host's operating system.

You can use the following commands.
- Linux&reg;:
   ```txt
   df -h
   ```
   {: pre}

   The command provides an output that shows how much space is available space and the percentage used.
   ```sh
   $ df -hT /dev/sda1
   Filesystem     Type      Size  Used Avail Use% Mounted on
   /dev/sda1      disk      6.0G  1.2G  4.9G  20% /
   ```

- Windows: you have two options.
   ```txt
   fsutil volume diskfree C:
   ```
   {: pre}

   ```text
   dir C:
   ```
   {: pre}

   The last line of the output shows how much space is unused.

   You can also view the free disk space in the File Explorer by clicking This PC.

### Why does the available capacity that I see in my OS not match the capacity that I provisioned?
{: faq}
{: #faq-storage-units-2}

One of the reasons can be that your operating system uses base-2 conversion. For example, when you provision a 4000 GB volume in the UI, the storage system reserves a 4,000 GiB volume or 4,294,967,296,000 bytes of storage space for you. The provisioned volume size is larger than 4 TB. However, your operating system might display the storage size as 3.9 T because it uses base-2 conversion and the T stands for TiB, not TB.

Second, partitioning your Block Storage and creating a file system on it reduces available storage space. The amount by which formatting reduces space varies depending upon the type of formatting that is used and the amount and size of the various files on the system.

### Is storage capacity measured in GB or GiB?
{: faq}
{: #faq-storage-units}

One confusing aspect of storage is the units that storage capacity and usage are reported in. Sometime GB is really gigabytes (base-10) and sometimes GB represents gibibytes (base-2) which ought to be abbreviated as GiB.

Humans usually think and calculate numbers in the decimal (base-10) system. In our documentation, we refer to storage capacity by using the unit GB (Gigabytes) to align with the industry standard terminology. In the UI, CLI, API, and Terraform, you see the unit GB used and displayed when you query the capacity. When you want to order a 4-TB volume, you enter 4,000 GB in your provisioning request.

However, computers operate in binary, so it makes more sense to represent some resources like memory address spaces in base-2. Since 1984, computer file systems show sizes in base-2 to go along with the memory. Back then, available storage devices were smaller, and the size difference between the binary and decimal units was negligible. Now that the available storage systems are considerably larger this unit difference is causing confusion.

The difference between GB and GiB lies in their numerical representation:
- GB (Gigabyte) is a decimal unit, where 1 GB equals 1,000,000,000 bytes. When you convert GB to TB, you use 1000 as the multiplier.
- GiB (Gibibyte), is a binary unit, where 1 GiB equals 1,073,741,824 bytes. When you convert GiB to TiB, you use 1024 as the multiplier.

The following table shows the same number of bytes expressed in decimal and binary units.

| Decimal SI (base 10) | Binary (base 2)       |
|----------------------|-----------------------|
| 2,000,000,000,000 B  | 2,000,000,000,000 B   |
|     2,000,000,000 KB |     1,953,125,000 KiB |
|         2,000,000 MB |         1,907,348 MiB |
|             2,000 GB |             1,862 GiB |
|                 2 TB |              1.81 TiB |
{: caption="Table 1. Decimal vs Binary units" caption-side="bottom"}

The storage system uses base-2 units for volume allocation. So if your volume is provisioned as 4,000 GB, that's really 4,000 GiB or 4,294,967,296,000 bytes of storage space. The provisioned volume size is larger than 4 TB. However, your operating system might display the storage size as 3.9 T because it uses base-2 conversion and the T stands for TiB, not TB. 

## Does the volume need to be pre-warmed to achieve the expected throughput?
{: #prewarm}
{: faq}
{: support}

Pre-warming is not needed. You can observe specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved by using a faster Ethernet connection?
{: #ethernet}
{: faq}
{: support}

Throughput limits are set at the LUN level and a faster Ethernet connection doesn't increase that limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

## Do firewalls and security groups impact performance?
{: #isolatedstoragetraffic}
{: faq}
{: support}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## How do I route {{site.data.keyword.blockstorageshort}} traffic to its own VLAN interface and bypass a firewall?
{: #howtoisolatedstorage}
{: faq}
{: support}

To enact this best practice, complete the following steps.
1. Provision a VLAN in the same data center as the host and the {{site.data.keyword.blockstorageshort}} device. For more information, see [Getting started with VLANs](/docs/vlans?topic=vlans-getting-started){: external}.
2. Provision a secondary private subnet to the new VLAN.3
3. Trunk the new VLAN to the private interface of the host. For more information, see [How do I trunk my VLANs to my servers](/docs/vlans?topic=vlans-vlans-faqs#trunk-vlans-to-servers){: external}.

   This action momentarily disrupts the network traffic on the host while the VLAN is being trunked to the host.
   {: note}

4. Create a network interface on the host.
   * In Linux&reg; or Windows, create an 802.11q interface. Choose one of the unused secondary IP addresses from the newly trunked VLAN and assign that IP address, subnet mask, and gateway to the new 802.11q interface that you created.
   * In VMware&reg;, create a VMkernel network interface (vmk) and assign the unused secondary IP address, subnet mask, and gateway from the newly trunked VLAN to the new vmk interface.
5. Add a new persistent static route on the host to the target iSCSI subnet.
6. Ensure that the IP for the newly added interface is added to the host authorization list.
7. Perform discovery and log in to target portal as described in the following topics.
   - [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Mounting LUNs on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)

## Is it good to run iSCSI traffic over 802.3ad LACP port channel?
{: #MPIOvsLACP}
{: faq}
{: support}

No. Link Aggregation Control Protocol (LACP) is not a recommended configuration with iSCSI. Use the multi-path input/output (MPIO) framework for I/O balancing and redundancy.

With an MPIO configuration, a server with multiple NICs can transmit and receive I/O across all available interfaces to a corresponding MPIO-enabled storage device. This setup provides redundancy that can ensure that the storage traffic remains steady even if one of the paths becomes unavailable. If a server has two 1-Gb NICs and the storage server has two 1-Gb NICs, the theoretical maximum throughput is about 200 MB/s.

Link aggregation (such as LACP or 802.3ad) through NIC teaming does not work the same way as MPIO. Link aggregation does not improve the throughput of a single I/O flow, nor does it provide multiple paths. A single flow always traverses one single path. The benefit of link aggregation can be observed when several “unique” flows exist, and each flow comes from a different source. Each individual flow is sent down its own available NIC interface, which is determined by a hash algorithm. Thus with more unique flows, more NICs can provide greater aggregate throughput.

Bonding works between a server and switch. However, MPIO works between a storage server and the host, even if a switch is in the path.

For more information, see one of the following articles.
- Red Hat Linux&reg;: [Is the use of bonded NIC interfaces recommended with iscsi?](https://access.redhat.com/solutions/41899){: external}
- Microsoft Windows: [NIC Teaming and iSCSI](https://learn.microsoft.com/en-us/archive/msdn-technet-forums/441d2157-119d-4b1e-b40c-1aa3670e44a6){: external}.
- VMware&reg;: [Host requirements for link aggregation](https://knowledge.broadcom.com/external/article?legacyId=1001938){: external} or [iSCSI and LAG/LACP](https://core.vmware.com/blog/iscsi-and-laglacp){: external}.

## What latency can be expected from the {{site.data.keyword.blockstorageshort}}?
{: #latency}
{: faq}
{: support}

Target latency within the storage is <1 ms. The storage is connected to Compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## I ordered a {{site.data.keyword.blockstorageshort}} LUN in the wrong data center. Is it possible to move or migrate storage to another data center?
{: #movedatacenter}
{: faq}
{: support}

You need to order a new {{site.data.keyword.blockstorageshort}} volume in the correct data center, and then cancel the {{site.data.keyword.blockstorageshort}} device that you ordered in the wrong location. When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. 
The 24-hour waiting period gives you a chance to void the cancellation request if needed. If you want to cancel the deletion of the volume, raise a [Support case](/unifiedsupport/cases/add){: external}. Billing for the volume stops immediately. When the reclaim period expires, the data is destroyed and the volume is removed from the console, too.

## How can we tell which {{site.data.keyword.blockstorageshort}} volumes are encrypted?
{: #volumeencrypt}
{: faq}
{: support}

When you look at your list of {{site.data.keyword.blockstorageshort}} in the [{{site.data.keyword.cloud}} console](/login){: external}, you can see a lock icon next to the volume name for the LUNs that are encrypted.

## Does {{site.data.keyword.blockstorageshort}} support SCSI-3 Persistent Reserve to implement I/O fencing for Db2 pureScale?
{: faq}
{: #scsi}
{: support}

Yes, {{site.data.keyword.blockstorageshort}} supports both SCSI-2 and SCSI-3 persistent reservations.

## What happens to the data when {{site.data.keyword.blockstorageshort}} LUNs are deleted?
{: #deleted}
{: faq}
{: support}

{{site.data.keyword.blockstoragefull}} presents Block volumes to customers on physical storage that is wiped before any reuse.

When you delete a {{site.data.keyword.blockstorageshort}} volume, that data immediately becomes inaccessible. All pointers to the data on the physical disk are removed. If you later create a new volume in the same or another account, a new set of pointers is assigned. The account can't access any data that was on the physical storage because those pointers are deleted. When new data is written to the disk, any inaccessible data from the deleted volume is overwritten.

IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten and eradicated. Further, when you delete a {{site.data.keyword.blockstorageshort}} volume, those blocks must be overwritten before that block storage is made available again, either to you or to another customer.

When IBM decommissions a physical drive, the drive is destroyed before disposal. The decommissioned drives are unusable and any data on them is inaccessible.

Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization can perform the data sanitization procedure before they delete their storage.

## What happens to the drives that are decommissioned from the cloud data center?
{: faq}
{: #decommission}
{: support}

When drives are decommissioned, IBM destroys them before they are disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.

## I cannot cancel a {{site.data.keyword.blockstorageshort}} volume because the Cancel action in the Cloud console is disabled. What’s happening?
{: faq}
{: #cancelstorage}

The cancellation process for this storage device is in progress so the Cancel action is no longer available. The volume remains visible for at least 24 hours until it is reclaimed. The UI indicates that it’s inactive and the status "Cancellation pending" is displayed. The minimum 24-hour waiting period gives you a chance to void the cancellation request if needed. If you want to cancel the deletion of the volume, raise a [Support case](/unifiedsupport/cases/add){: external}.

## My Windows 2012 host is supposed to have access to multiple Storage LUNs, but I can't see them in Disk Manager. How do I fix it?
{: faq}
{: #diskmanager}
{: support}

If you use more than two LUNs with the same host, and if all the iSCSI connections are from the same Storage device, you might see only two devices in Disk Manager. When this situation happens, you need to manually connect to each device in the iSCSI Initiator. For more information, see [troubleshooting Windows 2012 R2 - multiple iSCSI devices](/docs/BlockStorage?topic=BlockStorage-troubleshootingWin12).

## My storage appears offline or read-only. Why did it happen and how do I fix it?
{: #StorageOffline}
{: faq}
{: support}

In a couple of scenarios a host (bare metal or VM) might lose connection to the storage briefly and as a result, the host considers that storage read-only to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read-only from the host's perspective even when the network connection is restored. A restart of the host solves the read-only state issue.

This issue can be observed with hosts that have incorrect MPIO settings. When MPIO is not configured correctly, the host loses connection to the storage, and might not be able to reconnect to the storage when the connectivity issue is resolved.

## Can I attach the {{site.data.keyword.blockstorageshort}} with a single path? Do I need to use multipath?
{: #singlepath}
{: faq}
{: support}

It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to ensure no disruption of service. For more information about configuring MPIO connections, see the following articles.
- [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)

## How can I configure and validate multipath connections to the {{site.data.keyword.blockstorageshort}} volume?
{: #correctMPIO}
{: faq}
{: support}

During a planned maintenance or an unplanned disruption, one of the routes is taken down. If MPIO is configured correctly, the host can still access the attached storage through the second path. For more information about the MPIO settings, see the following articles.
- [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verifying MPIO on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#verifyMPIOLinux)
- [Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
- [Verifying MPIO on MS Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows#verifyMPIOWindows)

On rare occasions, a LUN is provisioned and attached while the second path is down. In such instances, the host might see one single path when the discovery scan is run. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](/status?component=block-storage&selected=status){: external} to see whether an event might be impacting your host's ability to access the storage. If no events are reported, perform the discovery scan again to ensure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](/unifiedsupport/cases/add){: external} so it can be properly investigated.

## I expanded the volume size of my {{site.data.keyword.blockstorageshort}} through the Cloud console, but the size on my server is still the same. How do I fix it?
{: #expandsize}
{: faq}
{: support}

To see the new expanded LUN size, you need to rescan and reconfigure your existing {{site.data.keyword.blockstorageshort}} disk on the server. See the following examples. For more information, see your operating system Documentation. 

### Windows 2016
{: #expandsizeWin}

1. Go to Server Manager > Tools > Computer Management > Disk Management.
2. Click Action > Refresh.
3. Click Action > Rescan Disks. This process can take up to 5 minutes or more to finish. The additional capacity displays as a deallocated partition on the existing Disk.
4. Partition the deallocated space as you want. For more information, see [Microsoft - Extend a basic volume](https://learn.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.

### Linux
{: #expandsizeLin}

1. Log out of each multipath session of the block storage device that you expanded.
   ```sh
   # iscsiadm --mode node --portal <Target IP> --logout
   ```
   {: pre}

2. Log in again.
   ```sh
   # iscsiadm --mode node --portal <Target IP> --login
   ```
   {: pre}

3. Rescan the iscsi sessions.
   ```sh
   # iscsiadm -m session --rescan
   ```
   {: pre}

4. List the new size by using `fdisk -l` to confirm that the storage was expanded.

5. Reload multipath device map.
   ```sh
   # multipath -r <WWID>
   ```
   {: pre}

   ```sh
   # multipath -r 3600a09803830477039244e6b4a396b30
   reload: 3600a09803830477039244e6b4a396b30 undef NETAPP  ,LUN C-Mode
   size=30G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=undef
   |-+- policy='round-robin 0' prio=50 status=undef
   | `- 2:0:0:3 sda  8:0     active ready running
   `-+- policy='round-robin 0' prio=10 status=undef
   `- 4:0:0:3 sdd  8:48    active ready running
   ```

6. Expand the file system.
   - LVM
     1. Resize Physical Volume.
        ```sh
        # pvresize /dev/mapper/3600a09803830477039244e6b4a396b30
          Physical volume "/dev/mapper/3600a09803830477039244e6b4a396b30" changed
          1 physical volume(s) resized or updated / 0 physical volume(s) not resized

        # pvdisplay -m /dev/mapper/3600a09803830477039244e6b4a396b30
          --- Physical volume ---
          PV Name               /dev/mapper/3600a09803830477039244e6b4a396b30
          VG Name               vg00
          PV Size               <30.00 GiB / not usable 3.00 MiB
          Allocatable           yes
          PE Size               4.00 MiB
          Total PE              7679 - Changed  <- new number of physical extents
          Free PE               2560
          Allocated PE          5119
          PV UUID               dehWT5-VxgV-SJsb-ydyd-1Uck-JUA9-B9w0cO

          --- Physical Segments ---
          Physical extent 0 to 5118:
          Logical volume  /dev/vg00/vol_projects
          Logical extents 6399 to 11517
          Physical extent 5119 to 7678:
            FREE
        ```

     2. Resize Logical Volume.
        ```sh
        # lvextend -l +100%FREE -r /dev/vg00/vol_projects
          Size of logical volume vg00/vol_projects changed from 49.99 GiB (12798 extents) to 59.99 GiB (15358 extents).
          Logical volume vg00/vol_projects successfully resized.
          resize2fs 1.42.9 (28-Dec-2013)
          Filesystem at /dev/mapper/vg00-vol_projects is mounted on /projects; on-line resizing required
          old_desc_blocks = 7, new_desc_blocks = 8
          The filesystem on /dev/mapper/vg00-vol_projects is now 15726592 blocks long.

        # lvdisplay
          --- Logical volume ---
          LV Path                /dev/vg00/vol_projects
          LV Name                vol_projects
          VG Name                vg00
          LV UUID                z1lukZ-AuvR-zjLr-u1kK-eWcp-AHjX-IcnerW
          LV Write Access        read/write
          LV Creation host, time acs-kyungmo-lamp.tsstesting.com, 2021-12-07 19:34:39 -0600
          LV Status              available
          # open                 1
          LV Size                59.99 GiB <--- new logical volume size
          Current LE             15358
          Segments               4
          Allocation             inherit
          Read ahead sectors     auto
          - currently set to     8192
          Block device           253:2
        ```

     3. Verify the file system size.
        ```sh
        # df -Th /projects
        Filesystem                    Type  Size  Used Avail Use% Mounted on
        /dev/mapper/vg00-vol_projects ext4   59G  2.1G   55G   4% /projects
        ```

        For more information, see [RHEL 8 - Modifying Logical Volume](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external}.


   - Non-LVM - ext2, ext3, ext4:
      1. Extend the existing partition on the disk by using `growpart` and `xfs_progs` utilities. If you need to install them, run the following command.
         ```sh
         # yum install cloud-utils-growpart xfsprogs -y
         ```
         {: pre}

         1. Unmount the volume that you want to expand the partition on.
            ```sh
            # umount /dev/mapper/3600a098038304338415d4b4159487669p1
            ```

         2. Run the `growpart` utility. This action grows the specified partition regardless whether it's an ext2, ext3, ext, or xfsf file system.
            ```sh
            # growpart /dev/mapper/3600a098038304338415d4b4159487669 1
            CHANGED: partition=1 start=2048 old: size=146800640 end=146802688 new: size=209713119,end=209715167
            ```

         3. Run `partprobe` to reread the disk and its partitions, then run `lsblk` to verify the new extended partition size.
            ```sh
            # partprobe

            # lsblk
            NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
            sda 8:0 0 100G 0 disk
            ├─sda1 8:1 0 100G 0 part
            └─3600a098038304338415d4b4159487669 253:0 0 100G 0 mpath
            └─3600a098038304338415d4b4159487669p1 253:1 0 100G 0 part
            sdb 8:16 0 100G 0 disk
            └─3600a098038304338415d4b4159487669 253:0 0 100G 0 mpath
            └─3600a098038304338415d4b4159487669p1 253:1 0 100G 0 part
            xvda 202:0 0 100G 0 disk
            ├─xvda1 202:1 0 256M 0 part /boot
            └─xvda2 202:2 0 99.8G 0 part /
            xvdb 202:16 0 2G 0 disk
            └─xvdb1 202:17 0 2G 0 part [SWAP]
            ```

      2. Extend the existing file system on the partition.
         1. Unmount the partition.
            ```sh
            # umount /dev/mapper/3600a098038304338415d4b4159487669p1
            ```

         2. Run `e2fsck -f` to ensure that the file system is clean and has no issues before you proceed with resizing.
            ```sh
            # e2fsck -f /dev/mapper/3600a098038304338415d4b4159487669p1
            e2fsck 1.42.9 (28-Dec-2013)
            Pass 1: Checking inodes, blocks, and sizes
            Pass 2: Checking directory structure
            Pass 3: Checking directory connectivity
            Pass 4: Checking reference counts
            Pass 5: Checking group summary information
            /dev/mapper/3600a098038304338415d4b4159487669p1: 12/4587520 files (0.0% non-contiguous), 596201/18350080 blocks
            ```

         3. Issue the `resize2fs` command to resize the file system.
            ```sh
            # resize2fs /dev/mapper/3600a098038304338415d4b4159487669p1
            resize2fs 1.42.9 (28-Dec-2013)
            Resizing the filesystem on /dev/mapper/3600a098038304338415d4b4159487669p1 to 26214139 (4k) blocks.
            The filesystem on /dev/mapper/3600a098038304338415d4b4159487669p1 is now 26214139 blocks long.
            ```

         4. Mount the partition and run `df -vh` to verify that the new size is correct.
            ```sh
            # mount /dev/mapper/3600a098038304338415d4b4159487669p1 /SL02SEL1160157-73

            # df -vh
            Filesystem Size Used Avail Use% Mounted on
            /dev/xvda2 99G 3.7G 90G 4% /
            devtmpfs 3.9G 0 3.9G 0% /dev
            tmpfs 3.9G 1.7M 3.9G 1% /dev/shm
            tmpfs 3.9G 25M 3.8G 1% /run
            tmpfs 3.9G 0 3.9G 0% /sys/fs/cgroup
            /dev/xvda1 240M 148M 80M 65% /boot
            fsf-sjc0401b-fz.adn.networklayer.com:/SL02SV1160157_8/data01 40G 1.1G 39G 3% /SL02SV1160157_8
            tmpfs 782M 0 782M 0% /run/user/0 /dev/mapper/3600a098038304338415d4b4159487669p1 99G 1.1G 93G 2% /SL02SEL1160157-73
            ```

   - Non-LVM - xfs
      1.  Mount the xfs file system back to its mount point. See `/etc/fstab` if you're not sure what the old mount point is for the xfs partition.
          ```sh
          # mount /dev/sdb1 /mnt
          ```

      2. Extend the file system. Substitute the mount point of the file system.
         ```sh
         # xfs_growfs -d </mnt>
         ```

## Why do I see two disks in Disk Management when I add a single storage device?
{: #add-mpio}
{: faq}

Seeing two disks in Disk Management can occur if MPIO is not installed or is disabled for iSCSI. To verify the MPIO configuration, refer to the steps for [Verifying MPIO configuration for Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#verifyMPIOLinux) or [Verifying whether MPIO is configured correctly in Windows Operating systems](/docs/BlockStorage?topic=BlockStorage-mountingWindows#verifyMPIOWindows).

## How do I reconnect storage after a chassis swap?
{: #chassis-swap}
{: faq}

Complete the following steps to successfully reconnect the storage after a chassis swap.
1. Before the swap, remove the authorization (revoke access) from the storage devices.
2. After the swap, authorize the host again.
3. Discover the storage devices again, with the new credentials that were gained from the new authorization.

For more information, see [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage).

## How do I disconnect my storage device from a host?
{: #disconnect}
{: faq}

Perform the following steps to disconnect from a host:
1. Remove operating system iSCSI sessions and, if applicable, unmount the device.
1. Revoke access for the host from the storage device in the [{{site.data.keyword.cloud}} console](/login){: external}.
1. Remove automatic discovery, and if applicable, remove connect database entries from the operating system for iSCSI connections.

## How do endurance and performance storage differ?
{: #tier-options}
{: faq}

Endurance and Performance are provisioning options that you can select for storage devices. In short, Endurance IOPS tiers offer predefined performance levels whereas you can fine-tune those levels with the Performance tier. The same devices are used but delivered with different options. For more information, see [IBM Cloud Block Storage: Details](https://www.ibm.com/products/block-storage){: external}.

## I am unable to upgrade storage. What can affect the ability to upgrade or expand storage?
{: #expand-fail}
{: faq}

The following situations can affect the ability to upgrade or expand storage:
- If the original volume is the Endurance 0.25 tier, then the IOPS tier can't be updated.
- Older storage types can't be upgraded. For more information, see [expanding {{site.data.keyword.blockstorageshort}} Capacity](/docs/BlockStorage?topic=BlockStorage-expandingcapacity).
- The permissions that you have in the [{{site.data.keyword.cloud}} console](/login){: external} can be a factor. For more information, see the topics within [User roles and permissions](/docs/account?topic=account-userroles).

## Are iSCSI LUNs thin or thick provisioned?
{: #thin}
{: faq}

All File and {{site.data.keyword.blockstorageshort}} services are thin-provisioned. This method is not modifiable.

## My billing ID changed, what does this mean?
{: #staasV2migration}
{: faq}

You might notice that your Storage volumes are now billed as "Endurance Storage Service” or "Performance Storage Service" instead of "Enterprise Storage". You might also have new options in the console, such as the ability to adjust IOPS or increase capacity. {{site.data.keyword.cloud}} strives to continuously improve storage capabilities. As hardware gets upgraded in the data centers, storage volumes that reside in those data centers are also upgraded to use all enhanced features. The price that you pay for your Storage volume does not change with this upgrade.

## How durable is {{site.data.keyword.blockstorageshort}}?
{: #stordurabilityfaq}
{: faq}

When you store your data in {{site.data.keyword.blockstorageshort}}, it's durable, highly available, and encrypted. The durability target for a single Availability zone is 99.999999999% (11 9's). For more information, see [Availability and Durability of {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-storageavailability).

## What's the average uptime for {{site.data.keyword.blockstorageshort}}?
{: #storavailabilityfaq}
{: faq}

When you store your data in {{site.data.keyword.blockstorageshort}}, it's durable, highly available, and encrypted. {{site.data.keyword.blockstorageshort}} is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. To ensure that the availability target of 99.999% (five 9's) is met, the data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure, and also controller failure because the node can still access its partner's disks seamlessly. For more information, see [Availability and Durability of {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-storageavailability).

## How can I identify a {{site.data.keyword.blockstorageshort}} volume from my OS?
{: #identifyLUNfaq}
{: faq}

Various reasons exist for why you would want to look up the LUN ID of the attached storage volumes on the Compute host. For example, you might have multiple storage devices that are mounted on the same host with the same volume sizes. You want to detach and decommission one of them. However, you are not sure how to correlate what you see on your Linux&reg; host with what you see in the console. Another example might be that you have multiple {{site.data.keyword.blockstorageshort}} volumes that are attached to an ESXi server. You want to expand the volume size of one of the LUNs, and you need to know the correct LUN ID of the storage to do that. For OS-specific instructions, click one of the following links.

- [Viewing LUN information in Linux&reg;](/docs/BlockStorage?topic=BlockStorage-identifyLUN#identifyLUNLin)
- [Viewing LUN information in Windows](/docs/BlockStorage?topic=BlockStorage-identifyLUN#identifyLUNWin)
- [Viewing LUN information in VMWare&reg;](/docs/BlockStorage?topic=BlockStorage-identifyLUN#identifyLUNVMware)

## Can I get storage performance metrics (IOPS or latency) from the Support team?
{: #storagemetrics}
{: faq}

{{site.data.keyword.cloud}} does not provide storage performance IOPS and latency metrics. Customers are expected to monitor their own {{site.data.keyword.blockstorageshort}} devices by using their choice of third-party monitoring tools.

The following examples are utilities that you might consider to use to check performance statistics.
- [`sysstat`](https://github.com/sysstat/sysstat/blob/master/README.md){: external} - System performance tools for the Linux&reg; operating system.
- [`typeperf`](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/typeperf){: external} - Windows command that writes performance data to the command window or to a log file.
- [`esxtop`](https://community.broadcom.com/vmware-cloud-foundation/blogs/zhelong-pan/2024/04/14/interpreting-esxtop-statistics){: external} - A command-line tool that gives administrators real-time information about resource usage in a VMware&reg; vSphere environment. It can monitor and collect data for all system resources: CPU, memory, disk, and network.


## What is the difference between a replica volume, a dependent and an independent duplicate volume?
{: #replicavsduplicate}
{: faq}

You can create a replica or a duplicate volume by using a snapshot of your volume. Replication and cloning use one of your snapshots to copy data to a destination volume. However, that is where the similarities end.

Replication keeps your data in sync in two different locations. Only one volume of the pair (primary volume and replica volume) can be active at a time. The replication process automatically copies information from the active volume to the inactive volume based on the replication schedule. For more information about replica volumes, see [Replicating data](/docs/BlockStorage?topic=BlockStorage-replication).

Duplication creates a copy of your volume based on a snapshot in the same availability zone as the parent volume. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot. The duplicate volume can be dependent or independent from the original volume, and it can be manually refreshed with data from the parent volume. You can adjust the IOPS or increase the volume size of the duplicate without any effect on the parent volume.

- A dependent duplicate volume does not go through the conversion of becoming independent, and can be refreshed at any time after it is created. The system locks the original snapshot so that the snapshot cannot be deleted while the dependent duplicate exists. The parent volume cannot be canceled while the dependent duplicate volume exists. If you want to cancel the parent volume, you must either cancel the dependent duplicate first or convert it to an independent duplicate.

- An independent duplicate is superior to the dependent duplicate in most regards, but it cannot be refreshed immediately after creation because of the lengthy conversion process. It can take up to several hours based on the size of the volume. For example, it might take up to a day for a 12-TB volume. However, after the separation process is complete, the data can be manually refreshed by using another snapshot of the original parent volume.

For more information about duplicates, see [Creating and managing duplicate volumes](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).

| Feature | Replica | Dependent duplicate | Independent duplicate |
|---------|---------|---------------------|-----------------------|
| Created from a snapshot | ![Checkmark icon.](../../icons/checkmark-icon.svg) | ![Checkmark icon.](../../icons/checkmark-icon.svg) | ![Checkmark icon.](../../icons/checkmark-icon.svg) |
| Location of copied volume | Remote Availability Zone | Same Availability Zone   | Same Availability Zone |
| Supports failover  | ![Checkmark icon.](../../icons/checkmark-icon.svg) |  |  |
| Different Size and IOPS |          | ![Checkmark icon.](../../icons/checkmark-icon.svg) | ![Checkmark icon.](../../icons/checkmark-icon.svg) |
| Auto-synced with parent volume | ![Checkmark icon.](../../icons/checkmark-icon.svg) | |  |
| On-demand refresh from parent volume | | ![Checkmark icon.](../../icons/checkmark-icon.svg)[^depdup] | ![Checkmark icon.](../../icons/checkmark-icon.svg)[^indepdup] |
| Separated from parent volume | | | ![Checkmark icon.](../../icons/checkmark-icon.svg) |
{: caption="Table 1. Comparison of features between different types of volume copies. " caption-side="top"}
{: summary="This table has row and column headers. The row headers identify the capability. The column headers identify the type of volume copy."}
{: #table1}

[^depdup]: Dependent duplicates can be refreshed immediately after creation.

[^indepdup]: Independent duplicates can be refreshed after the separation process completes.

## How long does it take to convert a dependent duplicate into an independent volume?
{: #duplicateconversion}
{: faq}

The conversion process can take some time to complete. The bigger the volume, the longer it takes to convert it. For a 12-TB volume, it might take 24 hours. You can check on the progress in the UI or from the CLI.

- In the UI, go to [Classic Infrastructure](/gen1/infrastructure/devices){: external}. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, then locate the volume in the list. The conversion status is displayed on the Overview page.

- From the CLI, use the following command.
   ```sh
   slcli block duplicate-convert-status <dependent-vol-id>
   ```

   The output looks similar to the following example.
   ```sh
   slcli block duplicate-convert-status 370597202
   Username            Active Conversion Start Timestamp   Completed Percentage
   SL02SEVC307608_74   2022-06-13 14:59:17                 90
   ```

## Where can I find more information about Portable Storage?
{: #portablestorageredirect}
{: faq}

Portable storage volumes (PSVs) are an auxiliary storage solution exclusively for {{site.data.keyword.BluVirtServers_short}}. You can detach the PSV from one virtual server and attach it to another. You can connect a portable storage disk to one virtual server at a time while all information that is stored on the disk is retained for transfer between devices. For more information, see [Portable SAN storage](/docs/virtual-servers?topic=virtual-servers-storage-options#portable-san-storage){: external}.
