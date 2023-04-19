---

copyright:
  years: 2021, 2023
lastupdated: "2023-04-18"

keywords: MPIO, iSCSI LUNs, multipath configuration file, Ubuntu 20, multipath, mpio, Linux, Ubuntu

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 1h

---
{{site.data.keyword.attribute-definition-list}}

# Mount iSCSI LUN on Ubuntu 20
{: #mountingUbu20}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="1h"}

This tutorial guides you through how to mount a {{site.data.keyword.blockstoragefull}} volume on a server with the Ubuntu 20.04 Server Edition operating system. Complete the following steps to connect a Linux&reg;-based {{site.data.keyword.cloud}} Compute instance to a multipath input/output (MPIO) iSCSI storage volume. You're going to create two connections from one network interface of your host to two target IP addresses of the storage array.
{: shortdesc}

Before you begin, make sure the host that is accessing the {{site.data.keyword.blockstorageshort}} volume is authorized correctly.
{: important}

## Authorizing the host in the UI
{: #authhostubu20}
{: ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device types or subnets.
   - If you choose Devices, you can select from Bare Metal Server or Virtual Server instances.
   - If you choose IP address, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that are supposed to access the volume and click **Save**.

When your host is authorized, take note of the following information, which is needed later.
* iSCSI Target IP addresses
* Username
* Password
* IQN

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

Before you start configuring iSCSI, make sure to have the network interfaces correctly set and configured in order for the open-iscsi package to work correctly, especially during startup time. In Ubuntu 20.04 LTS, the default network configuration tool is [netplan.io](https://netplan.readthedocs.io/en/latest/examples/#){: external}. For more information about how the ISCSI service works on the Ubuntu OS, see [iSCSI Initiator (or Client)](https://ubuntu.com/server/docs/service-iscsi){: external} Documentation.


## Authorizing the host from the SLCLI
{: #authhostubu20CLI}
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

When your host is authorized, take note of the following information, which is needed later.
* iSCSI Target IP addresses
* Username
* Password
* IQN

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

Before you start configuring iSCSI, make sure to have the network interfaces correctly set and configured in order for the open-iscsi package to work correctly, especially during startup time. In Ubuntu 20.04 LTS, the default network configuration tool is [netplan.io](https://netplan.readthedocs.io/en/latest/examples/#){: external}. For more information about how the ISCSI service works on the Ubuntu OS, see [iSCSI Initiator (or Client)](https://ubuntu.com/server/docs/service-iscsi){: external} Documentation.

## Authorizing the host with Terraform
{: #authhostclin8Cterraform}
{: terraform}

To authorize a compute host to access the volume, use the `ibm_storage_block` resource and specify the `allowed_virtual_guest_ids` for virtual servers, or `allowed_hardware_ids` for bare metal servers. Specify `allowed_ip_addresses` to define which IP addresses have access to the storage. 

The following example defines that the Virtual Server with the ID `27699397` can access the volume from the `10.40.98.193`, `10.40.98.200` addresses.

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

After your storage resource is created, you can access the `allowed_host_info` attribute, which contains the username, password, and host IQN of the hosts that are needed later.

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

Before you start configuring iSCSI, make sure to have the network interfaces correctly set and configured in order for the open-iscsi package to work correctly, especially during startup time. In Ubuntu 20.04 LTS, the default network configuration tool is [netplan.io](https://netplan.readthedocs.io/en/latest/examples/#){: external}. For more information about how the ISCSI service works on the Ubuntu OS, see [iSCSI Initiator (or Client)](https://ubuntu.com/server/docs/service-iscsi){: external} Documentation.

## Install the iSCSI and multipath utilities
{: #installutilsubu20}
{: step}

Ensure that your system is updated and includes the `open-iscsi` and `multipath-tools` packages. Use the following commands to install the packages.

- Install `open-iscsi`.

    ```sh
    sudo apt install open-iscsi
    ```
    {: pre}

    When the package is installed, it creates the following two files.
    * `/etc/iscsi/iscsid.conf`
    * `/etc/iscsi/initiatorname.iscsi`

- Install `multipath-tools`.

    ```sh
    sudo apt install multipath-tools
     ```
    {: pre}

    If you want to boot from the LUN, then the `multipath-tools-boot` package needs to be installed as well.
    {: tip}

## Set up the multipath
{: #setupmultipathdubu20}
{: step}

1. After you installed the multipath utility, create an empty configuration file that is called `/etc/multipath.conf`.
2. Modify the default values of `/etc/multipath.conf`.

   ```text
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

   The initial defaults section of the configuration file configures your system so that the names of the multipath devices are of the form `/dev/mapper/mpathn`, where `mpathn` is the WWID number of the device.

3. Save the configuration file and exit the editor, if necessary.
4. Start the multipath service.
   ```sh
   service multipath-tools start
   ```
   {: pre}

   If you need to edit the multipath configuration file after you started the multipath daemon, you must restart the `multipathd` service for the changes to take effect.
   {: note}

   For more information about using the Device Mapper Multipath feature on Ubuntu 20, see [Device Mapper Multipathing - Introduction](https://ubuntu.com/server/docs/device-mapper-multipathing-introduction){: external}.

## Update /etc/iscsi/initiatorname.iscsi file
{: #updateinitiatorubu20}
{: step}

Update `/etc/iscsi/initiatorname.iscsi` file with the IQN from the {{site.data.keyword.cloud}} console. Enter the value as lowercase.

```sh
InitiatorName=<value-from-the-Portal>
```
{: pre}


## Configure credentials
{: #configcredubu20}
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

Restart the iscsi service for the changes to take effect.

```sh
systemctl restart iscsid.service
```
{: pre}

## Discover the storage device and login
{: #discoverandloginubu20}
{: step}

The iscsiadm utility is a command-line tool that is used for the discovery and login to iSCSI targets, plus access and management of the open-iscsi database. For more information, see the [iscsiadm(8) man page](https://linux.die.net/man/8/iscsiadm){: external}. In this step, discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud}} console.

1. Run the discovery against the iSCSI array.
   ```sh
   iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
   ```
   {: pre}

   If the IP information and access details are displayed, then the discovery is successful.

2. Configure automatic login.
   ```sh
   sudo iscsiadm -m node --op=update -n node.conn[0].startup -v automatic
   sudo iscsiadm -m node --op=update -n node.startup -v automatic
   ```
3. Enable the necessary services.
   ```sh
   systemctl enable open-iscsi
   systemctl enable iscsid
   ```

4. Restart the iscsid service.
   ```sh
   systemctl restart iscsid.service
   ```

5. Log in to the iSCSI array.
   ```sh
   sudo iscsiadm -m node --loginall=automatic
   ```
   {: pre}


## Verifying configuration
{: #verifyconfigubu20}
{: step}

1. Validate that the iSCSI session is established.
   ```sh
   iscsiadm -m session -o show
   ```
   {: pre}

2. Validate that multiple paths exist.
   ```sh
   multipath -ll
   ```
   {: pre}

   This command reports the paths. If it is configured correctly, then each volume has a single group, with a number of paths equal to the number of iSCSI sessions. It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to ensure no disruption of service.

   ```text
   $ sudo multipath -ll
   mpathb (360014051f65c6cb11b74541b703ce1d4) dm-1 LIO-ORG,TCMU device
   size=1.0G features='0' hwhandler='0' wp=rw
   |-+- policy='service-time 0' prio=1 status=active
   | `- 7:0:0:2 sdh 8:112 active ready running
   `-+- policy='service-time 0' prio=1 status=enabled
     `- 8:0:0:2 sdg 8:96  active ready running
   mpatha (36001405b816e24fcab64fb88332a3fc9) dm-0 LIO-ORG,TCMU device
   size=1.0G features='0' hwhandler='0' wp=rw
   |-+- policy='service-time 0' prio=1 status=active
   | `- 7:0:0:1 sdj 8:144 active ready running
   `-+- policy='service-time 0' prio=1 status=enabled
     `- 8:0:0:1 sdi 8:128 active ready running
   ```

   If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance. MPIO ensures an extra level of connectivity during those events, and keeps an established session to the LUN with active read/write operations.

   In the example,`36001405b816e24fcab64fb88332a3fc9` is the WWID that is persistent while the volume exists. It is recommended that your application uses the WWID. It's also possible to assign more easier-to-read names by using "user_friendly_names" or "alias" keywords in multipath.conf. For more information, see the [`multipath.conf` man page](https://linux.die.net/man/5/multipath.conf){: external}.
   {: tip}

3. Check `dmesg` to make sure that the new disks are detected.
   ```sh
   dmesg
   ```
   {: pre}

## Creating a partition and a file system (optional)
{: #createfilesysubu20}
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
