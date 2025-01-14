---

copyright:
  years: 2021, 2025
lastupdated: "2025-01-14"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL8, multipath, mpio, Linux, Red Hat Enterprise Linux 8

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mount iSCSI LUN on Red Hat Enterprise Linux 8
{: #mountingRHEL8}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

This tutorial guides you through how to mount an {{site.data.keyword.blockstoragefull}} volume on a server with the Red Hat Enterprise Linux&reg; 8 operating system. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

If you're using another Linux&reg; operating system, refer to the Documentation of your specific distribution, and make sure that the multipath supports ALUA for path priority.
{: tip}

## Before you begin
{: #beforemountingRHEL8}

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

Before you begin, make sure that the host that is to access the {{site.data.keyword.blockstorageshort}} volume is authorized. For more information, see [Authorizing the host in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}.
{: requirement}

## Install the iSCSI and multipath utilities
{: #installutils}
{: step}

Make sure that your system is updated and includes the `iscsi-initiator-utils` and `device-mapper-multipath` packages. Use the following command to install the packages.

```sh
sudo dnf -y install iscsi-initiator-utils device-mapper-multipath
```
{: pre}

## Set up the multipath
{: #setupmultipathd}
{: step}

You set up DM Multipath with the `mpathconf` utility, which creates the multipath configuration file `/etc/multipath.conf`.

* If the /etc/multipath.conf file exists, the mpathconf utility can edit it.
* If the /etc/multipath.conf file does not exist, the mpathconf utility creates the /etc/multipath.conf file from scratch.

For more information about the mpathconf utility, see the [mpathconf(8) man page](https://linux.die.net/man/8/mpathconf){: external}.

1. Enter the mpathconf command with the `--enable` option.
   ```sh
   # mpathconf --enable --user_friendly_names n
   ```
   {: pre}

2. Edit the `/etc/multipath.conf` file with the following minimum configuration.

   ```sh
   defaults {
   user_friendly_names no
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

3. Save the configuration file and exit the editor, if necessary.
4. Issue the following command.
   ```sh
   systemctl start multipathd.service
   ```
   {: pre}

   If you need to edit the multipath configuration file after you started the multipath daemon, you must issue the `systemctl reload multipathd.service` command for the changes to take effect.
   {: note}

   For more information about using the Device Mapper Multipath feature on RHEL 8, see [Configuring the device mapper multipath](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/8/pdf/configuring_device_mapper_multipath/red_hat_enterprise_linux-8-configuring_device_mapper_multipath-en-us.pdf){: external}.

## Update /etc/iscsi/initiatorname.iscsi file
{: #updateinitiator}
{: step}

Update the `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. Enter the value as lowercase.

```sh
InitiatorName=<value-from-the-Portal>
```
{: pre}

## Configure credentials
{: #configcred}
{: step}

Edit the following settings in `/etc/iscsi/iscsid.conf` by using the username and password from the {{site.data.keyword.cloud}} console. Use uppercase for CHAP names.

```text
node.session.auth.authmethod = CHAP
node.session.auth.username = <Username-value-from-Portal>
node.session.auth.password = <Password-value-from-Portal>
discovery.sendtargets.auth.authmethod = CHAP
discovery.sendtargets.auth.username = <Username-value-from-Portal>
discovery.sendtargets.auth.password = <Password-value-from-Portal>
```
{: codeblock}

Leave the other CHAP settings commented. {{site.data.keyword.cloud}} storage uses only one-way authentication. Do not enable Mutual CHAP.
{: important}

## Discover the storage device and login
{: #discoverandlogin}
{: step}

The iscsiadm utility is a command-line tool that is used for discovery and login to iSCSI targets, plus access and management of the open-iscsi database. For more information, see the [iscsiadm(8) man page](https://linux.die.net/man/8/iscsiadm){: external}. In this step, discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

1. Run the discovery against the iSCSI array.
   ```sh
   iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
   ```
   {: pre}

   If the IP address information and access details are displayed, then the discovery is successful.

2. Log in to the iSCSI array.
   ```sh
   iscsiadm -m node --login
   ```
   {: pre}

## Verifying configuration
{: #verifyconfigrhel8}
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

   This command reports the paths. If it is configured correctly, then each volume has a single group, with a number of paths equal to the number of iSCSI sessions. It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to make sure that no disruption of service occurs.

   If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO provides an extra level of connectivity during those events, and keeps an established session to the LUN with active read/write operations.

3. List the partition tables for the connected device.
   ```sh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   By default the storage device attaches to `/dev/mapper/<wwid>`. WWID is persistent while the volume exists. The command reports something similar to the following example.
   ```sh
   Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
   ```

   In the example, the string `3600a0980383030523424457a4a695266` is the WWID. Your application ought to use the WWID. It's also possible to assign more easier-to-read names by using "user_friendly_names" or "alias" keywords in multipath.conf. For more information, see the [`multipath.conf` man page](https://linux.die.net/man/5/multipath.conf){: external}.
   {: tip}

   The volume is now mounted and accessible on the host. You can create a file system next.

## Creating a file system (optional)
{: #createfilesys}
{: step}

Follow these steps to create a file system on the newly mounted volume. A file system is necessary for most applications to use the volume. Use [`fdisk` for drives that are less than 2 TB](#fdiskrhel) and [`parted` for a disk bigger than 2 TB](#partedrhel).

### Creating a file system with `fdisk`
{: #fdiskrhel}

1. Get the disk name.
   ```sh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   The disk name that is returned looks similar to `/dev/mapper/XXX`.

2. Create a partition on the disk.

   ```sh
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   The XXX represents the disk name that is returned in Step 1.

   Scroll further down for the command codes that are listed in the `fdisk` command table.
   {: tip}

3. Create a file system on the new partition.

   ```sh
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - The new partition is listed with the disk, similar to `XXXp1`, followed by the size, Type (83), and Linux&reg;.
   - Take a note of the partition name, you need it in the next step. (The XXXp1 represents the partition name.)
   - Create the file system:

     ```sh
     mkfs.ext3 /dev/mapper/XXXp1
     ```
     {: pre}

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

5. To enable automatic mounting om boot, add the new file system to the system's `/etc/fstab` file.
   - Append the following line to the end of `/etc/fstab` (with the partition name from Step 3).

     ```sh
     /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

### Creating a file system with `parted`
{: #partedrhel}

On many Linux&reg; distributions, `parted` comes preinstalled. However, if you need to you can install it by issuing the following command.
```sh
# dnf install parted
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

## Verifying MPIO configuration
{: #verifyMPIOLinux}

If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO provides an extra level of connectivity during those events, and keeps an established session to the LUN with active read/write operations.

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

   The string `3600a09803830304f3124457a45757067` in the example is the unique WWID of the LUN. Each volume is identified by its unique WWID, which is persistent while the volume exists.

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

* If a LUN is provisioned and attached while the second path is down, the host might see a single path when the discovery scan is run for the first time. If you encounter this rare phenomenon, check the [{{site.data.keyword.cloud}} status page](https://{DomainName}/status?component=block-storage&selected=status){: external} to see whether an event that impacts your host's ability to access the storage is in progress. If no events are reported, perform the discovery scan again to make sure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](https://{DomainName}/unifiedsupport/cases/add){: external} so it can be properly investigated.

## Unmounting {{site.data.keyword.blockstorageshort}} volumes
{: #unmountingLin}

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

   For more information, see the [`iscsiadm` manual](https://linux.die.net/man/8/iscsiadm){: external}.
   {: tip}
   
