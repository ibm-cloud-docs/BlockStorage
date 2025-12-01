---

copyright:
  years: 2025
lastupdated: "2025-12-01"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL8, multipath, mpio, Linux, Red Hat Enterprise Linux 8

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 2h

---
{{site.data.keyword.attribute-definition-list}}

# Mount iSCSI volume on Red Hat Enterprise Linux 9
{: #mountingRHEL}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="2h"}

The following tutorial guides you through how to mount an {{site.data.keyword.blockstoragefull}} volume on a [virtual server](/docs/virtual-servers?topic=virtual-servers-getting-started-tutorial) with the Red Hat Enterprise Linux&reg; 9 operating system. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

If you're using another Linux&reg; operating system, refer to the Documentation of your specific distribution, and make sure that the multipath supports ALUA for path priority.
{: tip}

## Before you begin
{: #beforemountingRHEL9}

1. Make sure that the host that is to access the {{site.data.keyword.blockstorageshort}} volume is authorized. For more information, see [Authorizing the host in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}.

1. An active [VPN connection](/docs/iaas-vpn?topic=iaas-vpn-using-ssl-vpn) is required to access to the private network of IBM Cloud and to interact with several services.

1. Establish an SSH connection to your server. The IP address, username, and password can be found in the console. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), click **Infrastructure** ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure** > ** Devices**. Then, locate your server in the list and click its name to display its details.

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your OS Documentation.

## Update the OS and install the iSCSI and multipath utilities
{: #installutilsrhel9}
{: step}

Make sure that your system is updated and includes the `iscsi-initiator-utils` and `device-mapper-multipath` packages. Use the following commands to install the packages.

1. Update the OS.

   ```sh
   sudo dnf update
   ```
   {: pre}

1. Install multipath utility   

   ```sh
   sudo dnf -y install device-mapper-multipath
   ```
   {: pre}

   When the package is installed, enter `mpathconf --enable --user_friendly_names n` command to enable it.

1. Install ISCSI utility

   ```sh
   sudo dnf -y install iscsi-initiator-utils
   ```
   {: pre}

   When the package is installed, start it with the `systemctl start iscsid` command.

## Update iSCSI configuration files
{: #updateinitiatorrhel9}
{: step}

1. Update the `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. 
   1. The following example shows how to check the current initiator name.
   
     ```sh
     [root@vsi4classic ~]# cat /etc/iscsi/initiatorname.iscsi
     InitiatorName=iqn.1994-05.com.redhat:7acdadcdc20
     ```
     {: screen}

   1. Open the file in a text editor: 
     ```sh 
     vi /etc/iscsi/initiatorname.iscsi
     ```
     {: pre}

   1. Enter the IQN in the following format `InitiatorName=<value-from-the-Portal>`, as you can see in the following example. Replace the IQN value with your own.

     ```sh
     InitiatorName=iqn.2025-11.com.ibm:sl02su1414935-v154455886
     ```
     {: pre}

   1. Save the file (`:w`), and exit (`:x`).

1. Uncomment and update the following entries in `/etc/iscsi/iscsid.conf` by using the username and password from the {{site.data.keyword.cloud}} console. Use uppercase for CHAP names.

   ```text
   node.session.auth.authmethod = CHAP
   node.session.auth.username = <Username-value-from-Portal>
   node.session.auth.password = <Password-value-from-Portal>
   discovery.sendtargets.auth.authmethod = CHAP
   discovery.sendtargets.auth.username = <Username-value-from-Portal>
   discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   Don't change the other CHAP settings. {{site.data.keyword.cloud}} storage uses only one-way authentication. Do not enable Mutual CHAP.
   {: important}     

1. Restart the iscsid service.

   ```sh
   systemctl restart iscsid
   ```
   {: pre}

1. Validate that the configuration is correct by running discovery against the iSCSI array.
   ```sh
   iscsiadm -m discovery -t sendtargets -p TARGET IP
   ```
   {: pre}

   If the IP address information and access details are displayed, then the discovery is successful.

   ```sh
   [root@vsi4classic ~]# iscsiadm -m discovery -t sendtargets -p 161.26.99.113
    161.26.99.113:3260,1034 iqn.1992-08.com.netapp:stfdal1006
    161.26.99.110:3260,1030 iqn.1992-08.com.netapp:stfdal1006
   ```
   {: screen}

## Set up the multipath
{: #setupmultipathdrhel9}
{: step}

You set up DM Multipath with the `mpathconf` utility, which creates the multipath configuration file `/etc/multipath.conf`. For more information about the mpathconf utility, see the [mpathconf(8) man page](https://man.linuxreviews.org/man8/mpathconf.8.html){: external}.

1. If you didn't do it already, enter the mpathconf command with the `--enable` option.
   ```sh
   mpathconf --enable --user_friendly_names n
   ```
   {: pre}

2. Edit the `/etc/multipath.conf` file with `nano`, `vi`, or another editor of your choice. 

   ```sh
   sudo vi /etc/multipath.conf
   ```
   {: pre}

3. Add the following minimum configuration.

   ```sh
   defaults {
   user_friendly_names no
   find_multipaths on
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
   # All data in the following section must be specific to your system.
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
   features "2 pg_init_retries 50"
   no_path_retry queue
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
   {: screen}

   The initial defaults section of the configuration file configures your system so that the names of the multipath devices are of the form /dev/mapper/mpath n, where `mpath n` is the WWID of the device.

4. Save the configuration file and exit the editor.

5. Issue the following command.
   ```sh
   systemctl start multipathd.service
   ```
   {: pre}

   If you need to edit the multipath configuration file after you started the multipath daemon, you must issue the `systemctl reload multipathd.service` command for the changes to take effect.
   {: note}

## Discover the storage device and login
{: #discoverandloginrhel9}
{: step}

The iscsiadm utility is a command-line tool that is used for discovery and login to iSCSI targets, plus access and management of the open-iscsi database. For more information, see the [iscsiadm(8) man page](https://man.linuxreviews.org/man8/iscsiadm.8.html){: external}. In this step, discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

1. Run the discovery against the iSCSI array.
   ```sh
   iscsiadm -m discovery -t sendtargets -p TARGET IP
   ```
   {: pre}

   If the IP address information and access details are displayed, then the discovery is successful.

2. Log in to the iSCSI array.
   ```sh
   iscsiadm -m node --login
   ```
   {: pre}

   ```sh
   [root@vsi4classic ~]# iscsiadm -m node --login
   Login to [iface: default, target: iqn.1992-08.com.netapp:stfdal1006, portal: 161.26.99.110,3260] successful.
   Login to [iface: default, target: iqn.1992-08.com.netapp:stfdal1006, portal: 161.26.99.113,3260] successful.
   ```
   {: screen}

## Verifying configuration
{: #verifyconfigrhel9}
{: step}

1. Validate that the iSCSI session is established.
   ```sh
   iscsiadm -m session -o show
   ```
   {: pre}

2. Validate that multiple paths exist.
   ```sh
   multipath -l
   ```
   {: pre}


   This command reports the paths. If it is configured correctly, then each volume has a single group, with a number of paths equal to the number of iSCSI sessions. It's possible to attach a volume with a single path, but it is important that connections are established on both paths to ward against disruption of service.

   ```sh
   [root@vsi4classic ~]# multipath -l
   3600a0980383056716724514550666270 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=0 status=active
   | `- 2:0:0:0 sda 8:0  active undef running
   `-+- policy='round-robin 0' prio=0 status=enabled
     `- 3:0:0:0 sdb 8:16 active undef running
   ```
   {: screen}

   If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO provides an extra level of connectivity during those events, and keeps an established session to the volume with active read/write operations.

3. List the partition tables for the connected device.
   ```sh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   By default the storage device attaches to `/dev/mapper/<wwid>`. WWID is persistent while the volume exists. The command reports something similar to the following example.
   
   ```sh
   [root@vsi4classic ~]# fdisk -l | grep /dev/mapper
   Disk /dev/mapper/3600a0980383056716724514550666270: 20 GiB, 21474836480 bytes, 41943040 sectors
   ```
   {: screen}

   In the example, the string `3600a0980383056716724514550666270` is the WWID. Your application ought to use the WWID. It's also possible to assign easier-to-read names by using "user_friendly_names" or "alias" keywords in multipath.conf. For more information, see the [`multipath.conf` man page](https://man.linuxreviews.org/man5/multipath.conf.5.html){: external}.
   {: tip}

   The volume is now mounted and accessible on the host. You can create a file system next.

## Creating a file system (optional)
{: #createfilesysrhel9}
{: step}

Follow these steps to create a file system on the newly mounted volume. A file system is necessary for most applications to use the volume. Use [`fdisk` for drives that are less than 2 TB](#fdiskrhel) and [`parted` for a disk bigger than 2 TB](#partedrhel).

### Creating a file system with `fdisk`
{: #fdiskrhel9}

1. Get the disk name.
   ```sh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   ```sh
   [root@vsi4classic ~]# fdisk -l | grep /dev/mapper
   Disk /dev/mapper/3600a0980383056716724514550666270: 20 GiB, 21474836480 bytes, 41943040 sectors

   [root@vsi4classic ~]# fdisk -l /dev/mapper/3600a0980383056716724514550666270
   Disk /dev/mapper/3600a0980383056716724514550666270: 20 GiB, 21474836480 bytes, 41943040 sectors
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   ```
   {: screen}

2. Create a partition on the disk. The following example shows the creation of the partition with default values.

   ```sh
   [root@vsi4classic ~]# fdisk /dev/mapper/3600a0980383056716724514550666270

   Welcome to fdisk (util-linux 2.37.4).
   Changes will remain in memory only, until you decide to write them.
   Be careful before using the write command.

   Device does not contain a recognized partition table.
   Created a new DOS disklabel with disk identifier 0x2aa2e16c.

   Command (m for help): n
   Partition type
     p   primary (0 primary, 0 extended, 4 free)
     e   extended (container for logical partitions)
   Select (default p): p
   Partition number (1-4, default 1): 1
   First sector (2048-41943039, default 2048): 2048
   Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-41943039, default 41943039): 41943039

   Created a new partition 1 of type 'Linux' and of size 20 GiB.

   Command (m for help): w
   The partition table has been altered.
   Calling ioctl() to re-read partition table.
   Re-reading the partition table failed.: Invalid argument

   The kernel still uses the old table. The new table will be used at the next reboot or after you run partprobe(8) or partx(8).
   ```
   {: screen}
   
3. Create a file system on the new partition.

   - The new partition is listed with the disk, similar to `XXXp1`, followed by the size, Type (83), and Linux&reg;. Take a note of the partition name, you need it in the next step. (The XXXp1 represents the partition name.)
     ```sh
     [root@vsi4classic ~]# fdisk -l /dev/mapper/3600a0980383056716724514550666270
     Disk /dev/mapper/3600a0980383056716724514550666270: 20 GiB, 21474836480 bytes, 41943040 sectors
     Units: sectors of 1 * 512 = 512 bytes
     Sector size (logical/physical): 512 bytes / 4096 bytes
     I/O size (minimum/optimal): 4096 bytes / 65536 bytes
     Disklabel type: dos
     Disk identifier: 0x2aa2e16c

     Device                                           Boot Start      End  Sectors Size Id Type
     /dev/mapper/3600a0980383056716724514550666270p1        2048 41943039 41940992  20G 83 Linu
     ```
     {: screen}

   - Create the file system:

     ```sh
     mkfs.ext3 /dev/mapper/XXXp1
     ```
     {: pre}

     ```sh
     [root@vsi4classic ~]# mkfs.ext3 /dev/mapper/3600a0980383056716724514550666270p1
     mke2fs 1.46.5 (30-Dec-2021)
     Discarding device blocks: done                            
     Creating filesystem with 5242624 4k blocks and 1310720 inodes
     Filesystem UUID: 09e7f833-32b7-4c7f-ab68-8c6083cb8a63
     Superblock backups stored on blocks: 
	     32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	     4096000

     Allocating group tables: done                            
     Writing inode tables: done                            
     Creating journal (32768 blocks): done
     Writing superblocks and filesystem accounting information: done
     ```
     {: screen}

4. Create a mount point for the file system, and mount it.
   - Create a partition name `PerfDisk` or where you want to mount the file system.

     ```sh
     mkdir /PerfDisk
     ```
     {: pre}

   - Mount the storage with the partition name.
     ```sh
     mount /dev/mapper/XXXp1 /PerfDisk
     ```
     {: pre}

   - Check that you see your new file system listed.
     ```sh
     df -h
     ```
     {: pre}

     ```sh
     [root@vsi4classic ~]# mkdir /PerfDisk
     [root@vsi4classic ~]# mount /dev/mapper/3600a0980383056716724514550666270p1 /PerfDisk
     [root@vsi4classic ~]# df -h
     Filesystem                                       Size  Used Avail Use% Mounted on
     devtmpfs                                         4.0M     0  4.0M   0% /dev
     tmpfs                                            469M     0  469M   0% /dev/shm
     tmpfs                                            188M   11M  177M   6% /run
     /dev/xvda2                                        24G  2.0G   21G   9% /
     /dev/xvda1                                       974M  320M  588M  36% /boot
     tmpfs                                             94M     0   94M   0% /run/user/0
     /dev/mapper/3600a0980383056716724514550666270p1   20G  156K   19G   1% /PerfDisk
     ```
     {: screen}

5. To enable automatic mounting om boot, add the new file system to the system's `/etc/fstab` file.
   - Append the following line to the end of `/etc/fstab` (with the partition name from Step 3).

   ```sh
   /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
   ```
   {: pre}

   For more information, see [An introduction to the Linux `/etc/fstab` file](https://www.redhat.com/en/blog/etc-fstab){: external}.  

### Creating a file system with `parted`
{: #partedrhel9}

On Red Hat Enterprise Linux&reg; 9, `parted` comes preinstalled. However, if you need to, you can install it by issuing the following command.

```sh
dnf install parted
```
{: pre}

To create a file system with `parted`, follow these steps.

1. Start the interactive `parted` shell.

   ```sh
   parted
   ```
   {: pre}

2. Create a partition on the disk.
   1. Unless it is specified otherwise, the `parted` utility uses your primary drive, which is `/dev/sda` in most cases. Switch to the disk that you want to partition by using the command **select**. Replace **XXX** with your new device name.

      ```sh
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Run `print` to confirm that you are on the correct disk.

      ```sh
      print
      ```
      {: pre}

   3. Create a GPT partition table.

      ```sh
      mklabel gpt
      ```
      {: pre}

   4. `Parted` can be used to create primary and logical disk partitions, the steps that are involved are the same. To create a partition, the utility uses `mkpart`. You can give it other parameters like **primary** or **logical** depending on the partition type that you want to create.

       ```sh
       mkpart
       ```
       {: pre}

       The listed units default to megabytes (MB). To create a 10-GB partition, you start from 1 and end at 10000. You can also change the sizing units to terabytes by entering `unit TB` if you want to.
       {: tip}

   5. Exit `parted` with `quit`.

      ```sh
      quit
      ```
      {: pre}

3. Create a file system on the new partition.

   ```sh
   mkfs.ext3 /dev/mapper/XXXp1
   ```
   {: pre}

   It's important to select the correct disk and partition when you run this command. Verify the result by printing the partition table. Under the file system column, you can see ext3.
   {: important}

4. Create a mount point for the file system and mount it.
   - Create a partition name `PerfDisk` or where you want to mount the file system.

   ```sh
   mkdir /PerfDisk
   ```
   {: pre}

   - Mount the storage with the partition name.

   ```sh
   mount /dev/mapper/XXXp1 /PerfDisk
   ```
   {: pre}

   - Check that you see your new file system listed.

   ```sh
   df -h
   ```
   {: pre}

5. To enable automatic mounting om boot, add the new file system to the system's `/etc/fstab` file.
   - Append the following line to the end of `/etc/fstab` (by using the partition name from Step 3).

   ```sh
   /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
   ```
   {: pre}

   For more information, see [An introduction to the Linux `/etc/fstab` file](https://www.redhat.com/en/blog/etc-fstab){: external}.  

### Managing user permissions to the content of the mounted volume
{: #user-group-permissions-rhel9}

As a system administrator, you can manage the access to data on the mounted volume. After the file system is ready, you can refine access control by using the `chown` and `chmod` commands to assign read, write, and execute permissions to individual users and groups. For more information, see [Red Hat's tutorial: How to manage Linux permissions for users, groups, and others](https://www.redhat.com/en/blog/manage-permissions){: external}.

## Verifying MPIO configuration
{: #verifyMPIOLinux}

If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO provides an extra level of connectivity during those events, and keeps an established session to the volume with active read/write operations.

* To check whether multipath is picking up the devices, list the current configuration. If it is configured correctly, then a single group exists for each volume, with a number of paths equal to the number of iSCSI sessions.

   ```sh
   multipath -l
   ```
   {: pre}

   ```sh
   root@server:~# multipath -l
   3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode
   size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
   |-+- policy='round-robin 0' prio=-1 status=active
   | `6:0:0:101 sdd 8:48 active ready running
   `-+- policy='round-robin 0' prio=-1 status=enabled
    `- 7:0:0:101 sde 8:64 active ready running
   ```

   The string `3600a09803830304f3124457a45757067` in the example is the unique WWID of the volume. Each volume is identified by its unique WWID, which is persistent while the volume exists.

* Confirm that all the disks are present. In a correct configuration, you can expect two disks to show in the output with the same identifier, and a `/dev/mapper` listing of the same size with the same identifier. The `/dev/mapper` device is the one that multipath sets up.

   ```sh
   fdisk -l | grep Disk
   ```
   {: pre}

   - Example output of a correct configuration.

    ```sh
    root@server:~# fdisk -l | grep Disk
    Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

    The WWID is included in the device name that the multipath creates. The WWID is recommended to be used by your application.

   - Example output of an incorrect configuration. No `/dev/mapper` disk exists.

    ```sh
    root@server:~# fdisk -l | grep Disk
    Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

* To confirm that no local disks are included in the list of multipath devices, display the current configuration with verbosity level 3. The output of the following command displays the devices and also shows which ones were added to the blocklist.
   ```sh
   multipath -l -v 3 | grep sd <date and time>
   ```
   {: pre}

* If a volume is provisioned and attached while the second path is down, the host might see a single path when the discovery scan is run for the first time. If you encounter this rare phenomenon, check the [{{site.data.keyword.cloud}} status page](https://{DomainName}/status?component=block-storage&selected=status){: external} to see whether an event that impacts your host's ability to access the storage is in progress. If no events are reported, perform the discovery scan again to make sure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](https://{DomainName}/unifiedsupport/cases/add){: external} so it can be properly investigated.

## Unmounting {{site.data.keyword.blockstorageshort}} volumes
{: #unmountingLinrhel9}

1. Unmount the file system.
   ```sh
   umount /dev/mapper/XXXp1 /PerfDisk
   ```
   {: pre}

2. If you do not have any other volumes in that target portal, you can log out of the target.
   ```sh
   iscsiadm -m node -T <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. If you do not have any other volumes in that target portal, delete the target portal record to prevent future login attempts.
   ```sh
   iscsiadm -m node -o delete -T <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   For more information, see the [`iscsiadm` manual](https://man.linuxreviews.org/man8/iscsiadm.8.html){: external}.
   {: tip}
   
