---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting to MPIO iSCSI LUNs on Linux

These instructions are for RHEL6/Centos6. We have added notes for other OS, but this documentation does **not** cover all Linux distributions. If you're using another Linux operating systems, refer to documentation of your specific distribution and ensure that the multipath supports ALUA for path priority. For example, you can find Ubuntu's instructions for iSCSI Initiator Configuration [here](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} and DM-Multipath setup [here](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.

Before starting, make sure the host accessing the {{site.data.keyword.blockstoragefull}} volume has been authorized through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. From the {{site.data.keyword.blockstorageshort}} listing page, click **Actions** associated with the new volume.
2. Click **Authorize Host**.
3. From the list, select the host or hosts that should be able to access the volume and click **Submit**.

## Mounting {{site.data.keyword.blockstorageshort}} volumes

Following are the steps required to connect a Linux-based {{site.data.keyword.BluSoftlayer_full}} Compute instance to a multipath input/output (MPIO) Internet Small Computer System Interface (iSCSI) logical unit number (LUN).

**Note:** The Host IQN, user name, password, and target address referenced in the instructions can be obtained from the **{{site.data.keyword.blockstorageshort}} Details** screen in the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Note:** We recommend running storage traffic on a VLAN which bypasses the firewall. Running storage traffic through software firewalls will increase latency and adversely affect storage performance.

1. Install the iSCSI and multipath utilities to your host:
   - RHEL/CentOS:

   ```
   yum install iscsi-initiator-utils device-mapper-multipath
   ```
   {: pre}

   - Ubuntu/Debian:

   ```
   sudo apt-get update
   sudo apt-get install multipath-tools
   ```
   {: pre}

2. Create or edit your multipath configuration file.
   - Edit **/etc/multipath.conf** with the minimum configuration provided in the following commands. <br /><br /> **Note:** Be aware that for RHEL7/CentOS7, `multipath.conf` can be blank as the OS has built-in configurations. Ubuntu doesn't use multipath.conf since it's built into multipath-tools.

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
   {: codeblock}

3. Load the multipath module, start multipath services and set it start on boot.
   - RHEL 6:
     ```
     modprobe dm-multipath
     ```
     {: pre}

     ```
     service multipathd start
     ```
     {: pre}

     ```
     chkconfig multipathd on
     ```
     {: pre}

   - CentOS 7:
     ```
     modprobe dm-multipath
     ```
     {: pre}

     ```
     systemctl start multipathd
     ```
     {: pre}

     ```
     systemctl enable multipathd
     ```
     {: pre}

   - Ubuntu:
     ```
     service multipath-tools start
     ```
     {: pre}

   - For other distributions, please consult the OS vendor documentation.

4. Verify multipath is working.
   - RHEL 6:
     ```
     multipath -l
     ```
     {: pre}

     If it returns blank at this time it is working.

   - CentOS 7:
     ```
     multipath -ll
     ```
     {: pre}

     RHEL 7/CentOS 7 may return No fc_host device, which can be ignored.

5. Update **/etc/iscsi/initiatorname.iscsi** file with the IQN from the {{site.data.keyword.slportal}}. Enter the value as lower case.
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. Edit the CHAP settings in **/etc/iscsi/iscsid.conf** using the username and password from the {{site.data.keyword.slportal}}. Use upper case for CHAP names.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   **Note:** Leave the other CHAP settings commented. {{site.data.keyword.BluSoftlayer_full}} storage uses only one-way authentication.

7. Set iSCSI to start at boot and start it at this time.
   - RHEL 6:

      ```
      chkconfig iscsi on
      ```
      {: pre}
      ```
      chkconfig iscsid on
      ```
      {: pre}

      ```
      service iscsi start
      ```
      {: pre}

      ```
      service iscsid start
      ```
      {: pre}

   - CentOS 7:

      ```
      systemctl enable iscsi
      ```
      {: pre}

      ```
      systemctl enable iscsid
      ```
      {: pre}

      ```
      systemctl start iscsi
      ```
      {: pre}

      ```
      systemctl start iscsid
      ```
      {: pre}

   - Other distributions: please consult the OS vendor documentation.

8. Discover the device using the Target IP address obtained from the {{site.data.keyword.slportal}}.

     a. Run the discovery against the iSCSI array:
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     b. Set the host to automatically log into the iSCSI array:
     ```
     iscsiadm -m node -L automatic
     ```
     {: pre}

9. Verify that the host has logged into the iSCSI array and maintained its sessions.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   This should report the paths at this time.

10. Verify the device is connected.  By default the device will attach to /dev/mapper/mpathX where X is the generated ID of the connected device.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Should report something similar to the following,
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 byte
    ```
  The volume is now mounted and accessible on the host.

## Create a file system (optional)

Following are the steps to create a file system on top of the newly mounted volume. A file system is necessary for most applications to utilize the volume. Use **fdisk** for drives that are less than 2 TB and **parted** for a disk bigger than 2 TB.

### fdisk

1. Get the disk name.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   The disk name returned should be similar to /dev/mapper/XXX.

2. Create a partition on the disk using fdisk.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   The XXX represents the disk name returned in Step 1. <br />

   **Note**: Scroll further down for the commands codes listed in the Fdisk command table under this section.

3. Create a file system on the new partition.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - The new partition should be listed with the disk, similar to XXXlp1, followed by the size, Type (83), and Linux.
   - Take a note of the partition name, you will need it in the next step. (The XXXlp1 represents the partition name.)
   - Create the file system:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Create a mount point for the file system and mount it.
   - create a partition name PerfDisk or where you want to mount the file system:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Using the partition name mount the storage:
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Check that you see your new file system listed:
     ```
     df -h
     ```
     {: pre}

5. Add the new filesystem to the system's **/etc/fstab** file to enable automatic mounting on boot.
   - Append the following line to the bottom of **/etc/fstab** (using the partition name from Step 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Fdisk command table
<table border="0" cellpadding="0" cellspacing="0">
 <tbody>
	<tr>
		<th style="width:40%;"><div>Command</div></th>
		<th style="width:60%;">Result</th>
	</tr>
	<tr>
		<td><li>&#42; <code>Command: n</code></li></td>
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
		<td><li>&#42; <code>Command: t</code></li></td>
		<td>Sets up the type of partition.</td>
	</tr>
	<tr>
		<td><li><code>Select partition 1.</code></li></td>
		<td>Selects partition 1 to be set up as a specific type.</td>
	</tr>
	<tr>
		<td><li>&#42;&#42; <code>Hex code: 83</code></li></td>
		<td>Selects Linux as the Type (83 is the hex code for Linux).</td>
	 </tr>
	<tr>
		<td><li>&#42; <code>Command: w</code></li></td>
		<td>Writes the new partition information to the disk.</td>
	</tr>
 </tbody>
</table>

  (`*`)Type m for Help.

  (`**`)Type L to list the hex codes

### parted

On many Linux distributions, **parted** comes pre-installed. If it is not included in your distro, you can install it with:
- Debian/Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL/CentOS:
  ```
  yum install parted  
  ```
  {: pre}


To create a file system with **parted** follow these steps:

1. Run parted.

   ```
   parted
   ```
   {: pre}

2. Create a partition on the disk.
   1. Unless specified otherwise, parted will use your primary drive, which in most cases is **/dev/sda**. Switch to the disk you want to partition using the command **select**. Replace **XXX** with your new device name.

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. Run **print** to confirm you are on the right disk.

      ```
      (parted) print
      ```
      {: pre}

   3. Create a new GPT partition table

      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. Parted can be used to create primary and logical disk partitions, the steps involved are the same. To create new partition, parted uses **mkpart**. You can give it additional parameters like **primary** or **logical** depending on the partition type that you wish to create.
   <br /> **Note**: The listed units default to megabytes (MB), to create a 10 GB partition you should start from 1 and end at 10000. You can also change the sizing units to terabytes by entering `(parted) unit TB` if you want to.

      ```
      (parted) mkpart
      ```
      {: pre}

   5. Exit parted with **quit**.

      ```
      (parted) quit
      ```
      {: pre}

3. Create a file system on the new partition.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **Note**: It’s important to select the right disk and partition when executing the above command!
   Verify the result by printing the partition table. Under file system column, you should see ext3.

4. Create a mount point for the file system and mount it.
   - create a partition name PerfDisk or where you want to mount the file system:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Using the partition name mount the storage:

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Check that you see your new file system listed:

     ```
     df -h
     ```
     {: pre}

5. Add the new filesystem to the system's **/etc/fstab** file to enable automatic mounting on boot.
   - Append the following line to the bottom of **/etc/fstab** (using the partition name from Step 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## How to Verify if MPIO is Configured Correctly in *NIX OSes

To check if multipath is picking up the devices, only the NETAPP devices should show up and there should be two of them.

```
# multipath -l
```
{: pre}

```
root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
7:0:0:101 sde 8:64 active undef running
```

Check that the disks are present, and there should be two disks with the same identifier, and a /dev/mapper listing of the same size with the same identifier. The /dev/mapper device is the one that multipath sets up:

```
# fdisk -l | grep Disk
```
{: pre}

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
# multipath -l -v 3 | grep sd <date and time>
```
{: pre}

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
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```
