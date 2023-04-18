---

copyright:
  years: 2018, 2023
lastupdated: "2023-04-18"

keywords: IBM Block Storage, MPIO, iSCSI, LUN, mount secondary storage, mount storage in CloudLinux

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Connecting to iSCSI LUNs on CloudLinux
{: #mountingCloudLinux}

Follow these instructions to mount your iSCSI LUN with multipath on a CloudLinux Server release 6.10.
{: shortdesc}

Before you start, make sure the host that is accessing the {{site.data.keyword.blockstoragefull}} volume is authorized correctly.
{: important}

## Authorizing the host in the UI
{: #authhostCloudLinui}
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

## Authorizing the host from the SLCLI
{: #authhostCloudLinCli}
{: cli}

You can authorize the host through the SLCLI. 

```sh
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
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

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

## Authorizing the host with Terraform 
{: #authhostCloudLinTerraform}
{: terraform}

When you provision your storage with Terraform, you authorize a compute host to access the volume by specifing the `allowed_virtual_guest_ids` for virtual servers, or `allowed_hardware_ids` for bare metal servers. You can specify `allowed_ip_addresses` to define which IP addresses have access to the storage. The following example provides authorization to the Virtual Server with the ID `27699397` can access the volume from the `10.40.98.193`, `10.40.98.200` addresses.

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

## Mounting {{site.data.keyword.blockstorageshort}} volumes
{: #mountingCloudLin}

1. Install the iSCSI and multipath utilities on your host, and activate them.
   ```sh
   yum install iscsi-initiator-utils
   ```
   {: pre}

   ```sh
   yum install multipath-tools

   ```
   {: pre}

   ```sh
   chkconfig multipathd on
   ```
   {: pre}

   ```sh
   chkconfig iscsid on
   ```
   {: pre}

2. Create or edit your configuration files.
   - Update your `/etc/multipath.conf`.
     ```sh
     defaults {
        user_friendly_names no
        flush_on_last_del       yes
        queue_without_daemon    no
        dev_loss_tmo            infinity
        fast_io_fail_tmo        5
     }
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

   - Update your CHAP settings `/etc/iscsi/iscsid.conf` by adding the username, and password.

     ```sh
     iscsid.startup = /etc/rc.d/init.d/iscsid force-start
     node.startup = automatic
     node.leading_login = No
     node.session.auth.authmethod = CHAP
     node.session.auth.username = <user name value from the console>
     node.session.auth.password = <password value from the console>
     discovery.sendtargets.auth.authmethod = CHAP
     discovery.sendtargets.auth.username = <user name value from the console>
     discovery.sendtargets.auth.password = <password value from the console>
     ```
     {: codeblock}

     Use uppercase for CHAP names. Leave the other CHAP settings commented. {{site.data.keyword.cloud}} storage uses only one-way authentication. Do not enable Mutual CHAP.
     {: important}


3. Restart `iscsi` and `multipathd` services.
   ```sh
   /etc/init.d/iscsi restart
   ```
   {: pre}

   ```sh
   /etc/init.d/multipathd restart
   ```
   {: pre}

4. Discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud_notm}} console.

    1. Run the discovery against the iSCSI array.
       ```sh
       # iscsiadm -m discovery -t sendtargets -p "ip-value-from-SL-Portal"
       ```
       {: pre}

       The output looks similar to the following example.
       ```sh
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

    2. Set the host to automatically log in to the iSCSI array.
       ```sh
       # iscsiadm -m node -L automatic
       ```
       {: pre}

5. Verify that the host is logged in to the iSCSI array and maintained its sessions.
   ```sh
   iscsiadm -m session
   ```
   {: pre}

   The output looks similar to the following example.
   ```sh
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. Verify that the device is connected.
   ```sh
   fdisk -l
   ```
   {: pre}

   The output looks similar to the following example.
   ```sh
   Disk /dev/sda: 999.7 GB, 999653638144 bytes
   255 heads, 63 sectors/track, 121534 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 262144 bytes / 262144 bytes
   Disk identifier: 0x00040f06

   Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1          33      262144   83  Linux
   Partition 1 does not end on cylinder boundary.
   /dev/sda2              33         164     1048576   82  Linux swap / Solaris
   Partition 2 does not end on cylinder boundary.
   /dev/sda3             164      121535   974912512   83  Linux

   Disk /dev/sdb: 21.5 GB, 21474836480 bytes
   64 heads, 32 sectors/track, 20480 cylinders
   Units = cylinders of 2048 * 512 = 1048576 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   Disk identifier: 0x00000000

   Disk /dev/sdc: 21.5 GB, 21474836480 bytes
   64 heads, 32 sectors/track, 20480 cylinders
   Units = cylinders of 2048 * 512 = 1048576 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   Disk identifier: 0x00000000
   ```

   The volume is now mounted and accessible on the host.

7. Verify whether MPIO is configured correctly by listing the devices. It's possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, but it is important that connections are established on both paths to ensure no disruption of service. If the configuration is correct, two NETAPP disks show in the output.

    ```sh
    # multipath -l
    ```
    {: pre}

   The output looks similar to the following example.
    ```sh
    root@server:~# multipath -l
    3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
    size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
    |-+- policy='round-robin 0' prio=50 status=active
    | `- 1:0:0:1 sdb 8:16 active ready running
    `-+- policy='round-robin 0' prio=10 status=enabled
     `- 2:0:0:1 sdc 8:32 active ready running
    ```

On very rare occasions, a LUN is provisioned and attached while the second path is down. In such instances, the host might see one single path when the discovery scan is run. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](/status?component=block-storage&selected=status){: external} to see whether a current event might impact your host's ability to access the storage. If no events are reported, perform the discovery scan again to ensure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](/unifiedsupport/cases/add){: external} so it can be properly investigated.
