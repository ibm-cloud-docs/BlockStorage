---

copyright:
  years: 2018, 2020
lastupdated: "2020-06-25"

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

## Authorizing the host
{: #authhostclin}

1. Log in to the [{{site.data.keyword.cloud_notm}} console](/login){: external}. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic").
2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the new volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device types or subnets.
    - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
    - If you choose IP address, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that are supposed to access the volume and click **Save**.

Alternatively, you can authorize the host through the SLCLI.
```zsh
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

```zsh
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

## Mounting {{site.data.keyword.blockstorageshort}} volumes
{: #mountingCloudLin}

1. Install the iSCSI and multipath utilities on your host, and activate them.
   ```zsh
   yum install iscsi-initiator-utils
   ```
   {: pre}

   ```zsh
   yum install multipath-tools

   ```
   {: pre}

   ```zsh
   chkconfig multipathd on
   ```
   {: pre}

   ```zsh
   chkconfig iscsid on
   ```
   {: pre}

2. Create or edit your configuration files.
   - Update your `/etc/multipath.conf`.
     ```zsh
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

   - Update your CHAP settings `/etc/iscsi/iscsid.conf` by adding the user name, and password.

     ```zsh
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
   ```zsh
   /etc/init.d/iscsi restart
   ```
   {: pre}

   ```zsh
   /etc/init.d/multipathd restart
   ```
   {: pre}

4. Discover the device by using the Target IP address that was obtained from the {{site.data.keyword.cloud_notm}} console.

    1. Run the discovery against the iSCSI array.
       ```zsh
       # iscsiadm -m discovery -t sendtargets -p "ip-value-from-SL-Portal"
       ```
       {: pre}

       Example output
       ```zsh
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

    2. Set the host to automatically log in to the iSCSI array.
       ```zsh
       # iscsiadm -m node -L automatic
       ```
       {: pre}

5. Verify that the host is logged in to the iSCSI array and maintained its sessions.
   ```zsh
   iscsiadm -m session
   ```
   {: pre}

   Example output
   ```zsh
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. Verify that the device is connected.
   ```zsh
   fdisk -l
   ```
   {: pre}

   Example output
   ```zsh
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

    ```zsh
    # multipath -l
    ```
    {: pre}

   Example output
    ```zsh
    root@server:~# multipath -l
    3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
    size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
    |-+- policy='round-robin 0' prio=50 status=active
    | `- 1:0:0:1 sdb 8:16 active ready running
    `-+- policy='round-robin 0' prio=10 status=enabled
     `- 2:0:0:1 sdc 8:32 active ready running
    ```

In the rare case of a LUN being provisioned and attached while the second path is down, when the discovery scan is run for the first time, the host might see a single path returned. If you encounter this phenomenon, check the [{{site.data.keyword.cloud}} status page](https://{DomainName}/status?component=block-storage&selected=status){: external} to see whether there's an event that impacts your host's ability to access the storage. If no events are reported, perform the discovery scan again to ensure that all paths are properly discovered. If there's an event, the storage can be attached with a single path. However, it's essential that paths are rescanned after the event is completed. If both paths are not discovered after the rescan, [create a support case](https://{DomainName}/unifiedsupport/cases/add){: external} so it can be properly investigated.
