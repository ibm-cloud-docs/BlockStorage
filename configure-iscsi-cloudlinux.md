---

copyright:
  years: 2014, 2018
lastupdated: "2018-07-30"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Connecting to MPIO iSCSI LUNs on CloudLinux

Follow these instructions to install your iSCSI LUN with multipath on CloudLinux Server release 6.10.

Before you start, make sure the host that is accessing the {{site.data.keyword.blockstoragefull}} volume was previously authorized through the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Log in to the [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. From the {{site.data.keyword.blockstorageshort}} listing page, locate the new volume and click **Actions**.
3. Click **Authorize Host**.
4. From the list, select the host or hosts that can access the volume and click **Submit**.
5. Take note of the Host IQN, user name, password, and target address.

**Note:** It's best to run storage traffic on a VLAN, which bypasses the firewall. Running storage traffic through software firewalls increases latency and adversely affects storage performance.

## Mounting {{site.data.keyword.blockstorageshort}} volumes

1. Install the iSCSI and multipath utilities on your host and activate them.
   ```
   yum install iscsi-initiator-utils
   ```
   {: pre}
   
   ```
   yum install multipath-tools
   
   ```
   {: pre}
   
   ```
   chkconfig multipathd on
   ```
   {: pre}
   
   ```
   chkconfig iscsid on
   ```
   {: pre}

2. Create or edit your configuration files.
   - Update your '/etc/multipath.conf'
     ```
     [root@acs-mvries-cloudlinux multipath]# cat /etc/multipath.conf | grep -v \# | grep -v ^$
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

   - Update your CHAP settings `/etc/iscsi/iscsid.conf` by adding the user name, password
   
     ```
     iscsid.startup = /etc/rc.d/init.d/iscsid force-start
     node.startup = automatic
     node.startup = automatic
     node.leading_login = No
     node.session.auth.authmethod = CHAP
     node.session.auth.username = <Username-value-from-Portal>
     node.session.auth.password = <Password-value-from-Portal>
     discovery.sendtargets.auth.username = <Username-value-from-Portal>
     discovery.sendtargets.auth.password = <Password-value-from-Portal>
     discovery.sendtargets.auth.authmethod = CHAP
     node.session.timeo.replacement_timeout = 120
     node.conn[0].timeo.login_timeout = 15
     node.conn[0].timeo.logout_timeout = 15
     node.conn[0].timeo.noop_out_interval = 5
     node.conn[0].timeo.noop_out_timeout = 5
     node.session.err_timeo.abort_timeout = 15
     node.session.err_timeo.lu_reset_timeout = 30
     node.session.err_timeo.tgt_reset_timeout = 30
     node.session.initial_login_retry_max = 8
     node.session.cmds_max = 128
     node.session.queue_depth = 32
     node.session.xmit_thread_priority = -20
     node.session.iscsi.InitialR2T = No
     node.session.iscsi.InitialR2T = No
     node.session.iscsi.ImmediateData = Yes
     node.session.iscsi.ImmediateData = Yes
     node.session.iscsi.FirstBurstLength = 262144
     node.session.iscsi.MaxBurstLength = 16776192
     node.conn[0].iscsi.MaxRecvDataSegmentLength = 262144
     node.conn[0].iscsi.MaxXmitDataSegmentLength = 0
     discovery.sendtargets.iscsi.MaxRecvDataSegmentLength = 32768
     node.conn[0].iscsi.HeaderDigest = None
     node.session.nr_sessions = 1
     node.session.iscsi.FastAbort = Yes
     node.conn[0].timeo.login_timeout = 15
     node.conn[0].timeo.logout_timeout = 15
     node.conn[0].timeo.noop_out_interval = 10
     node.conn[0].timeo.noop_out_timeout = 15
     node.conn[0].iscsi.MaxRecvDataSegmentLength = 65536
     ```
     {: codeblock}
   
     **Note:** Leave the other CHAP settings commented. {{site.data.keyword.BluSoftlayer_full}} storage uses only one-way authentication.


3. Restart `iscsi` and `multipathd` services.
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}
   
   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}
 
4. Discover the device by using the Target IP address that was obtained from the {{site.data.keyword.slportal}}.

     A. Run the discovery against the iSCSI array.
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     B. Set the host to automatically log in to the iSCSI array.
     ```
     iscsiadm -m node -L automatic
     ```
     {: pre}

5. Verify that the host is logged in to the iSCSI array and maintained its sessions.
   ```
   iscsiadm -m session
   ```
   {: pre}


6. Verify that the device is connected.
   ```
   fdisk -l 
   ```
   {: pre}
    
   This command reports something similar to the following example.
   
   ```
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
   {: codeblock}
    
   The volume is now mounted and accessible on the host.

7. Verify whether MPIO is configured correctly by listing the devices. If it's configured correct, only two NETAPP devices show up.

   ```
   # multipath -l
   ```
   {: pre}

   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
   {: codeblock}
