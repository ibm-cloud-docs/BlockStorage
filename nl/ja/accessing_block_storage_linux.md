---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Linux での MPIO iSCSI LUN への接続

以下の説明は、RHEL6/Centos6 向けのものです。 別の Linux オペレーティング・システムを使用している場合、構成については、ご使用の特定のディストリビューションの資料を参照し、マルチパスがパスの優先順位として ALUA をサポートしていることを確認してください。

開始する前に、{{site.data.keyword.blockstoragefull}} ボリュームにアクセスするホストが、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} を介して許可されていることを確認してください。

1. {{site.data.keyword.blockstorageshort}} リスト表示ページで、新たにプロビジョンされたボリュームに関連付けられている**「アクション」**をクリックします。
2. **「ホストの許可」**をクリックします。
3. リストから目的のホスト (複数可) を選択し、**「送信」**をクリックします。これにより、ホストはボリュームへのアクセスを許可されます。

## {{site.data.keyword.blockstorageshort}} ボリュームのマウント

以下に、Linux ベースの {{site.data.keyword.BluSoftlayer_full}} コンピューティング・インスタンスをマルチパス入出力 (MPIO) Internet Small Computer System Interface (iSCSI) 論理装置番号 (LUN) に接続するために必要なステップを示します。

この例は、**Red Hat Enterprise Linux 6** に基づいています。その他の Linux ディストリビューションの場合、オペレーティング・システム (OS) ベンダーの資料に従って、ステップを調整する必要があります。 その他の OS に関する注記を追加しましたが、本書は、すべての Linux ディストリビューションをカバーするものでは**ありません**。 例えば、Ubuntu の場合、iSCSI イニシエーター構成の説明については、[ここ](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:}をクリックしてください。また、DM-Multipath のセットアップの詳細については、[ここ](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}をクリックしてください。

**注:** 手順に示されているホスト IQN、ユーザー名、パスワード、およびターゲット・アドレスは、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} の**「{{site.data.keyword.blockstorageshort}} の詳細」**画面で取得できます。

**注:** ベスト・プラクティスとして、ファイアウォールをバイパスする VLAN 上でストレージ・トラフィックを実行することをお勧めします。 ソフトウェア・ファイアウォールを介してストレージ・トラフィックを実行すると、待ち時間が増加し、ストレージ・パフォーマンスに悪影響を与えます。

1. iSCSI とマルチパスのユーティリティーをホストにインストールします。
   - RHEL/CentOS:

   ```
   yum install iscsi-initiator-utils device-mapper-multipath
   ```
   {: pre}

   - Ubuntu/Debian:

   ```
   sudo apt-get update
   sudo apt-get install multipath-tools
   ```
   {: pre}

2. マルチパス構成ファイルを作成または編集します。
   - 以下のコマンドに指定されている最小限の構成で **/etc/multipath.conf** を編集します。 <br /><br /> **注:** RHEL7/CentOS7 の場合、OS に組み込みの構成があるため、`multipath.conf` はブランクにできます。 multipath-tools に multipath.conf が組み込まれているため、Ubuntu は multipath.conf を使用しません。

   ```
   defaults {
   user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
   # All data under blacklist must be specific to your system.
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

3. マルチパス・モジュールをロードして、マルチパス・サービスを開始し、ブート時に開始するように設定します。
   - RHEL 6:
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
   
   - CentOS 7: 
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
     
   - Ubuntu:
     ```
     service multipath-tools start
     ```
     {: pre}
    
   - その他のディストリビューションについては、OS ベンダーの資料を参照してください。

4. マルチパスが機能していることを確認します。
   - RHEL 6:
     ```
     multipath -l
     ```
     {: pre}
     
     この時点でブランクが返された場合は、機能しています。 
   
   - CentOS 7:
     ```
     multipath -ll
     ```
     {: pre}
     
     RHEL 7/CentOS 7 では「No fc_host device」が返される場合がありますが、これは無視してかまいません。 

5. {{site.data.keyword.slportal}} から取得した IQN で **/etc/iscsi/initiatorname.iscsi** ファイルを更新します。 値を小文字で入力します。
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. {{site.data.keyword.slportal}} から取得したユーザー名とパスワードを使用して **/etc/iscsi/iscsid.conf** 内の CHAP 設定を編集します。 CHAP 名には大文字を使用します。
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   **注:** その他の CHAP 設定はコメント化したままにしてください。 {{site.data.keyword.BluSoftlayer_full}} ストレージは、片方向認証のみを使用します。

7. ブート時に開始するように iSCSI を設定し、この時点で iSCSI を開始します。
   - RHEL 6:

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

   - CentOS 7:

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

   - その他のディストリビューション: OS ベンダーの資料を参照してください。
   
8. {{site.data.keyword.slportal}} から取得したターゲット IP アドレスを使用して、デバイスをディスカバーします。

     a. iSCSI アレイに対してディスカバリーを実行します。
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     b. iSCSI アレイに自動的にログインするようにホストを設定します。
     ```
     iscsiadm -m node -L automatic
     ```
     {: pre}

9. ホストが iSCSI アレイにログインしており、セッションを維持していることを確認します。
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   これにより、この時点でのパスが報告されます。

10. デバイスが接続されていることを確認します。  デフォルトでは、デバイスは /dev/mapper/mpathX に接続されます。ここで X は、接続されるデバイスの生成 ID です。
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  以下のような内容が報告されます。
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 byte
    ```
  ボリュームはホスト上にマウント済みであり、アクセス可能です。

