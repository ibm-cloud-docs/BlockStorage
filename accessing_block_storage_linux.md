---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-08"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Connecting to MPIO iSCSI LUNs on Linux

These instructions are for RHEL6/Centos6. If you are using another Linux operating systems, please refer to documentation of your specific distro for configuration and ensure that the multipath supports ALUA for path priority.

Before starting, make sure the host accessing the {{site.data.keyword.blockstoragefull}}  volume has been authorized through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. From the {{site.data.keyword.blockstorageshort}}  listing page, click the **Actions** associated with the newly provisioned volume 
2. Click Authorize Host.
3. Select the desired host(s) from the list and click **Submit**; this authorizes the host(s) to access the volume.

## Mounting {{site.data.keyword.blockstorageshort}} volumes

Following are the steps required to connect a Linux-based {{site.data.keyword.BluSoftlayer_full}} Compute instance to a multipath input/output (MPIO) Internet Small Computer System Interface (iSCSI) logical unit number (LUN). The example is based on Red Hat Enterprise Linux 6. The steps can be adjusted for other Linux distributions according to the operating system (OS) vendor documentation.

**Note:** The Host IQN, username, password, and target address referenced in the instructions can be obtained from the **{{site.data.keyword.blockstorageshort}} Details** screen in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Note:** We recommend running storage traffic on a vlan which bypasses the firewall as a best practice. Running storage traffic through software firewalls will increase latency and adversely affect storage performance.

1. Install the iSCSI and multipath utilities to your host
   - `yum install iscsi-initiator-utils device-mapper-multipath` (RHEL/CentOS)
   - `apt-get install open-iscsi multipath-tools` (Ubuntu/Debian)
2. Create or edit your multipath configuration file.
   - Edit `/etc/multipath.conf` with the minimum configuration provided in the following commands.

   **Be aware that for RHEL7/CentOS7, `multipath.conf` can be blank as the OS has built-in configurations.**

   ```
   defaults {
   user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
   # All data under blacklist must be specific to your system.
   blacklist {
   wwid "SAdaptec*"
   devnode "^hd[a-z]"
   devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]*"
   devnode "^cciss.*"  
   }
   devices {
   device {
   vendor "NETAPP"
   product "LUN"
   path_grouping_policy group_by_prio
   features "3 queue_if_no_path pg_init_retries 50"
   prio "alua"
   path_checker tur
   failback immediate
   path_selector "round-robin 0"
   hardware_handler "1 alua"
   rr_weight uniform
   rr_min_io 128
   }
   }
   ```

3. Load the multipath module, start multipath services and set it start on boot.
   - `modprobe dm-multipath`
   - `service multipathd start`
   - `chkconfig multipathd on`
4. Verify multipath is working.
   - `multipath -l` (if it returns blank at this time it is working. RHEL 7/CentOS 7 may return No fc_host device for 'host-1', which can be ignored. )
5. Update **/etc/iscsi/initiatorname.iscsi** file with the IQN from the {{site.data.keyword.slportal}}. Enter the value as lower case. 
   ```
   InitiatorName=<value-from-the-Portal> 
   ```
6. Edit the CHAP settings in /etc/iscsi/iscsid.conf using the username and password from the {{site.data.keyword.slportal}} (minus the quotation marks). Use upper case for CHAP names.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   **Note:** Leave the other CHAP settings commented. {{site.data.keyword.BluSoftlayer_full}} storage uses only one-way authentication.
          
7. Set iSCSI to start at boot and start it at this time.
   - `chkconfig iscsi on`
   - `chkconfig iscsid on`
   - `service iscsi start`
   - `service iscsid start`
8. Discover the device using the Target IP address obtained from the {{site.data.keyword.slportal}}.
    a. Run the discovery against the iSCSI array:
    `iscsiadm -m discovery -t sendtargets -p "ip-value-from-SL-Portal"`
       
    b. Set the host to automatically log into the iSCSI array:
    `iscsiadm -m node -L automatic`
       
9. Verify that the host has logged into the iSCSI array and maintained its sessions.
   - `iscsiadm -m session`
   - `multipath -l` (should report the paths at this time)
10. Verify the device is connected.  By default the device will attach to /dev/mapper/mpathX where X is the generated ID of the connected device.
    - `fdisk -l | grep /dev/mapper`
  Should report something similar to the following,
    - `Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 byte`
    
  The volume is now mounted and accessible on the host.

