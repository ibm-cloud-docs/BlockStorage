---

copyright:
  years: 2014, 2019
lastupdated: "2019-11-21"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL6, multipath, mpio, linux,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}


# Connecting to iSCSI LUNs on Linux
{: #mountingLinux}

These instructions are mainly for RHEL6 and CentOS6. Notes for other OS were added, but this documentation does **not** cover all Linux distributions. If you're using another Linux operating systems, refer to the documentation of your specific distribution, and ensure that the multipath supports ALUA for path priority.
{:note}

For example, for more information about Ubuntu specifics, see [iSCSI Initiator Configuration](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){: external} and [DM-Multipath](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){: external}.
{: tip}

Before you start, make sure the host that is accessing the {{site.data.keyword.blockstoragefull}} volume was previously authorized through the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}.
{:important}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}. From the **menu**, select **Classic Infrastructure**.
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume, and click the ellipsis (**...**).
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device type or subnets.
   - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
   - If you choose IP address, first, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that can access the volume and click **Save**.

Alternatively, you can authorize the host through the SLCLI.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```
{:codeblock}

## Mounting {{site.data.keyword.blockstorageshort}} volumes
{: #mountLin}

Complete the following steps to connect a Linux-based {{site.data.keyword.cloud}} Compute instance to a multipath input/output (MPIO) iSCSI storage volume. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{:shortdesc}

The Host IQN, user name, password, and target address that are referenced in the instructions can be obtained from the **{{site.data.keyword.blockstorageshort}} Detail** screen in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic/storage/block){: external}.
{: tip}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.
{:important}

1. Install the iSCSI and multipath utilities to your host.
  - RHEL and CentOS

    ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu and Debian

    ```
    sudo apt-get update
    sudo apt-get install multipath-tools
    ```
    {: pre}

2. Create or edit your multipath configuration file if it is needed.
  - RHEL 6 and CENTOS 6
    * Edit **/etc/multipath.conf** with the following minimum configuration.

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

    - Restart `iscsi` and `iscsid` services so that the changes take effect.

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - RHEL7 and CentOS7, `multipath.conf` can be blank as the OS has built-in configurations.
  - Ubuntu doesn't use `multipath.conf` because it's built into `multipath-tools`.

3. Load the multipath module, start multipath services, and set it start on boot.
  - RHEL 6
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

  - CentOS 7
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

  - Ubuntu
    ```
    service multipath-tools start
    ```
    {: pre}

  - For other distributions, check the OS vendor documentation.

4. Verify that multipath is working.
  - RHEL 6
    ```
    multipath -l
    ```
    {: pre}

    If it returns blank, it's working.
  - CentOS 7
    ```
    multipath -ll
    ```
    {: pre}

    RHEL 7 and CentOS 7 might return No fc_host device, which can be ignored.

5. Update `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. Enter the value as lowercase.

   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}

6. Edit the CHAP settings in `/etc/iscsi/iscsid.conf` by using the user name and password from the {{site.data.keyword.cloud}} console. Use uppercase for CHAP names.
   ```
   node.session.auth.authmethod = CHAP
   node.session.auth.username = <Username-value-from-Portal>
   node.session.auth.password = <Password-value-from-Portal>
   discovery.sendtargets.auth.authmethod = CHAP
   discovery.sendtargets.auth.username = <Username-value-from-Portal>
   discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   Leave the other CHAP settings commented. {{site.data.keyword.cloud}} storage uses only one-way authentication. Do not enable Mutual CHAP.
   {:important}

   Ubuntu users, while you are looking at the `iscsid.conf` file, check whether the `node.startup` setting is manual or automatic. If it's manual, change it to automatic.
   {:tip}

7. Set iSCSI to start at boot and start it now.
  - RHEL 6
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

  - CentOS 7
    ```
    systemctl enable iscsi
    ```
    {: pre}

    ```
    systemctl enable iscsid
    ```
    {: pre}

    ```
    systemctl restart iscsi
    ```
    {: pre}

    ```
    systemctl restart iscsid
    ```
    {: pre}

   - For other distributions, check the OS vendor documentation.

8. Discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

   A. Run the discovery against the iSCSI array.
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
    ```
    {: pre}

   B. Log in the host to the iSCSI array.
    ```
    iscsiadm -m node -L automatic
    ```
    {: pre}

9. Verify that the host is logged in to the iSCSI array and maintained its sessions.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   This command reports the paths.

10. Verify that the device is connected. By default the device attaches to `/dev/mapper/mpathX` where X is the generated ID of the connected device.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}

  This command reports something similar to the following example.
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```

  The volume is now mounted and accessible on the host.

## Creating a file system (optional)

Follow these steps to create a file system on the newly mounted volume. A file system is necessary for most applications to use the volume. Use [`fdisk` for drives that are less than 2 TB](#fdisk) and [`parted` for a disk bigger than 2 TB](#parted).

### Creating a file system with `fdisk`
{: #fdisk}

1. Get the disk name.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   The disk name that is returned looks similar to `/dev/mapper/XXX`.

2. Create a partition on the disk.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   The XXX represents the disk name that is returned in Step 1. <br />

   Scroll further down for the commands codes that are listed in the `fdisk` command table.
   {: tip}

3. Create a file system on the new partition.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - The new partition is listed with the disk, similar to `XXXp1`, followed by the size, Type (83), and Linux.
   - Take a note of the partition name, you need it in the next step. (The XXXp1 represents the partition name.)
   - Create the file system:

     ```
     mkfs.ext3 /dev/mapper/XXXp1
     ```
     {: pre}

4. Create a mount point for the file system, and mount it.
   - Create a partition name `PerfDisk` or where you want to mount the file system.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Mount the storage with the partition name.
     ```
     mount /dev/mapper/XXXp1 /PerfDisk
     ```
     {: pre}

   - Check that you see your new file system listed.
     ```
     df -h
     ```
     {: pre}

5. Add the new file system to the system's `/etc/fstab` file to enable automatic mounting on boot.
   - Append the following line to the end of `/etc/fstab` (with the partition name from Step 3). <br />

     ```
     /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### The  `fdisk` command table

