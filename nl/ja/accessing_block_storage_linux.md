---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL6, multipath, mpio, linux,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Linux での iSCSI LUN への接続
{: #mountingLinux}

ここでの説明は、主に RHEL6 および Centos6 向けのものです。 他の OS に関する注記を追加しましたが、本書は、すべての Linux ディストリビューションをカバーするものでは**ありません**。 別の Linux オペレーティング・システムを使用している場合は、ご使用の特定のディストリビューションの資料を参照し、マルチパスがパスの優先順位として ALUA をサポートしていることを確認してください。
{:note}

例えば、Ubuntu に固有の詳細情報については、[iSCSI Initiator Configuration](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){: external} および [DM-Multipath](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){: external} を参照してください。
{: tip}

開始する前に、{{site.data.keyword.blockstoragefull}} ボリュームにアクセスしているホストが、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}を介して事前に許可されていることを確認してください。
{:important}

1. {{site.data.keyword.blockstorageshort}} のリスト・ページで、新規ボリュームを見つけ、**「アクション」**をクリックします。
2. **「ホストの許可」**をクリックします。
3. リストから、ボリュームにアクセスできるホストを選択し、**「送信」**をクリックします。

あるいは、SLCLI を使用してホストを許可できます。
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```
{:codeblock}

## {{site.data.keyword.blockstorageshort}} ボリュームのマウント
{: #mountLin}

以下のステップを実行して、Linux ベースの {{site.data.keyword.cloud}} コンピューティング・インスタンスをマルチパス入出力 (MPIO) internet Small Computer System Interface (iSCSI) 論理装置番号 (LUN) に接続します。

手順に示されているホスト IQN、ユーザー名、パスワード、およびターゲット・アドレスは、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic/storage){: external}の**「{{site.data.keyword.blockstorageshort}} の詳細」**画面で取得できます。
{: tip}

ファイアウォールをバイパスするように VLAN 上でストレージ・トラフィックを実行することをお勧めします。 ソフトウェア・ファイアウォールを介してストレージ・トラフィックを実行すると、待ち時間が増加し、ストレージ・パフォーマンスに悪影響を与えます。
{:important}

1. iSCSI とマルチパスのユーティリティーをホストにインストールします。
  - RHEL および CentOS

    ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu および Debian

    ```
    sudo apt-get update
   sudo apt-get install multipath-tools
    ```
    {: pre}

2. 必要に応じて、マルチパス構成ファイルを作成または編集します。
  - RHEL 6 および CENTOS 6
    * 次の最小構成の **/etc/multipath.conf** を編集します。

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

    - `iscsi` サービスと `iscsid` サービスを再始動して、変更内容を有効にします。

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - RHEL7 および CentOS7 の場合、OS に組み込みの構成があるため、`multipath.conf` はブランクにできます。
  - `multipath.conf` は `multipath-tools` に組み込まれているため、Ubuntu では使用されません。

3. マルチパス・モジュールをロードして、マルチパス・サービスを開始し、ブート時に開始するように設定します。
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

  - その他のディストリビューションについては、OS ベンダーの資料を確認してください。

4. マルチパスが機能していることを確認します。
  - RHEL 6
    ```
    multipath -l
    ```
    {: pre}

    ブランクが返された場合は、機能しています。
  - CentOS 7
    ```
    multipath -ll
    ```
    {: pre}

    RHEL 7 および CentOS 7 では「No fc_host device」が返される場合がありますが、これは無視してかまいません。

5. {{site.data.keyword.cloud}} コンソールから取得した IQN で `/etc/iscsi/initiatorname.iscsi` ファイルを更新します。 値を小文字で入力します。

   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}

6. {{site.data.keyword.cloud}} コンソールから取得したユーザー名とパスワードを使用して `/etc/iscsi/iscsid.conf` 内の CHAP 設定を編集します。 CHAP 名には大文字を使用します。
   ```
   node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   その他の CHAP 設定はコメント化したままにしてください。 {{site.data.keyword.cloud}} ストレージは、片方向認証のみを使用します。 相互 CHAP を有効にしないこと。
   {:important}

   Ubuntu ユーザーの場合、`iscsid.conf` ファイルを確認する際に、`node.startup` の設定が 手動と自動のどちらになっているかを調べてください。 手動になっている場合は、自動に変更してください。
   {:tip}