## Create a file system (optional) 

Following are the steps to create a file system on top of the newly mounted volume. A file system is necessary for most applications to utilize the volume.

1. Get the disk name.
   - `fdisk -l | grep /dev/mapper` (the disk name returned will be similar to /dev/mapper/XXX).
2. Create a partition on the disk using fdisk.
   - `fdisk /dev/mapper/XXX` (where XXX represents the disk name returned in Step 1).
   - Follow the commands codes listed in the Fdisk command table at the bottom of this page.
3. Create a file system on the new partition.
   - `fdisk –l /dev/mapper/XXX`
   - The new partition should be listed with the disk, similar to XXXlp1, followed by the size, Type (83), and Linux. 
   Take a note of the partition name; you’ll need it in the next step.
   - `mkfs.ext3 /dev/mapper/XXXlp1` (where XXXlp1represents the partition name is that was returned in the previous step).
4. Create a mount point for the file system and mount it.
   - `mkdir /PerfDisk` (or where you want to mount the file system).
   - `mount /dev/mapper/XXXlp1 /PerfDisk` (using the partition name from Step 3).
   - `df -h` (you should see your new file system listed).

### Fdisk command table
<table border="0" cellpadding="0" cellspacing="0">
 <tbody>
	<tr>
		<td style="width:40%;"><div>Command</div></td>
		<td style="width:60%;">Result</td>
	</tr>
	<tr>
		<td><li><code>* Command: n</code></li>	</td>
		<td>Creates a new partition.</td>
	</tr>
	<tr>
		<td><li><code>Command action: p</code></li></td>
		<td>Makes the partition the primary one.</td>
	</tr>
	<tr>
		<td><li><code>Partition number (1-4): 1</code></li></td>
		<td>Becomes partition 1 on the disk.</td>
	</tr>
	<tr>
		<td><li><code>First cylinder (1-8877): 1 (default)</code></li></td>
		<td>Start at cylinder 1.</td>
	</tr>
	<tr>
		<td><li><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></li></td>
		<td>Hit Enter to go to the last cylinder.</td>
	</tr>
	<tr>
		<td colspan="2"><div>&nbsp;</div></td>
	</tr>
	<tr>
		<td><li><code>* Command: t</code></li></td>
		<td>Sets up the type of partition.</td>
	</tr>
	<tr>
		<td><li><code>Select partition 1.</code></li></td>
		<td>Selects partition 1 to be set up as a specific type.</td>
	</tr>
	<tr>
		<td><li><code>** Hex code: 83</code></li></td>
		<td>Selects Linux as the Type (83 is the hex code for Linux).</td>
	</tr>
	<tr>
		<td colspan="2"><div>&nbsp;</div></td>
	</tr>
	<tr>
		<td><li><code>* Command: w</code></li></td>
		<td>Writes the new partition information to the disk.</td>
	</tr>
 </tbody>
</table>
  
  (`*`)Type m for Help.

  (`**`)Type L to list the hex codes
  
## How to Verify if MPIO is Configured Correctly in *NIX OSes

To check if multipath is picking up the devices, only the NETAPP devices should show up and there should be two of them.

```
root@server:~# multipath -l 
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw 
|-+- policy='round-robin 0' prio=-1 status=active`
6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
7:0:0:101 sde 8:64 active undef running
```

Check that the disks are present, and there should be two disks with the same identifier, and a /dev/mapper listing of the same size with the same identifier. The /dev/mapper device is the one that multipath sets up:

```
root@server:~# fdisk -l | grep Disk 
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d 
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1 
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1 
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```

If it is not correctly setup, it will look like this:

```
No multipath output root@server:~# multipath -l root@server:~#
```

This will show the devices blacklisted:

```
root@server:~# multipath -l -v 3 | grep sd Feb 17 19:55:02 
| sda: device node name blacklisted Feb 17 19:55:02 
| sdb: device node name blacklisted Feb 17 19:55:02 
| sdc: device node name blacklisted Feb 17 19:55:02 
| sdd: device node name blacklisted Feb 17 19:55:02 
| sde: device node name blacklisted Feb 17 19:55:02 
```

fdisk shows only the `sd*` devices, and no `/dev/mapper`

```
root@server:~# fdisk -l | grep Disk 
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d 
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1 
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```
