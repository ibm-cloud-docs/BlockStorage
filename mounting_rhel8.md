---

copyright:
  years: 2021, 2022
lastupdated: "2022-12-03"

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

This tutorial guides you through how to mount a {{site.data.keyword.blockstoragefull}} volume on a server with the Red Hat Enterprise Linux&reg; 8 operating system. Complete the following steps to connect a Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a multipath input/output (MPIO) iSCSI storage volume. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

Before you begin, make sure the host that is accessing the {{site.data.keyword.blockstorageshort}} volume is authorized correctly.
{: important}

## Authorizing the host in the UI
{: #authhostrhelUI}
{: ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device types or subnets.
    - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
    - If you choose IP address, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that are supposed to access the volume and click **Save**.

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

## Authorizing the host from the SLCLI
{: #authhostrhelCLI}
{: cli}

Use the following command to authorize the host from the SLCLI.

```python
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

```python
# slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```
{: codeblock}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

## Install the iSCSI and multipath utilities
{: #installutils}
{: step}

Ensure that your system is updated and includes the `iscsi-initiator-utils` and `device-mapper-multipath` packages. Use the following command to install the packages.

```zsh
sudo dnf -y install iscsi-initiator-utils device-mapper-multipath
```
{: pre}

## Set up the multipath
{: #setupmultipathd}
{: step}

You set up DM Multipath with the `mpathconf` utility, which creates the multipath configuration file ``/etc/multipath.conf`.

* If the /etc/multipath.conf file already exists, the mpathconf utility can edit it.
* If the /etc/multipath.conf file does not exist, the mpathconf utility creates the /etc/multipath.conf file from scratch.

For more information on the mpathconf utility, see the [mpathconf(8) man page](https://linux.die.net/man/8/mpathconf){: external}.

1. Enter the mpathconf command with the --enable option specified:
   ```zsh
   # mpathconf --enable --user_friendly_names n
   ```
   {: pre}

2. Edit the /etc/multipath.conf file with the following minimum configuration.

   ```zsh
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
   {: pre}

   The initial defaults section of the configuration file configures your system so that the names of the multipath devices are of the form /dev/mapper/mpath n; where mpath n is the WWID of the device.

3. Save the configuration file and exit the editor, if necessary.
4. Execute the following command:
   ```zsh
   # systemctl start multipathd.service
   ```

   If you need to edit the multipath configuration file after you have started the multipath daemon, you must execute the `systemctl reload multipathd.service` command for the changes to take effect.
   {: note}

   For more information about using the Device Mapper Multipath feature on RHEL 8, see [Configuring device mapper multipath](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/pdf/configuring_device_mapper_multipath/Red_Hat_Enterprise_Linux-8-Configuring_device_mapper_multipath-en-US.pdf){: external}.

## Update /etc/iscsi/initiatorname.iscsi file
{: #updateinitiator}
{: step}

Update `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. Enter the value as lowercase.

```zsh
InitiatorName=<value-from-the-Portal>
```
{: pre}

## Configure credentials
{: #configcred}
{: step}

Edit the following settings in `/etc/iscsi/iscsid.conf` by using the user name and password from the {{site.data.keyword.cloud}} console. Use uppercase for CHAP names.

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

The iscsiadm utility is a command-line tool allowing discovery and login to iSCSI targets, as well as access and management of the open-iscsi database. For more information, see the [iscsiadm(8) man page](https://linux.die.net/man/8/iscsiadm){: external}. In this step, discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

1. Run the discovery against the iSCSI array.
   ```zhs
   iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
   ```
   {: pre}

   If the IP info and access details are displayed, then the discovery is successful.

2. Log in to the iSCSI array.
   ```zsh
   iscsiadm -m node --login
   ```
   {: pre}

## Verifying configuration
{: #verifyconfigrhel8}
{: step}

1. Validate that the iSCSI session is established.
   ```zsh
   iscsiadm -m session -o show
   ```
   {: pre}

2. Validate that multiple paths exist.
   ```zsh
   multipath -l
   ```
   {: pre}

   This command reports the paths. If it is configured correctly, then for each volume there is a single group, with a number of paths equal to the number of iSCSI sessions. It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to ensure no disruption of service.

   If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO ensures an extra level of connectivity during those events, and keeps an established session to the LUN with active read/write operations.

3. List the partition tables for the connected device.
   ```zsh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   By default the storage device attaches to `/dev/mapper/<wwid>`. WWID is persistent while the volume exists. The command reports something similar to the following example.
   ```zsh
   Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
   ```

   In the example, `3600a0980383030523424457a4a695266` is the WWID. Your application should use the WWID. It's also possible to assign more easier-to-read names by using "user_friendly_names" or "alias" keywords in multipath.conf. For more information, see the [`multipath.conf` man page](https://linux.die.net/man/5/multipath.conf){: external}.
   {: tip}

   The volume is now mounted and accessible on the host. You can create a file system next.


## Creating a file system (optional)
{: #createfilesys}
{: step}

Follow these steps to create a file system on the newly mounted volume. A file system is necessary for most applications to use the volume. Use [`fdisk` for drives that are less than 2 TB](#fdiskrhel) and [`parted` for a disk bigger than 2 TB](#partedrhel).

### Creating a file system with `fdisk`
{: #fdiskrhel}

1. Get the disk name.
   ```zsh
   fdisk -l | grep /dev/mapper
   ```
   {: pre}

   The disk name that is returned looks similar to `/dev/mapper/XXX`.

2. Create a partition on the disk.

   ```zsh
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   The XXX represents the disk name that is returned in Step 1.

   Scroll further down for the commands codes that are listed in the `fdisk` command table.
   {: tip}

3. Create a file system on the new partition.

   ```zsh
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - The new partition is listed with the disk, similar to `XXXp1`, followed by the size, Type (83), and Linux&reg;.
   - Take a note of the partition name, you need it in the next step. (The XXXp1 represents the partition name.)
   - Create the file system:

     ```zsh
     mkfs.ext3 /dev/mapper/XXXp1
     ```
     {: pre}

4. Create a mount point for the file system, and mount it.
   - Create a partition name `PerfDisk` or where you want to mount the file system.

     ```zsh
     mkdir /PerfDisk
     ```
     {: pre}

   - Mount the storage with the partition name.
     ```zsh
     mount /dev/mapper/XXXp1 /PerfDisk
     ```
     {: pre}

   - Check that you see your new file system listed.
     ```zsh
     df -h
     ```
     {: pre}

5. Add the new file system to the system's `/etc/fstab` file to enable automatic mounting on boot.
   - Append the following line to the end of `/etc/fstab` (with the partition name from Step 3).

     ```zsh
     /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

### Creating a file system with `parted`
{: #partedrhel}

On many Linux&reg; distributions, `parted` comes preinstalled. However, if you need to you can install it by executing the foilowing command.
```zsh
# dnf install parted
```
{: pre}

To create a file system with `parted`, follow these steps.

1. Start the interactive parted shell.

   ```zsh
   parted
   ```
   {: pre}

2. Create a partition on the disk.
   1. Unless it is specified otherwise, `parted` uses your primary drive, which is `/dev/sda` in most cases. Switch to the disk that you want to partition by using the command **select**. Replace **XXX** with your new device name.

      ```zsh
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Run `print` to confirm that you are on the right disk.

      ```zsh
      print
      ```
      {: pre}

   3. Create a GPT partition table.

      ```zsh
      mklabel gpt
      ```
      {: pre}

   4. `Parted` can be used to create primary and logical disk partitions, the steps that are involved are the same. To create a partition, `parted` uses `mkpart`. You can give it other parameters like **primary** or **logical** depending on the partition type that you want to create.

       ```zsh
       mkpart
       ```
       {: pre}

       The listed units default to megabytes (MB). To create a 10-GB partition, you start from 1 and end at 10000. You can also change the sizing units to terabytes by entering `unit TB` if you want to.
       {: tip}

   5. Exit `parted` with `quit`.

      ```zsh
      quit
      ```
      {: pre}

3. Create a file system on the new partition.

   ```zsh
   mkfs.ext3 /dev/mapper/XXXp1
   ```
   {: pre}

   It's important to select the right disk and partition when you run this command. Verify the result by printing the partition table. Under file system column, you can see ext3.
   {: important}

4. Create a mount point for the file system and mount it.
   - Create a partition name `PerfDisk` or where you want to mount the file system.

   ```zsh
   mkdir /PerfDisk
   ```
   {: pre}

   - Mount the storage with the partition name.

   ```zsh
   mount /dev/mapper/XXXp1 /PerfDisk
   ```
   {: pre}

   - Check that you see your new file system listed.

   ```zsh
   df -h
   ```
   {: pre}

5. Add the new file system to the system's `/etc/fstab` file to enable automatic mounting on boot.
   - Append the following line to the end of `/etc/fstab` (by using the partition name from Step 3).

   ```zsh
   /dev/mapper/XXXp1    /PerfDisk    ext3    defaults,_netdev    0    1
   ```
   {: pre}
