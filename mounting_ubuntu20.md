---

copyright:
  years: 2021, 2025
  lastupdated: "2025-12-03"

keywords: MPIO, iSCSI LUNs, multipath configuration file, Ubuntu 20, multipath, mpio, Linux, Ubuntu

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mount iSCSI volume on Ubuntu OS
{: #mountingUbuntu}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

This tutorial guides you through how to mount an {{site.data.keyword.blockstoragefull}} volume on a server with an Ubuntu operating system. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

For more information about how the iSCSI service works on the Ubuntu OS, see [iSCSI Initiator (or Client)](https://documentation.ubuntu.com/server/iscsi-initiator-or-client/){: external} Documentation. If you're using another Linux&reg; operating system, refer to the Documentation of your specific distribution, and make sure that the multipath supports ALUA for path priority.
{: tip}

## Before you begin
{: #beforemountingubu24}

1. [Create a virtual server for Classic in the console](/docs/virtual-servers?group=provisioning-ht), from the CLI, with the API, or [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-sample_infrastructure_config).

1. [Order a block storage volume](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage) in the same data center.

1. Make sure that the host is authorized to access the {{site.data.keyword.blockstorageshort}} volume. For more information, see [Authorizing the host in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}. After the authorization is complete, note the username, password, and host IQN information.

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your OS Documentation.

Before you start configuring iSCSI, make sure that the network interfaces are correctly set and configured for the open-iscsi package to work correctly, especially during startup time. In newer versions of Ubuntu, the main tool for setting network address information is [Netplan](https://netplan.readthedocs.io/en/latest/examples/#){: external}. It uses a YAML configuration file to define network settings, replacing older methods like `/etc/network/interfaces`. Netplan can be configured with the command line or through the NetworkManager in desktop environments.

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

## Install the iSCSI and multipath utilities
{: #installutilsubu24}
{: step}

Make sure that your system is updated and includes the `open-iscsi` and `multipath-tools` packages. Use the following commands to install the packages.

- Update the OS:
   ```sh
   apt-get update
   ```
   {: pre}

- Install `open-iscsi`.

    ```sh
    sudo apt install open-iscsi
    ```
    {: pre}

    When the package is installed, it creates the following two files.
    * `/etc/iscsi/iscsid.conf`
    * `/etc/iscsi/initiatorname.iscsi`

- Start the services:
    
    ```sh
    systemctl enable open-iscsi
    ```
    {: pre}

    ```sh
    systemctl enable iscsid  
    ```
    {: pre}

- Install `multipath-tools`.

    ```sh
    sudo apt install multipath-tools
     ```
    {: pre}

## Configure ISCSI
{: #updateiscsiconfigubu24}
{: step}

### Updating Initiator name
{: #updateinitiatorubu24}

1. Check that the `/etc/iscsi/initiatorname.iscsi` file exists and review its contents.

   ```sh
   cat /etc/iscsi/initiatorname.iscsi 
   ```
   {: pre}
   
   ```sh
   root@vsi4classicubuntu:~# cat /etc/iscsi/initiatorname.iscsi  
   GenerateName=yes  
   ```
   {: screen}

1. By using a text editor, update the `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. Enter the value in the following format.

   ```sh
   InitiatorName=<IQN-value-from-the-portal>
   ```
   {: pre}

### Configuring credentials
{: #configcredubu24}
{: step}

1. Make sure that the `iscsid.conf` file exists by checking its contents.
   ```sh
   cat /etc/iscsi/iscsid.conf
   ```
   {: pre} 

1. By using the text editor, uncomment and edit the following entries. You can find the username and password in the {{site.data.keyword.cloud}} console.

   ```text
   node.session.auth.authmethod = CHAP
   node.session.auth.username = <Username-value-from-Portal>
   node.session.auth.password = <Password-value-from-Portal>
   discovery.sendtargets.auth.authmethod = CHAP
   discovery.sendtargets.auth.username = <Username-value-from-Portal>
   discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   Keep the other CHAP settings commented. {{site.data.keyword.cloud}} storage uses only one-way authentication. Do not enable Mutual CHAP.
   {: important}

1. Restart the iscsi service for the changes to take effect.

   ```sh
   systemctl restart iscsid.service
   ```
   {: pre}

For more information, see [Ubuntu manuals - `iscsid`](https://manpages.ubuntu.com/manpages/questing/en/man8/iscsid.8.html){: external} and [Ubuntu manuals - `systemctl`](https://manpages.ubuntu.com/manpages/questing/en/man1/systemctl.1.html){: external}.

## Set up the multipath
{: #setupmultipathdubu24}
{: step}

1. After you installed the multipath utility, you can check the default content of the `multipath.conf` file.
   ```sh
   cat /etc/multipath.conf
   ```
   {: pre}  

2. Modify the default values of `/etc/multipath.conf` with a text editor by replacing them with the contents of the following snippet.

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
   ```
   {: codeblock}

   The initial defaults section of the configuration file configures your system so that the names of the multipath devices are of the form `/dev/mapper/mpathn`, where `mpathn` is the WWID number of the device. For more information, see [Ubuntu manuals - `multipath.conf`](https://manpages.ubuntu.com/manpages/questing/en/man5/multipath.conf.5.html){: external}.

3. Save the configuration file and exit the editor.
4. Start the multipath service.

   ```sh
   service multipath-tools start
   ```
   {: pre}

   3. Enable the necessary services.

   If you need to edit the multipath configuration file after you started the multipath daemon, you must restart the `multipathd` service for the changes to take effect.
   {: note}

   For more information about using the Device Mapper Multipath feature on Ubuntu 20, see [Device Mapper Multipathing - Introduction](https://documentation.ubuntu.com/server/introduction-to-device-mapper-multipathing/){: external}.

## Discover the storage device and login
{: #discoverandloginubu24}
{: step}

The iscsiadm utility is a command-line tool that is used for the discovery and login to iSCSI targets, plus access and management of the open-iscsi database. For more information, see the [Ubuntu manuals - `iscsiadm`](https://manpages.ubuntu.com/manpages/questing/en/man8/iscsiadm.8.html){: external}. In this step, discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

1. Run the discovery against the iSCSI array.
   ```sh
   iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
   ```
   {: pre}

   If the IP information and access details are displayed, then the discovery is successful.

   ```sh
   root@vsi4classicubuntu:~# iscsiadm -m discovery -t sendtargets -p 161.26.114.197  
   161.26.114.197:3260,1052 iqn.1992-08.com.netapp:stfdal1304
   161.26.114.196:3260,1049 iqn.1992-08.com.netapp:stfdal1304
   ```
   {: screen}  

2. Configure automatic login.
   ```sh
   sudo iscsiadm -m node --op=update -n node.conn[0].startup -v automatic
   sudo iscsiadm -m node --op=update -n node.startup -v automatic
   ```
   {: codeblock}

5. Log in to the iSCSI array.
   ```sh
   sudo iscsiadm -m node --login
   ```
   {: pre}

## Verifying configuration
{: #verifyconfigubu24}
{: step}

1. Validate that the iSCSI session is established.
   ```sh
   iscsiadm -m session -o show
   ```
   {: pre}

   ```sh
   root@vsi4classicubuntu:~# iscsiadm -m session -o show  
   tcp: [3] 161.26.114.197:3260,1052 iqn.1992-08.com.netapp:stfdal1304 (non-flash)  
   tcp: [4] 161.26.114.196:3260,1049 iqn.1992-08.com.netapp:stfdal1304 (non-flash)  
   ```
   {: scteen}

2. Validate that multiple paths exist.
   
   ```sh
   multipath -ll
   ```
   {: pre}

   This command reports the paths. If it is configured correctly, then each volume has a single group, with a number of paths equal to the number of iSCSI sessions. It's possible to attach a volume with a single path, but it is important that connections are established on both paths to ward against disruption of service.

   ```sh
   root@vsi4classicubuntu:~# multipath -ll  
    3600a0980383056666424506a33426478 dm-0 NETAPP,LUN C-Mode
    size=500G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua'
    wp=rw
    |-+- policy='service-time 0' prio=50 status=active
    | `- 3:0:0:0 sdb 8:16 active ready running
    `-+- policy='service-time 0' prio=10 status=enabled
      `- 2:0:0:0 sda 8:0    active ready running
   ```
   {: screen}

   If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO provides an extra level of connectivity during those events, and keeps an established session to the volume with active read/write operations.

   In the example,`3600a0980383056666424506a33426478` is the WWID that is persistent while the volume exists. It is recommended that your application uses the WWID. It's also possible to assign easier-to-read names by using "user_friendly_names" or "alias" keywords in multipath.conf.
   {: tip}

3. Check `dmesg` to make sure that the new disks are detected.
   ```sh
   dmesg
   ```
   {: pre}

## Creating a partition and a file system (optional)
{: #createfilesysubu24}
{: step}

After the volume is mounted and accessible on the host, you can create a file system. Follow these steps to create a file system on the newly mounted volume.

1. Create a partition.
   ```text
   $ sudo fdisk /dev/mapper/mpatha

   Welcome to fdisk (util-linux 2.34).
   Changes will remain in memory only, until you decide to write them.
   Be careful before using the write command.

   Device does not contain a recognized partition table.
   Created a new DOS disklabel with disk identifier 0x92c0322a.

   Command (m for help): p
   Disk /dev/mapper/mpatha: 1 GiB, 1073741824 bytes, 2097152 sectors
   Units: sectors of 1 * 512 = 512 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 512 bytes / 65536 bytes
   Disklabel type: dos
   Disk identifier: 0x92c0322a

   Command (m for help): n
   Partition type
      p   primary (0 primary, 0 extended, 4 free)
      e   extended (container for logical partitions)
   Select (default p): p
   Partition number (1-4, default 1):
   First sector (2048-2097151, default 2048):
   Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-2097151, default 2097151):

   Created a new partition 1 of type 'Linux' and of size 1023 MiB.

   Command (m for help): w
   The partition table has been altered.
   ```
   {: screen}

2. Create the file system.
   ```text
   $ sudo mkfs.ext4 /dev/mapper/mpatha-part1
   mke2fs 1.45.5 (07-Jan-2020)
   Creating filesystem with 261888 4k blocks and 65536 inodes
   Filesystem UUID: cdb70b1e-c47c-47fd-9c4a-03db6f038988
   Superblock backups stored on blocks:
           32768, 98304, 163840, 229376

   Allocating group tables: done
   Writing inode tables: done
   Creating journal (4096 blocks): done
   Writing superblocks and filesystem accounting information: done
   ```

3. Mount the block device.
   ```sh
   sudo mount /dev/mapper/mpatha-part1 /mnt
   ```

4. Access the data to confirm that the new partition and file system are ready for use.
   ```sh
   ls /mnt
   ```

## Unmounting {{site.data.keyword.blockstorageshort}} volumes
{: #unmountingUbu}

When you no longer need the volume, unmount it before you delete it.

1. Unmount the file system.
   ```zsh
   umount /dev/mapper/XXXp1 /PerfDisk
   ```
   {: pre}

   For more information, see the [Ubuntu manuals - `umount`](https://manpages.ubuntu.com/manpages/questing/en/man2/umount.2.html){: external}.

2. If you do not have any other volumes in that target portal, you can log out of the target.
   ```zsh
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. If you do not have any other volumes in that target portal, delete the target portal record to prevent future login attempts.
   ```zsh
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   For more information, see the [Ubuntu manuals - `iscsiadm`](https://manpages.ubuntu.com/manpages/questing/en/man8/iscsiadm.8.html){: external}.
   {: tip}
