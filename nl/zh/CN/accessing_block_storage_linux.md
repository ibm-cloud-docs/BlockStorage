---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-02"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# 在 Linux 上连接到 MPIO iSCSI LUN

以下指示信息适用于 RHEL6/Centos6。添加了针对其他操作系统的注释，但本文档**并未**涵盖所有 Linux 分发版。如果使用的是其他 Linux 操作系统，请参阅特定分发版的文档，并确保多路径支持 ALUA 以划分路径优先级。 

例如，您可以在[此处](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}找到 Ubuntu 有关 iSCSI 启动器配置的指示信息，在[此处](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:}找到有关 DM-Multipath 设置的指示信息。

开始之前，请确保正在访问 {{site.data.keyword.blockstoragefull}} 卷的主机先前已通过 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 授权。

1. 在 {{site.data.keyword.blockstorageshort}} 列表页面中，找到新卷，然后单击**操作**。
2. 单击**授权主机**。
3. 从列表中选择可以访问该卷的一个或多个主机，然后单击**提交**。

## 安装 {{site.data.keyword.blockstorageshort}} 卷

下面是将基于 Linux 的 {{site.data.keyword.BluSoftlayer_full}} 计算实例连接到多路径输入/输出 (MPIO) 因特网小型计算机系统接口 (iSCSI) 逻辑单元号 (LUN) 所需的步骤。

**注**：指示信息中引用的主机 IQN、用户名、密码和目标地址可从 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的 **{{site.data.keyword.blockstorageshort}} 详细信息**屏幕中获取。

**注**：最好是在绕过防火墙的 VLAN 上运行存储流量。通过软件防火墙运行存储流量会延长等待时间，并对存储器性能产生负面影响。

1. 将 iSCSI 和多路径实用程序安装到主机。
   - RHEL/CentOS

   ```
   yum install iscsi-initiator-utils device-mapper-multipath
   ```
   {: pre}

   - Ubuntu/Debian

   ```
   sudo apt-get update
   sudo apt-get install multipath-tools
   ```
   {: pre}

2. 创建或编辑多路径配置文件。
   - 使用以下命令中提供的最低配置来编辑 **/etc/multipath.conf**。<br /><br /> **注**：对于 RHEL7/CentOS7，`multipath.conf` 可能为空白，因为该操作系统具有内置配置。Ubuntu 内置于多路径工具中，因此不会使用 multipath.conf。

   ```
   defaults {
   user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
   # blacklist 下的所有数据必须特定于您的系统。
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

3. 装入多路径模块，启动多路径服务并将其设置为在引导时启动。
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

   - 对于其他分发版，请查阅相应的操作系统供应商文档。

4. 验证多路径是否生效。
   - RHEL 6
     ```
     multipath -l
     ```
     {: pre}

     如果返回空白，说明多路径生效。

   - CentOS 7
     ```
     multipath -ll
     ```
     {: pre}

     RHEL 7/CentOS 7 可能会返回“无 fc_host 设备”，可以忽略此消息。

5. 使用 {{site.data.keyword.slportal}}中的 IQN 更新 `/etc/iscsi/initiatorname.iscsi` 文件。请以小写输入值。
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. 使用 {{site.data.keyword.slportal}} 中的用户名和密码来编辑 `/etc/iscsi/iscsid.conf` 中的 CHAP 设置。对 CHAP 名称请使用大写。
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   **注**：将其他 CHAP 设置保持为注释状态。{{site.data.keyword.BluSoftlayer_full}} 存储器仅使用单向认证。不要启用相互 CHAP。

7. 将 iSCSI 设置为在引导时启动，并立即将其启动。
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
systemctl start iscsi
      ```
      {: pre}

      ```
systemctl start iscsid
      ```
      {: pre}

   - 其他分发版：请查阅相应的操作系统供应商文档。

8. 使用从 {{site.data.keyword.slportal}} 中获取的目标 IP 地址来发现该设备。

     A. 针对 iSCSI 阵列运行发现。
     ```
          iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     B. 将主机设置为自动登录到 iSCSI 阵列。
     ```
          iscsiadm -m node -L automatic
     ```
     {: pre}

9. 验证主机是否已登录到 iSCSI 阵列并维护其会话。
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   此命令会报告路径。

10. 验证该设备是否已连接。缺省情况下，该设备将连接到 `/dev/mapper/mpathX`，其中 X 是生成的已连接设备标识。
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  此命令报告类似于以下示例的内容。
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```
  现在卷已安装到主机上并可供访问。

## 创建文件系统（可选）

执行以下步骤以基于新安装的卷来创建文件系统。大多数应用程序都需要文件系统才可使用卷。对于小于 2 TB 的驱动器，请使用 `fdisk`；对于大于 2 TB 的磁盘，请使用 `parted`。

### 使用 `fdisk`

1. 获取磁盘名称。
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   返回的磁盘名称类似于 `/dev/mapper/XXX`。

2. 在磁盘上创建分区。

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX 表示步骤 1 中返回的磁盘名称。<br />

   **注**：进一步向下滚动可查看在 `fdisk` 命令表中列出的命令代码。

3. 在新分区上创建文件系统。

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - 新分区与磁盘一起列出，类似于 `XXXlp1`，后跟大小、类型 (83) 和 Linux。
   - 记下分区名称，在下一步中将需要此信息。（XXXlp1 表示分区名称。）
   - 创建文件系统：

     ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
     {: pre}