## ファイル・システムの作成 (オプション)

以下に、新しくマウントされたボリューム上にファイル・システムを作成するためのステップを示します。 ファイル・システムは、ボリュームを有効活用するために、ほとんどのアプリケーションに必要です。 2 TB より小さいドライブの場合は **fdisk** を使用し、2 TB より大きいディスクの場合は **parted** を使用します。

### fdisk

1. ディスク名を取得します。
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   返されるディスク名は、/dev/mapper/XXX のようになります。

2. fdisk を使用してディスク上にパーティションを作成します。

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX は、ステップ 1 で返されたディスク名を表します。 <br />

   **注**: このセクションの下にさらにスクロールダウンすると、Fdisk コマンドの表にコマンド・コードがリストされています。

3. 新規パーティション上にファイル・システムを作成します。

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - 新規パーティションはディスクと一緒にリストされ、XXXlp1 のようなものであり、その後にサイズ、タイプ (83)、および Linux が続きます。
   - 次のステップで必要になるので、パーティション名をメモします。 (XXXlp1 がパーティション名を表します。)
   - ファイル・システムを作成します。

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. ファイル・システムのマウント・ポイントを作成し、マウントします。
   - パーティション名 PerfDisk を作成するか、ファイル・システムをマウントする場所を作成します。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - パーティション名を使用して、ストレージをマウントします。
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 新しいファイル・システムがリストされることを確認します。
     ```
     df -h
     ```
     {: pre}

5. 新しいファイル・システムをシステムの **/etc/fstab** ファイルに追加して、ブート時の自動マウントを有効にします。
   - 以下の行を **/etc/fstab** の末尾に追加します (ステップ 3 で取得したパーティション名を使用します)。 <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Fdisk コマンド表
<table border="0" cellpadding="0" cellspacing="0">
 <tbody>
	<tr>
		<td style="width:40%;"><div>コマンド</div></td>
		<td style="width:60%;">結果</td>
	</tr>
	<tr>
		<td><li>&#42; <code>Command: n</code></li>	</td>
		<td>新規パーティションを作成します。</td>
	</tr>
	<tr>
		<td><li><code>Command action: p</code></li></td>
		<td>パーティションを 1 次パーティションにします。</td>
	</tr>
	<tr>
		<td><li><code>Partition number (1 から 4): 1</code></li></td>
		<td>ディスク上のパーティション 1 になります。</td>
	</tr>
	<tr>
		<td><li><code>First cylinder (1 から 8877): 1 (default)</code></li></td>
		<td>シリンダー 1 から開始します。</td>
	</tr>
	<tr>
		<td><li><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></li></td>
		<td>「Enter」を押して最後のシリンダーに移動します。</td>
	</tr>
	<tr>
		<td><li>&#42; <code>Command: t</code></li></td>
		<td>パーティションのタイプをセットアップします。</td>
	</tr>
	<tr>
		<td><li><code>Select partition 1.</code></li></td>
		<td>特定のタイプとしてセットアップするためにパーティション 1 を選択します。</td>
	</tr>
	<tr>
		<td><li>&#42;&#42; <code>Hex code: 83</code></li></td>
		<td>タイプとして Linux を選択します (83 は Linux の 16 進コードです)。</td>
	 </tr>
	<tr>
		<td><li>&#42; <code>Command: w</code></li></td>
		<td>新しいパーティション情報をディスクに書き込みます。</td>
	</tr>
 </tbody>
