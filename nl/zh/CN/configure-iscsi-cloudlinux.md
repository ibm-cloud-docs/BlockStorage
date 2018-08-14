---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-02"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# 在 CloudLinux 上连接到 MPIO iSCSI LUN

遵循以下指示信息在 CloudLinux Server R6.10 上安装使用多路径的 iSCSI LUN。

开始之前，请确保正在访问 {{site.data.keyword.blockstoragefull}} 卷的主机先前已通过 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 授权。

1. 登录到 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}。 
2. 在 {{site.data.keyword.blockstorageshort}} 列表页面中，找到新卷，然后单击**操作**。
3. 单击**授权主机**。
4. 从列表中选择可以访问该卷的一个或多个主机，然后单击**提交**。
5. 记下主机 IQN、用户名、密码和目标地址。

**注**：最好在绕过防火墙的 VLAN 上运行存储流量。通过软件防火墙运行存储流量会延长等待时间，并对存储器性能产生负面影响。

## 安装 {{site.data.keyword.blockstorageshort}} 卷

1. 在主机上安装 iSCSI 和多路径实用程序并将其激活。
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

2. 创建或编辑配置文件。
   - 更新“/etc/multipath.conf”。<br/>**注** - blacklist 下的所有数据必须特定于您的系统。
     ```
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

   - 通过添加用户名和密码来更新 CHAP 设置 `/etc/iscsi/iscsid.conf`。
   
     ```
     iscsid.startup = /etc/rc.d/init.d/iscsid force-start
     node.startup = automatic
     node.leading_login = No
     node.session.auth.authmethod = CHAP
     node.session.auth.username = <USER NAME VALUE FROM PORTAL>
     node.session.auth.password = <PASSWORD VALUE FROM PORTAL>
     discovery.sendtargets.auth.authmethod = CHAP
     discovery.sendtargets.auth.username = <USER NAME VALUE FROM PORTAL>
     discovery.sendtargets.auth.password = <PASSWORD VALUE FROM PORTAL>
     ```
     {: codeblock}
   
     **注** - 对 CHAP 名称请使用大写。将其他 CHAP 设置保持为注释状态。{{site.data.keyword.BluSoftlayer_full}} 存储器仅使用单向认证。不要启用相互 CHAP。


3. 重新启动 `iscsi` 和 `multipathd` 服务。
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}
   
   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}
 
4. 使用从 {{site.data.keyword.slportal}} 中获取的目标 IP 地址来发现该设备。

     A. 针对 iSCSI 阵列运行发现。
     ```
            iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
       {: pre}
     
        示例输出
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. 将主机设置为自动登录到 iSCSI 阵列。
     ```
            iscsiadm -m node -L automatic
     ```
       {: pre}

5. 验证主机是否已登录到 iSCSI 阵列并维护其会话。
   ```
   iscsiadm -m session
   ```
   {: pre}
   
   示例输出
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. 验证该设备是否已连接。
   ```
   fdisk -l 
   ```
   {: pre}
    
   示例输出
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
    
     现在卷已安装到主机上并可供访问。

7. 通过列出设备来验证是否正确配置了 MPIO。如果配置正确，将仅显示两个 NETAPP 设备。

   ```
# multipath -l
```
   {: pre}
   
   示例输出
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