4. 为文件系统创建安装点并安装文件系统。
   - 创建分区名称 `PerfDisk` 或要在其中安装文件系统的分区的名称。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - 使用分区名称安装存储器。
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 检查是否看到新的文件系统列出。
     ```
     df -h
     ```
     {: pre}

5. 将新的文件系统添加到系统的 `/etc/fstab` 文件中，以启用引导时自动安装。
   - 将以下行附加到 `/etc/fstab` 的末尾（使用步骤 3 中的分区名称）。<br />

     ```
/dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### `fdisk` 命令表

<table border="0" cellpadding="0" cellspacing="0">
	<caption><code>fdisk</code> 命令表的左侧包含命令，右侧包含预期结果。</caption>
    <thead>
	<tr>
		<th style="width:40%;">命令</th>
		<th style="width:60%;">结果</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Command: n</code></td>
		<td>创建新分区。&#42;</td>
	</tr>
	<tr>
		<td><code>Command action: p</code></td>
		<td>使分区成为主分区。</td>
	</tr>
	<tr>
		<td><code>Partition number (1-4): 1</code></td>
		<td>成为磁盘上的分区 1。</td>
	</tr>
	<tr>
		<td><code>First cylinder (1-8877): 1 (default)</code></td>
		<td>从柱面 1 开始。</td>
	</tr>
	<tr>
		<td><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></td>
		<td>按 Enter 键以转至最后一个柱面。</td>
	</tr>
	<tr>
		<td><code>Command: t</code></td>
		<td>设置分区的类型。&#42;</td>
	</tr>
	<tr>
		<td><code>Select partition 1.</code></td>
		<td>选择分区 1 以设置为特定类型。</td>
	</tr>
	<tr>
		<td><code>Hex code: 83</code></td>
		<td>选择 Linux 作为类型（83 是表示 Linux 的十六进制代码）。&#42;&#42;</td>
	 </tr>
	<tr>
		<td><code>Command: w</code></td>
		<td>将新的分区信息写入磁盘。&#42;</td>
	</tr>
   </tbody>
</table>

  (`*`) 输入 m 可获取帮助。

  (`**`) 输入 L 可列出十六进制代码

### 使用 `parted`

许多 Linux 分发版上已预安装 `parted`。如果您的分发版中不包含 parted，可以通过以下方式进行安装：
- Debian/Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL/CentOS
  ```
  yum install parted  
  ```
  {: pre}


要使用 `parted` 创建文件系统，请执行以下步骤。

1. 运行 `parted`。

   ```
   parted
   ```
   {: pre}

2. 在磁盘上创建分区。
   1. 除非另行指定，否则 `parted` 会使用主驱动器，在大多数情况下为 `/dev/sda`。使用 **select** 命令切换到要分区的磁盘。将 **XXX** 替换为新设备名称。

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. 运行 `print` 以确认您位于正确的磁盘上。

      ```
      (parted) print
      ```
      {: pre}

   3. 创建 GPT 分区表。

      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. 可以使用 `parted` 来创建主磁盘分区和逻辑磁盘分区，这两个操作所涉及的步骤相同。要创建分区，`parted` 会使用 `mkpart`。可以为其提供其他参数，如 **primary** 或 **logical**，具体取决于您要创建的分区类型。
   <br /> **注**：列出的单位缺省为兆字节 (MB)；要创建 10 GB 的分区，请从 1 开始，到 10000 结束。如果需要，还可以通过输入 `(parted) unit TB` 将大小单位更改为太字节。

      ```
      (parted) mkpart
      ```
      {: pre}

   5. 使用 `quit` 退出 `parted`。

      ```
      (parted) quit
      ```
      {: pre}

3. 在新分区上创建文件系统。

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **注**：运行此命令时，请务必选择正确的磁盘和分区！
   通过打印分区表来验证结果。在“文件系统”列下，可以看到 ext3。

4. 为文件系统创建安装点并安装文件系统。
   - 创建分区名称 `PerfDisk` 或要在其中安装文件系统的分区的名称。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - 使用分区名称安装存储器。
     

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 检查是否看到新的文件系统列出。
     

     ```
     df -h
     ```
     {: pre}

5. 将新的文件系统添加到系统的 `/etc/fstab` 文件中，以启用引导时自动安装。
   - 将以下行附加到 `/etc/fstab` 的末尾（使用步骤 3 中的分区名称）。<br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## 验证是否在 `*NIX` 操作系统中正确地配置了 MPIO

要检查多路径是否在选取设备，请列出设备。如果配置正确，将仅显示两个 NETAPP 设备。

```
# multipath -l
```
{: pre}

```
root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
7:0:0:101 sde 8:64 active undef running
```

检查磁盘是否存在。确认有两个具有相同标识的磁盘，并且 `/dev/mapper` 会列出具有相同标识且大小相同的项。`/dev/mapper` 设备是多路径设置的设备：

```
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```

如果未正确设置，那么将类似于以下示例。
```
No multipath output root@server:~# multipath -l root@server:~#
```

此命令显示列入黑名单的设备。
```
# multipath -l -v 3 | grep sd <date and time>
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

`fdisk` 仅显示 `sd*` 设备，而不会显示 `/dev/mapper`。

```
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```
