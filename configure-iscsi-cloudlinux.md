---

copyright:
  years: 2018, 2023
lastupdated: "2023-12-18"

keywords: IBM Block Storage, MPIO, iSCSI, LUN, mount secondary storage, mount storage in CloudLinux

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Connecting to iSCSI LUNs on CloudLinux
{: #mountingCloudLinux}

Follow these instructions to mount your iSCSI LUN with multipath on a CloudLinux Server release 6.10.
{: shortdesc}

If multiple hosts mount the same {{site.data.keyword.blockstorageshort}} volume without being cooperatively managed, your data is at risk for corruption. Volume corruption can occur if changes are made to the volume by multiple hosts at the same time. You need a cluster-aware, shared-disk file system to prevent data loss such as Microsoft&reg; Cluster Shared Volumes (CSV), Red Hat Global File System (GFS2), VMware&reg; VMFS, and others. For more information, see your host's OS Documentation.
{: attention}

It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance. For more information about routing storage traffic to its own VLAN interface, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#howtoisolatedstorage).
{: important}

Before you begin, make sure that the host that is to access the {{site.data.keyword.blockstorageshort}} volume is authorized. For more information, see [Authorizing the host in the UI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}.
{: requirement}

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

   - Update your CHAP settings `/etc/iscsi/iscsid.conf` by adding the username and password.

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
    {: screen}

On rare occasions, a LUN is provisioned and attached while the second path is down. In such instances, the host might see one single path when the discovery scan is run. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](/status?component=block-storage&selected=status){: external} to see whether a current event might impact your host's ability to access the storage. If no events are reported, perform the discovery scan again to ensure that all paths are properly discovered. If an event is in progress, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](/unifiedsupport/cases/add){: external} so it can be properly investigated.