7. ブート時に開始するように iSCSI を設定し、今すぐ開始します。
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

   - その他のディストリビューションについては、OS ベンダーの資料を確認してください。

8. {{site.data.keyword.cloud}} コンソールから取得したターゲット IP アドレスを使用して、デバイスをディスカバーします。

   A. iSCSI アレイに対してディスカバリーを実行します。
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
    ```
    {: pre}

   B. iSCSI アレイに自動的にログインするようにホストを設定します。
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

   このコマンドはパスを報告します。

10. デバイスが接続されていることを確認します。 デフォルトでは、デバイスは `/dev/mapper/mpathX` に接続されます。ここで X は、接続されるデバイスの生成 ID です。
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}

  このコマンドは、次の例のような情報を報告します。
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```

  ボリュームはホスト上にマウント済みであり、アクセス可能です。

## ファイル・システムの作成 (オプション)

新しくマウントされたボリュームにファイル・システムを作成するには、以下の手順に従います。 ファイル・システムは、ほとんどのアプリケーションでボリュームを使用するために必要です。 2 TB より小さいドライブの場合は [`fdisk` を使用し](#fdisk)、2 TB より大きいディスクの場合は [`parted` を使用します](#parted)。

### `fdisk` を使用したファイル・システムの作成
{: #fdisk}

1. ディスク名を取得します。
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   返されるディスク名は、`/dev/mapper/XXX` のようになります。

2. ディスク上にパーティションを作成します。

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX は、ステップ 1 で返されたディスク名を表します。 <br />

   さらに下にスクロールして、`fdisk` コマンドの表にリストされているコマンド・コードを表示します。
   {: tip}

3. 新規パーティション上にファイル・システムを作成します。

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - 新規パーティションはディスクと一緒にリストされ、`XXXlp1` のようなものであり、その後にサイズ、タイプ (83)、および Linux が続きます。
   - 次のステップで必要になるので、パーティション名をメモします。 (XXXlp1 がパーティション名を表します。)
   - ファイル・システムを作成します。

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. ファイル・システムのマウント・ポイントを作成し、マウントします。
   - パーティション名 `PerfDisk` を作成するか、ファイル・システムをマウントする場所を作成します。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - パーティション名を使用してストレージをマウントします。
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 新しいファイル・システムがリストされることを確認します。
     ```
     df -h
     ```
     {: pre}

5. 新しいファイル・システムをシステムの `/etc/fstab` ファイルに追加して、ブート時の自動マウントを有効にします。
   - 以下の行を `/etc/fstab` の末尾に追加します (ステップ 3 でメモしたパーティション名を使用します)。 <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### `fdisk` コマンドの表

| コマンド | 結果 |
|-----|-----|
| `Command: n`| パーティションを作成します。 * |
| `Command action: p` | パーティションを 1 次パーティションにします。 |
| `Partition number (1 から 4): 1` | ディスク上のパーティション 1 になります。 |
| `First cylinder (1 から 8877): 1 (default)` | シリンダー 1 から開始します。 |
| `Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)` | 「Enter」を押して最後のシリンダーに移動します。 |
| `Command: t` | パーティションのタイプをセットアップします。 * |
| `Select partition 1.` | 特定のタイプとしてセットアップするパーティション 1 を選択します。 |
| `Hex code: 83` | タイプとして Linux を選択します (83 は Linux の 16 進コードです)。 ** |
| `Command: w` | 新しいパーティション情報をディスクに書き込みます。 ** |
{: caption="表 1 - 左側にコマンド、右側に予期される結果が示されている <code>fdisk</code> コマンドの表。" caption-side="top"}

(`*`) ヘルプが必要な場合、m を入力します。

(`**`) 16 進コードをリストするには、L を入力します。

### `parted` を使用したファイル・システムの作成
{: #parted}

多くの Linux ディストリビューションで、`parted` はプリインストールされています。 ご使用のディストリビューションに含まれていない場合、以下のようにしてインストールできます。
- Debian および Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL および CentOS
  ```
  yum install parted  
  ```
  {: pre}


`parted` を使用してファイル・システムを作成するには、以下のステップに従います。

1. `parted` を実行します。

   ```
   parted
   ```
   {: pre}

2. ディスク上にパーティションを作成します。
   1. 別途指定されない限り、`parted` は 1 次ドライブを使用します (ほとんどの場合、これは `/dev/sda` です)。 コマンド **select** を使用して、パーティション化するディスクに切り替えます。 **XXX** は、新しいデバイス名に置き換えてください。

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. `print` を実行して、正しいディスク上にいることを確認します。

      ```
      print
      ```
      {: pre}

   3. GPT パーティション表を作成します。

      ```
      mklabel gpt
      ```
      {: pre}

   4. `parted` は、1 次ディスク・パーティションと論理ディスク・パーティションの作成に使用できます。それぞれで必要なステップは同じです。 パーティションを作成する場合、`parted` は `mkpart` を使用します。 作成するパーティション・タイプに応じて、**primary** や **logical** などの他のパラメーターを指定できます。<br />

   リストされる単位のデフォルトはメガバイト (MB) です。10 GB のパーティションを作成するには、1 から始めて 10000 で終了する必要があります。 必要に応じて、`unit TB` と入力することにより、サイズ変更単位をテラバイトに変更することもできます。
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. `quit` を使用して、`parted` を終了します。

      ```
      quit
      ```
      {: pre}

3. 新規パーティション上にファイル・システムを作成します。

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   このコマンドを実行するときは、正しいディスクとパーティションを選択することが重要です。<br />パーティション表を表示することで、結果を検証してください。 ファイル・システムの列の下に、ext3 が表示されます。
   {:important}

4. ファイル・システムのマウント・ポイントを作成し、マウントします。
   - パーティション名 `PerfDisk` を作成するか、ファイル・システムをマウントする場所を作成します。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - パーティション名を使用してストレージをマウントします。

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 新しいファイル・システムがリストされることを確認します。

     ```
     df -h
     ```
     {: pre}

5. 新しいファイル・システムをシステムの `/etc/fstab` ファイルに追加して、ブート時の自動マウントを有効にします。
   - 以下の行を `/etc/fstab` の末尾に追加します (ステップ 3 で取得したパーティション名を使用します)。 <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## MPIO 構成の検証
{: #verifyMPIOLinux}

1. マルチパスがデバイスをピックアップしているかどうかを確認するには、デバイスをリストします。 構成が正しい場合は、2 つの NETAPP デバイスのみが表示されます。

  ```
  multipath -l
  ```
  {: pre}

  ```
  root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
  6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
  7:0:0:101 sde 8:64 active undef running
  ```

2. ディスクが存在することを確認してください。 同じ ID を持つ 2 つのディスクが存在し、同じ ID で同じサイズの `/dev/mapper` がリストされている必要があります。 `/dev/mapper` デバイスが、マルチパスによってセットアップされるデバイスです。
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - 構成が正しい場合の出力例

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - 構成が間違っている場合の出力例

    ```
    No multipath output root@server:~# multipath -l root@server:~#
    ```

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. ローカル・ディスクがマルチパス・デバイスに含まれていないことを確認します。 次のコマンドで、ブラックリストにあるデバイスを表示します。
   ```
   multipath -l -v 3 | grep sd <date and time>
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

## {{site.data.keyword.blockstorageshort}} ボリュームのアンマウント
{: #unmountingLin}

1. ファイル・システムをアンマウントします。
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. そのターゲット・ポータルに他のボリュームがない場合は、ターゲットからログアウトできます。
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. そのターゲット・ポータルに他のボリュームがない場合は、ターゲット・ポータル・レコードを削除して、今後ログインを試行できないようにします。
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   詳しくは、[`iscsiadm` の資料](https://linux.die.net/man/8/iscsiadm)を参照してください。
   {:tip}
