---

copyright:
  years: 2014, 2023
lastupdated: "2023-03-20"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL6, multipath, mpio, Linux,

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}


# Connecting to iSCSI LUNs on Linux
{: #mountingLinux}

These instructions are mainly for RHEL6 and CentOS6. Notes for other OS were added, but this documentation does **not** cover all Linux&reg; distributions. If you're using another Linux&reg; operating systems, refer to the documentation of your specific distribution, and ensure that the multipath supports ALUA for path priority.
{: note}

For more information about Ubuntu specifics, see [iSCSI Initiator Configuration](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){: external} and [DM-Multipath](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){: external}.
{: tip}

Before you start, make sure the host that is accessing the {{site.data.keyword.blockstoragefull}} volume is authorized correctly.
{: requirement}

## Authorizing the host
{: #authhostlin}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device types or subnets.
   - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
   - If you choose IP address, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that are supposed to access the volume and click **Save**.

Alternatively, you can authorize the host through the SLCLI.
```sh
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```
{: codeblock}

```sh
# slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```
{: codeblock}

When your host is authorized, take note of the following information, which is needed later.
* iSCSI Target IP addresses
* Username
* Password
* IQN

Bear in mind that if multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

## Authorizing the host with Terraform
{: #authhostclin8Cterraform}
{: terraform}

You authorize a compute host to access the volume, use the "ibm_storage_block" resource and specify the `allowed_virtual_guest_ids` for virtual servers, `allowed_hardware_ids` for bare metal servers. Specify `allowed_ip_addresses` to define which IP addresses have access to the storage. 

The following example defines that the virtual server with the ID `27699397` can access the volume from the `10.40.98.193`, `10.40.98.200` addresses.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal09"
        capacity = 40
        iops = 4
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        snapshot_capacity = 10
        hourly_billing = true
}
```
{: codeblock}

After your storage resource is created, you can access the `allowed_host_info` attribute which contains the user name, password, and host IQN of the hosts that are needed later.

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

Bear in mind that if multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

## Mounting {{site.data.keyword.blockstorageshort}} volumes
{: #mountLin}

Complete the following steps to connect a Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a multipath input/output (MPIO) iSCSI storage volume. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

The Host IQN, username, password, and target address that are referenced in the instructions can be obtained from the **{{site.data.keyword.blockstorageshort}} Detail** screen in the [{{site.data.keyword.cloud}} console](/login){: external}.
{: tip}

1. Install the iSCSI and multipath utilities to your host.
   - RHEL and CentOS

    ```sh
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

   - Ubuntu and Debian

    ```sh
    sudo apt-get update
    sudo apt-get install multipath-tools
    ```
    {: pre}

2. Create or edit your multipath configuration file if it is needed.
   - **RHEL** 6 and **CENTOS 6**
      * Edit **/etc/multipath.conf** with the following minimum configuration.

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
      {: pre}

     - Restart `iscsi` and `iscsid` services so that the changes take effect.

      ```sh
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

   - **RHEL7** and **CentOS7**, edit `multipath.conf` with the following minimum configuration.
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
      devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]"
      devnode "^cciss."
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
      {: pre}

   - **Ubuntu** has multipath configuration that is built into `multipath-tools`. However, the built-in configuration uses a "service-time 0" load-balancing policy, which can leave your connection vulnerable to interruptions. Create a multipath.conf file and update it as follows.

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
      devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]"
      devnode "^cciss."
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
      {: pre}

      - Restart `multipathd` service so that the changes take effect.

        ```sh
        systemctl multipathd restart
        ```
        {: pre}

3. Load the multipath module, start multipath services, and set it start on boot.
   - RHEL 6
     ```sh
     modprobe dm-multipath
     ```
     {: pre}

     ```sh
     service multipathd start
     ```
     {: pre}

     ```sh
     chkconfig multipathd on
     ```
     {: pre}

   - CentOS 7
     ```sh
     modprobe dm-multipath
     ```
     {: pre}

     ```sh
     systemctl start multipathd
     ```
     {: pre}

     ```sh
     systemctl enable multipathd
     ```
     {: pre}

   - Ubuntu
     ```sh
     service multipath-tools start
     ```
     {: pre}

   - For other distributions, check the OS vendor documentation.

4. Verify that multipath is working.
   - RHEL 6
     ```sh
     multipath -l
     ```
     {: pre}

     If it returns blank, it's working.
   - CentOS 7
     ```sh
     multipath -ll
     ```
     {: pre}

     RHEL 7 and CentOS 7 might return No fc_host device, which can be ignored.

5. Update `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. Enter the value as lowercase.

   ```sh
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}

6. Update the credential settings in `/etc/iscsi/iscsid.conf` by using the username and password from the {{site.data.keyword.cloud}} console. Use uppercase for CHAP names.
   ```sh
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

   Ubuntu users, while you are looking at the `iscsid.conf` file, check whether the `node.startup` setting is manual or automatic. If it's manual, change it to automatic.
   {: tip}

7. Set iSCSI to start at boot and start it now.
   - RHEL 6
     ```sh
     chkconfig iscsi on
     ```
     {: pre}

     ```sh
     chkconfig iscsid on
     ```
     {: pre}

     ```sh
     service iscsi start
     ```
     {: pre}

     ```sh
     service iscsid start
     ```
     {: pre}

   - CentOS 7
     ```sh
     systemctl enable iscsi
     ```
     {: pre}

     ```sh
     systemctl enable iscsid
     ```
     {: pre}

     ```sh
     systemctl restart iscsi
     ```
     {: pre}

     ```sh
     systemctl restart iscsid
     ```
     {: pre}

   - For other distributions, check the OS vendor documentation.

8. Discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

   A. Run the discovery against the iSCSI array.
     ```sh
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
     ```
     {: pre}

   B. Log in the host to the iSCSI array.
     ```sh
     iscsiadm -m node -L automatic
     ```
     {: pre}

9. Verify that the host is logged in to the iSCSI array and maintained its sessions.
   ```sh
   iscsiadm -m session
   ```
   {: pre}

   ```sh
   multipath -l
   ```
   {: pre}

   This command reports the paths. It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to ensure no disruption of service.

10. Verify that the device is connected by issuing the following command.

   ```sh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   By default the device attaches to `/dev/mapper/<wwid>`. WWID is the generated worldwide ID of the connected storage device that is persistent while the volume exists. So that command reports something similar to the following example.
   ```sh
   Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
   ```

   In the example, `3600a0980383030523424457a4a695266` is the WWID. Your application ought to use the WWID. It's also possible to assign more easier-to-read names by using "user_friendly_names" or "alias" keywords in multipath.conf. For more information, see the [`multipath.conf` man page](https://manpages.debian.org/unstable/multipath-tools/multipath.conf.5.en.html){: external}.
   {: tip}

The volume is now mounted and accessible on the host. You can create a file system next.

## Creating a file system (optional)
{: #optionalcreatefilesystem}

Follow these steps to create a file system on the newly mounted volume. A file system is necessary for most applications to use the volume. Use [`fdisk` for drives that are less than 2 TB](#fdisk) and [`parted` for a disk bigger than 2 TB](#parted).

### Creating a file system with `fdisk`
{: #fdisk}

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

5. Add the new file system to the system's `/etc/fstab` file to enable automatic mounting on boot.
   - Append the following line to the end of `/etc/fstab` (with the partition name from Step 3).

     ```sh
     /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

For more information about available command options, see [fdisk - manipulate disk partition table](https://manpages.ubuntu.com/manpages/xenial/man8/fdisk.8.html){: external}.

### Creating a file system with `parted`
{: #parted}

On many Linux&reg; distributions, `parted` comes preinstalled. If it isn't included in your distro, you can install it with:
- Debian and Ubuntu
   ```sh
   sudo apt-get install parted
   ```
   {: pre}

- RHEL and CentOS
   ```sh
   yum install parted
   ```
   {: pre}


To create a file system with `parted`, follow these steps.

1. Run `parted`.

   ```sh
   parted
   ```
   {: pre}

2. Create a partition on the disk.
   1. Unless it is specified otherwise, `parted` uses your primary drive, which is `/dev/sda` in most cases. Switch to the disk that you want to partition by using the command **select**. Replace **XXX** with your new device name.

      ```sh
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Run `print` to confirm that you are on the right disk.

      ```sh
      print
      ```
      {: pre}

   3. Create a GPT partition table.

      ```sh
      mklabel gpt
      ```
      {: pre}

   4. `Parted` can be used to create primary and logical disk partitions, the steps that are involved are the same. To create a partition, `parted` uses `mkpart`. You can give it other parameters like **primary** or **logical** depending on the partition type that you want to create.
     The listed units default to megabytes (MB). To create a 10-GB partition, you start from 1 and end at 10000. You can also change the sizing units to terabytes by entering `unit TB` if you want to.
     {: tip}

      ```sh
      mkpart
      ```
      {: pre}

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

   It's important to select the right disk and partition when you run this command.
   Verify the result by printing the partition table. Under file system column, you can see ext3.
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

5. Add the new file system to the system's `/etc/fstab` file to enable automatic mounting on boot.
   - Append the following line to the end of `/etc/fstab` (by using the partition name from Step 3).

     ```sh
     /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}


## Verifying MPIO configuration
{: #verifyMPIOLinux}

If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO ensures an extra level of connectivity during those events, and keeps an established session to the LUN with active read/write operations.

* To check whether multipath is picking up the devices, list the current configuration. If it is configured correctly, then each volume has a single group, with a number of paths equal to the number of iSCSI sessions.

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

   - The following example output shows a correct configuration.

    ```sh
    root@server:~# fdisk -l | grep Disk
    Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

    The WWID is included in the device name that the multipath creates.

   - The following example output shows incorrect configuration. The `/dev/mapper` disk does not exist.

    ```sh
    root@server:~# fdisk -l | grep Disk
    Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
    Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

* To confirm that no local disks are included in the list multipath devices, display the current configuration with verbosity level 3. The output of the following command displays the devices and also shows which ones were added to the blocklist.
   ```sh
   multipath -l -v 3 | grep sd <date and time>
   ```
   {: pre}

* On very rare occasions, a LUN is provisioned and attached while the second path is down. In such instances, the host might see one single path when the discovery scan is run. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](/status?component=block-storage&selected=status){: external} to see whether a current event might impact your host's ability to access the storage. If no events are reported, perform the discovery scan again to ensure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](/unifiedsupport/cases/add){: external} so it can be properly investigated.


## Unmounting {{site.data.keyword.blockstorageshort}} volumes
{: #unmountingLin}

1. Unmount the file system.
   ```sh
   umount /dev/mapper/XXXp1 /PerfDisk
   ```
   {: pre}

2. If you do not have any other volumes in that target portal, you can log out of the target.
   ```sh
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. If you do not have any other volumes in that target portal, delete the target portal record to prevent future login attempts.
   ```sh
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   For more information, see the [`iscsiadm` manual](https://linux.die.net/man/8/iscsiadm){: external}.
   {: tip}