</table>

  (`*`) ヘルプが必要な場合、m を入力します。

  (`**`) 16 進コードをリストするには、L を入力します。

### parted

多くの Linux ディストリビューションで、**parted** はプリインストールされています。 ご使用のディストリビューションに含まれていない場合、以下のようにしてインストールできます。
- Debian/Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL/CentOS:
  ```
  yum install parted  
  ```
  {: pre}


**parted** を使用してファイル・システムを作成するには、以下のステップに従います。

1. parted を実行します。

   ```
   parted
   ```
   {: pre}

2. ディスク上にパーティションを作成します。
   1. 別途指定されない限り、parted は 1 次ドライブを使用します (ほとんどの場合、これは **/dev/sda** です)。 コマンド **select** を使用して、パーティション化するディスクに切り替えます。 **XXX** は、新しいデバイス名に置き換えてください。

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. **print** を実行して、正しいディスク上にいることを確認します。

      ```
      (parted) print
      ```
      {: pre}

   3. 新規 GPT パーティション表を作成します。 
   
      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. parted は、1 次ディスク・パーティションと論理ディスク・パーティションの作成に使用できます。それぞれで必要なステップは同じです。 新しいパーティションを作成する場合、parted は **mkpart** を使用します。 作成するパーティション・タイプに応じて、**primary** や **logical** などの追加パラメーターを指定できます。
   <br /> **注**: リストされる単位のデフォルトはメガバイト (MB) です。10 GB のパーティションを作成するには、1 から始めて 10000 で終了する必要があります。 必要に応じて、`(parted) unit TB` と入力することにより、サイズ変更単位をテラバイトに変更することもできます。

      ```
      (parted) mkpart
      ```
      {: pre}

   5. **quit** を使用して、parted を終了します。

      ```
      (parted) quit
      ```
      {: pre}

3. 新規パーティション上にファイル・システムを作成します。

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **注**: 上記のコマンドを実行するときは、正しいディスクとパーティションを選択することが重要です。
   パーティション表を表示することで、結果を検証してください。 ファイル・システムの列の下に、ext3 が表示されます。

4. ファイル・システムのマウント・ポイントを作成し、マウントします。
   - パーティション名 PerfDisk を作成するか、ファイル・システムをマウントする場所を作成します。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - パーティション名を使用して、ストレージをマウントします。

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 新しいファイル・システムがリストされることを確認します。

     ```
     df -h
     ```
     {: pre}

5. 新しいファイル・システムをシステムの **/etc/fstab** ファイルに追加して、ブート時の自動マウントを有効にします。
   - 以下の行を **/etc/fstab** の末尾に追加します (ステップ 3 で取得したパーティション名を使用します)。 <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## *NIX OS で MPIO が正しく構成されているかどうかを検証する方法

マルチパスがデバイスをピックアップしているかどうかを確認するには、NETAPP デバイスのみが表示され、2 つのデバイスが存在している必要があります。

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

ディスクが存在すること、同じ ID を持つ 2 つのディスクが存在し、同じ ID と同じサイズの /dev/mapper リストがあることを確認してください。 /dev/mapper デバイスが、マルチパスによってセットアップされるデバイスです。

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

正しくセットアップされていない場合は、以下のような表示になります。
```
No multipath output root@server:~# multipath -l root@server:~#
```

以下を実行すると、ブラックリストに登録されているデバイスが表示されます。
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

fdisk は、`sd*` デバイスのみを表示し、`/dev/mapper` は表示しません。

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