| Command | Result |
|-----|-----|
| `Command: n`| Creates a partition. * |
| `Command action: p` | Makes the partition the primary one. |
| `Partition number (1-4): 1` | Becomes partition 1 on the disk. |
| `First cylinder (1-8877): 1 (default)` | Start at cylinder 1. |
| `Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)` | Press Enter to go to the last cylinder. |
| `Command: t` | Sets up the type of partition. * |
| `Select partition 1.` | Selects partition 1 to be set up as a specific type. |
| `Hex code: 83` | Selects Linux as the Type (83 is the hex code for Linux). ** |
| `Command: w` | Writes the new partition information to the disk. ** |
{: caption="Table 1 - The <code>fdisk</code> command table contains commands on the left and expected results on the right." caption-side="top"}

(`*`) Type m for Help.

(`**`) Type L to list the hex codes.

### Creating a file system with `parted`
{: #parted}

On many Linux distributions, `parted` comes preinstalled. If it isn't included in your distro, you can install it with:
- Debian and Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL and CentOS
  ```
  yum install parted  
  ```
  {: pre}


To create a file system with `parted`, follow these steps.

1. Run `parted`.

   ```
   parted
   ```
   {: pre}

2. Create a partition on the disk.
   1. Unless it is specified otherwise, `parted` uses your primary drive, which is `/dev/sda` in most cases. Switch to the disk that you want to partition by using the command **select**. Replace **XXX** with your new device name.

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Run `print` to confirm that you are on the right disk.

      ```
      print
      ```
      {: pre}

   3. Create a GPT partition table.

      ```
      mklabel gpt
      ```
      {: pre}

   4. `Parted` can be used to create primary and logical disk partitions, the steps that are involved are the same. To create a partition, `parted` uses `mkpart`. You can give it other parameters like **primary** or **logical** depending on the partition type that you want to create.<br />

   The listed units default to megabytes (MB). To create a 10-GB partition, you start from 1 and end at 10000. You can also change the sizing units to terabytes by entering `unit TB` if you want to.
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. Exit `parted` with `quit`.

      ```
      quit
      ```
      {: pre}

3. Create a file system on the new partition.

   ```
   mkfs.ext3 /dev/mapper/XXXp1
   ```
   {: pre}

   It's important to select the right disk and partition when you run this command.<br />Verify the result by printing the partition table. Under file system column, you can see ext3.
   {:important}

4. Create a mount point for the file system and mount it.
   - Create a partition name `PerfDisk` or where you want to mount the file system.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Mount the storage with the partition name.

     ```
     mount /dev/mapper/XXXp1 /PerfDisk
     ```
     {: pre}

   - Check that you see your new file system listed.

     ```
     df -h
     ```
     {: pre}

5. Add the new file system to the system's `/etc/fstab` file to enable automatic mounting on boot.
   - Append the following line to the end of `/etc/fstab` (by using the partition name from Step 3). <br />

     ```
     /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## Verifying MPIO configuration
{: #verifyMPIOLinux}

1. To check whether multipath is picking up the devices, list the devices. If it is configured correctly, only two NETAPP devices show up.

  ```
  multipath -l
  ```
  {: pre}

  ```
  root@server:~# multipath -l
  3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
  |-+- policy='round-robin 0' prio=-1 status=active`
  6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
  7:0:0:101 sde 8:64 active undef running
  ```

2. Check that the disks are present. Expect to see two disks with the same identifier, and a `/dev/mapper` listing of the same size with the same identifier. The `/dev/mapper` device is the one that multipath sets up.
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - Example output of a correct configuration.

    ```
    root@server:~# fdisk -l | grep Disk
    Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - Example outputs of an incorrect configuration.

    ```
    No multipath output root@server:~# multipath -l root@server:~#
    ```

    ```
    root@server:~# fdisk -l | grep Disk
    Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. Confirm that local discs are not included in the multipath devices. The following command shows the devices that are blacklisted.
   ```
   multipath -l -v 3 | grep sd <date and time>
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

## Unmounting {{site.data.keyword.blockstorageshort}} volumes
{: #unmountingLin}

1. Unmount the file system.
   ```
   umount /dev/mapper/XXXp1 /PerfDisk
   ```
   {: pre}

2. If you do not have any other volumes in that target portal, you can log out of the target.
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. If you do not have any other volumes in that target portal, delete the target portal record to prevent future login attempts.
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   For more information, see the [`iscsiadm` manual](https://linux.die.net/man/8/iscsiadm).
   {:tip}
