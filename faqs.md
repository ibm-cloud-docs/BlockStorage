---

copyright:
  years: 2014, 2022
lastupdated: "2022-04-28"

keywords: Block Storage, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# FAQs
{: #block-storage-faqs}

## How many instances can share the use of a {{site.data.keyword.blockstorageshort}} volume?
{: #authlimit}
{: faq}
{: support}

The default limit for the number of authorizations per block volume is eight. This means that up to 8 hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} LUN. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware&reg; deployment can request the authorization limit to be increased to 64. To request a limit increase, contact your sales representative or raise a [Support case](https://{DomainName}/unifiedsupport/cases/add){: external}.

## Our compute hosts have multiple network cards with different IP addresses for network redundancy and expanded bandwidth. How can we authorize them all to access the same Storage volume?
{: #authsubnets}
{: faq}
{: support}

It is possible to authorize a subnet of IP addresses to access a specific {{site.data.keyword.blockstorageshort}} volume through the console, SLCLI, or API. To authorize a host to connect from multiple IP addresses on a subnet, complete the following steps.

### Console UI
{: #authinUI}

1. Go to [Classic Infrastructure](https://{DomainName}/classic/devices){: external}.
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the volume and click the ellipsis (**...**).
4. Click **Authorize Host**.
5. To see the list of available IP addresses, select **IP address** as the host type. Then, select the subnet where your host resides.
6. From the filtered list, select one or more IP addresses that can access the volume and click **Save**.

### SLCLI
{: #authinSLCLI}

```python
# slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```

## How many volumes can be ordered?
{: #orderlimit}
{: faq}
{: support}

By default, you can provision a combined total of 750 block and file storage. To increase your volume limit, contact your sales representative. For more information, see [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).

## How many {{site.data.keyword.blockstorageshort}} volumes can be mounted to a host?
{: #volumelimit}
{: faq}
{: support}

That depends on what the host operating system can handle, it’s not something that {{site.data.keyword.cloud}} limits. Refer to your OS documentation for limits on the number of volumes that can be mounted.

## Can I attach multiple LUNs with different OS settings?
{: #multiplelun}
{: faq}
{: support}

No. A host cannot be authorized to access LUNs of differing OS types at the same time. A host can be authorized to access LUNs of a **single** OS type. If you attempt to authorize a host to access multiple LUNs with different OS types, the operation results in an error.

## Which Windows version am I to choose for my {{site.data.keyword.blockstorageshort}} LUN?
{: #windowsOStypes}
{: faq}
{: support}

When you create a LUN, you must specify the OS type. The OS type must be based on the operating system, which is used by the hosts that access the LUN. The OS Type can't be modified after the LUN is created. The actual size of the LUN might vary slightly based on the OS type of the LUN.

### Windows&reg; GPT
{: #winGPT}

-  The LUN stores Windows&reg; data by using the GUID Partition Type (GPT) partitioning style. Use this option if you want to use the GPT partitioning method and your host can use it. Windows&reg; Server 2003, Service Pack 1 and later can use the GPT partitioning method, and all 64-bit versions of Windows&reg; support it.

### Windows&reg; 2003
{: #win2003}

- The LUN stores a raw disk type in a single-partition Windows&reg; disk that uses the Master Boot Record (MBR) partitioning style. Use this option only if your host operating system is Windows&reg; 2000 Server, Windows&reg; XP, or Windows&reg; Server 2003 that uses the MBR partitioning method.

### Windows&reg; 2008+
{: #win2008}

- The LUN stores Windows&reg; data for Windows&reg; 2008 and later versions. Use this OS option if your host operating system is Windows&reg; Server 2008, Windows&reg; Server 2012, Windows&reg; Server 2016. Both MBR and GPT partitioning methods are supported.


## Is the allocated IOPS limit enforced by instance or by volume?
{: #iopslimit}
{: faq}
{: support}

IOPS is enforced at the volume level. Said differently, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS.

## Measuring IOPS
{: #iopsmeasure}
{: faq}
{: support}

IOPS is measured based on a load profile of 16-KB blocks with random 50 percent read and 50 percent writes. Workloads that differ from this profile can experience inferior performance. To improve performance, you can try [adjusting the host queue depth settings](/docs/BlockStorage?topic=BlockStorage-hostqueuesettings) or [enabling Jumbo frames](/docs/BlockStorage?topic=FileStorage-jumboframes).

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

{{site.data.keyword.blockstorageshort}} is yours to format and manage the way you want to. {{site.data.keyword.cloud}} can't see the contents of the LUN, and therefore the UI can't provide information about the disk space usage. You can obtain more information about the volume, such as how much disk space is taken and how much is available, from your Compute host's operating system. 

You can use the following commands.
- Linux&reg;: 
   ```txt
   df -h
   ```
   {: pre}

   The command provides an output that shows how much space is available space and the percentage used.
   ```zsh
   $ df -hT /dev/sda1
   Filesystem     Type      Size  Used Avail Use% Mounted on
   /dev/sda1      disk      6.0G  1.2G  4.9G  20% /
   ```

- Windows&reg;:
   ```txt
   fsutil volume diskfree C:
   ```
   {: pre}

   or

   ```text
   dir C:
   ```
   {: pre}
   
   The last line of the output shows how much space is free.
   
   You can also view the free disk space in the File Explorer by clicking This PC.

## Does the volume need to be pre-warmed to achieve expected throughput?
{: #prewarm}
{: faq}
{: support}

There's no need for pre-warming. You can observe specified throughput immediately upon provisioning the volume.

## Can more throughput be achieved by using a faster Ethernet connection?
{: #ethernet}
{: faq}
{: support}

There are limits set at the LUN level and a faster Ethernet connection doesn't increase that limit. However, with a slower Ethernet connection, your bandwidth can be a potential bottleneck.

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
   * In Linux&reg; or Windows&reg;, create an 802.11q interface. Choose one of the unused secondary IP addresses from the newly trunked VLAN and assign that IP address, subnet mask, and gateway to the new 802.11q interface that you created.
   * In VMware&reg;, create a VMkernel network interface (vmk) and assign the unused secondary IP address, subnet mask, and gateway from the newly trunked VLAN to the new vmk interface.
5. Add a new persistent static route on the host to the target iSCSI subnet.
6. Ensure that the IP for the newly added interface is added to the host authorization list.
7. Perform discovery/target portal login as described in the following topics.
   - [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Mounting LUNs on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Mapping LUNS on Microsoft&reg; Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows)

## Should I run iSCSI traffic over 802.3ad LACP port channel?
{: #MPIOvsLACP}
{: faq}
{: support}

No. Link Aggregation Control Protocol (LACP) is not a recommended configuration with iSCSI. Use Multi-Path Input/Output (MPIO) framework for I/O balancing and redundancy.

With an MPIO configuration, a server with multiple NICs can transmit and receive I/O across all available interfaces to a corresponding MPIO-enabled storage device. This provides redundancy that can ensure that the storage traffic remains steady even if one of the paths becomes unavailable. If a server has two 1-Gb NICs and the storage server has two 1Gb NICs, the theoretical maximum throughput is about 200 MB/s. 

Link aggregation (such as LACP or 802.3ad) through NIC teaming does not work the same way as MPIO. Link aggregation does not improve the throughput of a single I/O flow, nor does it provide multiple paths. A single flow always traverses one single path. The benefit of link aggregation can be observed when several “unique” flows exist, and each flow comes from a different source. Each individual flow is sent down its own available NIC interface which is determined by a hash algorithm. Thus with more unique flows, more NICs can provide greater aggregate throughput.

Bonding works between a server and switch. However, MPIO works between a storage server and the client server, whether or not there is a switch in the path.

For more information, see one of the following articles.
- Redhat Linux&reg;: [Is using bonded nic interfaces recommended with iscsi?](https://access.redhat.com/solutions/41899){: external}
- Microsoft&reg; Windows&reg;: [Never run MPIO on top of NIC teaming](https://social.technet.microsoft.com/Forums/en-US/441d2157-119d-4b1e-b40c-1aa3670e44a6/nic-teaming-and-iscsi?forum=winserverhyperv){: external}
- VMware&reg;: [Host requirements for link aggregation](https://kb.vmware.com/s/article/1001938){: external} or [iSCSI and LAG/LACP](https://core.vmware.com/blog/iscsi-and-laglacp){: external}

## What latency can be expected from the {{site.data.keyword.blockstorageshort}}?   
{: #latency}  
{: faq}
{: support}

Target latency within the storage is <1 ms. The storage is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic during the operation.

## I ordered a {{site.data.keyword.blockstorageshort}} LUN in the wrong data center. Is it possible to move or migrate storage to another data center?
{: #movedatacenter}
{: faq}
{: support}

You need to order new {{site.data.keyword.blockstorageshort}} in the correct data center, and then cancel the {{site.data.keyword.blockstorageshort}} device that you ordered in the wrong location. When the volume is canceled, there's a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours. Billing for the volume stops immediately. When the reclaim period expires, the data is destroyed and the volume is removed from the console, too.

## Why can {{site.data.keyword.blockstorageshort}} with Endurance 10 IOPS/GB tier be ordered in some data centers and not in others?
{: #orderendurance}
{: faq}
{: support}

The 10 IOPS/GB tier of Endurance type {{site.data.keyword.blockstorageshort}} is available in most [data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

## How can we tell which {{site.data.keyword.blockstorageshort}} volumes are encrypted?
{: #volumeencrypt}
{: faq}
{: support}

When you look at your list of {{site.data.keyword.blockstorageshort}} in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/block){: external}, you can see a lock icon next to the volume name for the LUNs that are encrypted.

## How do we know when we're provisioning {{site.data.keyword.blockstorageshort}} in an upgraded data center?
{: faq}
{: #upgradedcenter}
{: support}

When you order {{site.data.keyword.blockstorageshort}}, all upgraded data centers are denoted with an asterisk (`*`) in the order form and an indication that you're about to provision storage with encryption. When the storage is provisioned, you can see an icon in the storage list that shows that storage as encrypted. All encrypted volumes and LUNs are provisioned in upgraded data centers only. You can find a full list of upgraded data centers and available features [here](/docs/BlockStorage?topic=BlockStorage-selectDC).

## If we own nonencrypted {{site.data.keyword.blockstorageshort}} in a data center that was recently upgraded, can we encrypt that {{site.data.keyword.blockstorageshort}}?
{: faq}
{: #encryptupgrade}
{: support}

{{site.data.keyword.blockstorageshort}} that is provisioned before the data center upgrade can't be encrypted.
New {{site.data.keyword.blockstorageshort}} that is provisioned in upgraded data centers is automatically encrypted. There's no encrypt setting to choose from, it’s automatic.
Data on nonencrypted storage in an upgraded data center can be encrypted by creating a LUN, then copying the data to the new encrypted LUN with host-based migration. For more information, see [Upgrading existing {{site.data.keyword.blockstorageshort}} to enhanced {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-migratestorage#migratestorage).

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

## I cannot cancel a {{site.data.keyword.blockstorageshort}} volume because the Cancel action in the Cloud console is unavailable. What’s happening?
{: faq}
{: #cancelstorage}

The cancellation process for this storage device is in progress so the Cancel action is no longer available. The volume remains visible for at least 24 hours until it’s reclaimed. An hourglass or clock icon appears next to the device name to indicate that it’s in a waiting period. The minimum 24-hour waiting period gives you a chance to void the cancel request if needed.

## My Windows 2012 host is supposed to have access to multiple Storage LUNs, but I can't see them in Disk Manager. How do I fix it?
{: faq}
{: #diskmanager}
{: support}

If you use more than two iSCSI LUNs with the same host, and if all the iSCSI connections are from the same Storage device, you might find that you can see only two devices in Disk Manager. When this happens, you need to manually connect to each device in the iSCSI Initiator. For more information, see [troubleshooting Windows&reg; 2012 R2 - multiple iSCSI devices](/docs/BlockStorage?topic=BlockStorage-troubleshootingWin12).

## My storage appears offline or read-only. Why did it happen and how do I fix it?
{: #StorageOffline}
{: faq}
{: support}

There are a couple of scenarios where a host (bare metal or VM) loses connection to the storage however briefly and as a result, the host considers that storage read-only to avoid data corruption. Most of the time the loss of connectivity is network-related but the status of the storage remains read-only from the host's perspective even when the network connection is restored. A reboot of the host solves the read-only state issue.

This issue can be observed with hosts that have incorrect MPIO settings. When MPIO is not configured correctly, the host loses connection to the storage and might not be able to reconnect to the storage when the connectivity issue is resolved.

## Can I attach the {{site.data.keyword.blockstorageshort}} with a single path? Do I have to use multipath?
{: #singlepath}
{: faq}
{: support}

It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to ensure no disruption of service. For more information about configuring MPIO connections, see the following articles.
- [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Mapping LUNS on Microsoft&reg; Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows)

## How can I configure and validate multipath connections to the {{site.data.keyword.blockstorageshort}} volume?
{: #correctMPIO}
{: faq}
{: support}

If MPIO is configured right, then when an unplanned disruption or a planned maintenance occurs, and one of the routes is taken down, the host can still access the attached storage through the second path. For more information about the MPIO settings, see the following articles.
- [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verifying MPIO on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#verifyMPIOLinux)
- [Mapping LUNS on Microsoft&reg; Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
- [Verifying MPIO on MS Windows&reg;](/docs/BlockStorage?topic=BlockStorage-mountingWindows#verifyMPIOWindows)

In the rare case of a LUN being provisioned and attached while the second path is down, when the discovery scan is run for the first time, the host might see a single path returned. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](https://{DomainName}/status?component=block-storage&selected=status){: external} to see whether there's an event that impacts your host's ability to access the storage. If no events are reported, perform the discovery scan again to ensure that all paths are properly discovered. If there's an event, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](https://{DomainName}/unifiedsupport/cases/add){: external} so it can be properly investigated.

## I expanded the volume size of my {{site.data.keyword.blockstorageshort}} through the Cloud console, but the size on my server is still the same. How do I fix it?
{: #expandsize}
{: faq}
{: support}

To see the new expanded LUN size, you need to rescan and reconfigure your existing {{site.data.keyword.blockstorageshort}} disk on the server. Check your operating system documentation for steps. Here are a couple of examples.

### Windows 2016
{: #expandsizeWin}

1. Go to Server Manager > Tools > Computer Management > Disk Management.
2. Click Action > Refresh.
3. Click Action > Rescan Disks. This can take up to 5 minutes or more to finish. The additional capacity displays as an Unallocated partition on the existing Disk. 
4. Partition the unallocated space as you want. For more information, see [Microsoft&reg; - Extend a basic volume](https://docs.microsoft.com/en-us/windows-server/storage/disk-management/extend-a-basic-volume){: external}.

### Linux
{: #expandsizeLin}

1. Log out of each multipath session of the block storage device that you expanded.
   ```zsh
   # iscsiadm --mode node --portal <Target IP> --logout
   ```
   {: pre}

2. Log in again.
   ```zsh
   # iscsiadm --mode node --portal <Target IP> --login
   ```
   {: pre}

3. Re-scan the iscsi sessions.
   ```zsh
   # iscsiadm -m session --rescan
   ```
   {: pre}

4. List the new size by using `fdisk -l` to confirm that the storage was expanded.

5. Reload multipath device map. 
   ```zsh
   # multipath -r <WWID>
   ```
   {: pre}

   ```zsh
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
        ```zsh
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
        ```zsh
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

     3. Verify file system size.
        ```zsh
        # df -Th /projects
        Filesystem                    Type  Size  Used Avail Use% Mounted on
        /dev/mapper/vg00-vol_projects ext4   59G  2.1G   55G   4% /projects
        ```

        For more information, see [RHEL 8 - Modifying Logical  Volume](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_and_managing_logical_volumes/modifying-the-size-of-a-logical-volume_configuring-and-managing-logical-volumes){: external}.

       
   - Non-LVM - ext2, ext3, ext4:
      1. Extend the existing partition on the disk by using `growpart` and `xfs_progs` utilities. If you don't have them installed already, run the following command.
         ```zsh
         # yum install cloud-utils-growpart xfsprogs -y
         ```
         {: pre}

         1. Unmount the volume  that you want to expand the partition on.
            ```zsh
            # umount /dev/mapper/3600a098038304338415d4b4159487669p1
            ```

         2. Run the `growpart` utility. This grows the partition specified regardless whether it's an ext2, ext3, ext, or xfsf filesystem.
            ```zsh
            # growpart /dev/mapper/3600a098038304338415d4b4159487669 1
            CHANGED: partition=1 start=2048 old: size=146800640 end=146802688 new: size=209713119,end=209715167
            ```

         3. Run `partprobe` to reread the disks and its partitions, then run `lsblk` to verify the new extended partition size.
            ```zsh
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

      2. Extend the existing filesystem on the partition.
         1. Unmount the partition. 
            ```zsh
            # umount /dev/mapper/3600a098038304338415d4b4159487669p1
            ```

         2. Run `e2fsck -f` to ensure the filesystem is clean and has no issues before you proceed with resizing.
            ```zsh
            # e2fsck -f /dev/mapper/3600a098038304338415d4b4159487669p1
            e2fsck 1.42.9 (28-Dec-2013)
            Pass 1: Checking inodes, blocks, and sizes
            Pass 2: Checking directory structure
            Pass 3: Checking directory connectivity
            Pass 4: Checking reference counts
            Pass 5: Checking group summary information
            /dev/mapper/3600a098038304338415d4b4159487669p1: 12/4587520 files (0.0% non-contiguous), 596201/18350080 blocks
            ```

         3. Issue the `resize2fs` command to resize the filesystem.
            ```zsh
            # resize2fs /dev/mapper/3600a098038304338415d4b4159487669p1
            resize2fs 1.42.9 (28-Dec-2013)
            Resizing the filesystem on /dev/mapper/3600a098038304338415d4b4159487669p1 to 26214139 (4k) blocks.
            The filesystem on /dev/mapper/3600a098038304338415d4b4159487669p1 is now 26214139 blocks long.
            ```

         4. Mount the partition and run `df -vh` to verify that the new size.
            ```zsh
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
      1.  Mount the xfs filesystem back to its mountpoint. See /etc/fstab if you're not sure what the old mountpoint is for the xfs partition.
          ```zsh
          # mount /dev/sdb1 /mnt
          ```
        
      2. Extend the filesystem. Substitute the mount point of the file system.
         ```zsh
         # xfs_growfs -d </mnt>
         ```

## Why do I see two disks in Disk Management when I add a single storage device?
{: #add-mpio}
{: faq}

Seeing two disks in Disk Management can occur if MPIO is not installed or is disabled for ISCSI. To verify the MPIO configuration, refer to the steps for [Verifying MPIO configuration for Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux#verifyMPIOLinux) or [Verifying whether MPIO is configured correctly in Windows&reg; Operating systems](/docs/BlockStorage?topic=BlockStorage-mountingWindows#verifyMPIOWindows).

## How do I reconnect storage after a chassis swap?
{: #chassis-swap}
{: faq}

Complete these tasks to connect storage after a swap:
1. Remove the authorization (revoke access) from the storage devices, and then authorize the host again.
1. Discover the storage devices again, with the new credentials that were gained from the new authorization.

For more information, see [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage).

## How do I disconnect my storage device from a host?
{: #disconnect}
{: faq}

Perform the following steps to disconnect from a host:
1. Remove operating system ISCSI sessions and, if applicable, unmount the device.
1. Revoke access for the host from the storage device in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/block){: external}.
1. Remove automatic discovery, and if applicable, remove connect database entries from the operating system for ISCSI connections.

## How do endurance and performance storage differ?
{: #tier-options}
{: faq}

Endurance and Performance are provisioning options that you can select for storage devices. In short, Endurance IOPS tiers offer predefined performance levels whereas you can fine-tune those levels with the Performance tier. The same devices are used but delivered with different options. For more information, see [IBM Cloud Block Storage: Details](https://www.ibm.com/cloud/block-storage/details){: external}.

## I am unable to upgrade storage. What can affect the ability to upgrade or expand storage?
{: #expand-fail}
{: faq}

The following situations can affect the ability to upgrade or expand storage:
- If the original volume is the Endurance 0.25 tier, then the IOPS tier can't be updated.
- Older storage types can't be upgraded. Ensure that the storage was ordered in an upgraded Data Center that allows for [Expanding {{site.data.keyword.blockstorageshort}} Capacity](/docs/BlockStorage?topic=BlockStorage-expandingcapacity).
- The permissions that you have in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/block){: external} can be a factor. For more information, see the topics within [User roles and permissions](/docs/account?topic=account-userroles).

## Are ISCSI LUNs thin or thick provisioned?
{: #thin}
{: faq}

All File and {{site.data.keyword.blockstorageshort}} services are thin-provisioned. This method is not modifiable.

## My billing ID changed, what does this mean?
{: #staasV2migration}
{: faq}

You might notice that your Storage volumes are now billed as "Endurance Storage Service” or "Performance Storage Service" instead of "Enterprise Storage", and you have new options in the console, such as the ability to adjust IOPS or increase capacity. {{site.data.keyword.cloud}} strives to continuously improve storage capabilities. As hardware gets upgraded in the datacenters, storage volumes that reside in those datacenters are also upgraded to utilize all enhanced features. The price that you pay for your Storage volume does not change with this upgrade.

## How durable is {{site.data.keyword.blockstorageshort}}?
{: #stordurabilityfaq}
{: faq}

When you store your data in {{site.data.keyword.blockstorageshort}}, it's durable, highly available, and encrypted. The durability target for a single Availability zone is 99.999999999% (11 9's). For more information, see [Availability and Durability of {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=FBlockStorage-storageavailability).

## What's the average uptime for {{site.data.keyword.blockstorageshort}}?
{: #storavailabilityfaq}
{: faq}

When you store your data in {{site.data.keyword.blockstorageshort}}, it's durable, highly available, and encrypted. {{site.data.keyword.blockstorageshort}} is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. To ensure that the availability target of 99.999% (five 9's) is met, the data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure, and also controller failure because the node can still access its partner's disks seamlessly. For more information, see [Availability and Durability of {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-storageavailability).

## How can I identify a {{site.data.keyword.blockstorageshort}} volume from my OS?
{: #identifyLUNfaq}
{: faq}

Various reasons exist for why you would want to look up the LUN ID of the attached storage volumes on the Compute host. For example, you might have multiple storage devices that are mounted on the same host with the same volume sizes and you want to detach and decommission one of them but you are not quite sure how to correlate what you see on your Linux&reg; host with what you see in the console. Another example could be that you have multiple {{site.data.keyword.blockstorageshort}} volumes that are attached to an esxi server and you want to expand the volume size of one of the LUNs, and you need to know the correct LUN ID of the storage that you want to expand to do that. For OS-specific instructions, click one of the following links.

- [Viewing LUN information in Linux&reg;](/docs/BlockStorage?topic=BlockStorage-identifyLUN#identifyLUNLin)
- [Viewing LUN information in Windows&reg;](/docs/BlockStorage?topic=BlockStorage-identifyLUN#identifyLUNWin)
- [Viewing LUN information in VMWare&reg;](/BlockStorage?topic=BlockStorage-identifyLUN#identifyLUNVMware)
