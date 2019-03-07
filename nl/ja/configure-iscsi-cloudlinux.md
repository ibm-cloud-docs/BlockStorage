---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# CloudLinux での iSCSI LUN への接続
{: #mountingCloudLinux}

以下の手順に従って、CloudLinux Server リリース 6.10 にマルチパスを使用して iSCSI LUN をインストールします。

開始する前に、{{site.data.keyword.blockstoragefull}} ボリュームにアクセスしているホストが、[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} を介して以前に許可されていることを確認してください。
{:tip}

1. [{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} にログインします。
2. {{site.data.keyword.blockstorageshort}} のリスト・ページで、新規ボリュームを見つけ、**「アクション」**をクリックします。
3. **「ホストの許可」**をクリックします。
4. リストから、ボリュームにアクセスできるホストを選択し、**「送信」**をクリックします。
5. ホスト IQN、ユーザー名、パスワード、およびターゲット・アドレスを書き留めます。

あるいは、SLCLI を使用してホストを許可できます。
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```
{:codeblock}

ファイアウォールをバイパスするように VLAN 上でストレージ・トラフィックを実行することをお勧めします。 ソフトウェア・ファイアウォールを介してストレージ・トラフィックを実行すると、待ち時間が増加し、ストレージ・パフォーマンスに悪影響を与えます。
{:important}

## {{site.data.keyword.blockstorageshort}} ボリュームのマウント
{: #mountingCloudLin}

1. ホストに iSCSI およびマルチパス・ユーティリティーをインストールしてアクティブにします。
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

2. 構成ファイルを作成または編集します。
   - '/etc/multipath.conf' を更新します。 <br/>**注** - ブラックリスト下のすべてのデータは、システムに固有でなければなりません。
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

   - ユーザー名、パスワードを追加して、CHAP 設定 `/etc/iscsi/iscsid.conf` を更新します。

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

     CHAP 名には大文字を使用します。 その他の CHAP 設定はコメント化したままにしてください。 {{site.data.keyword.BluSoftlayer_full}} ストレージは、片方向認証のみを使用します。 相互 CHAP を有効にしないこと。
     {:important}


3. `iscsi` サービスおよび `multipathd` サービスを再始動します。
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}

   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}

4. {{site.data.keyword.slportal}} から取得したターゲット IP アドレスを使用して、デバイスをディスカバーします。

     A. iSCSI アレイに対してディスカバリーを実行します。
       ```
       iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
       ```
       {: pre}

        出力例
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. iSCSI アレイに自動的にログインするようにホストを設定します。
       ```
       iscsiadm -m node -L automatic
       ```
       {: pre}

5. ホストが iSCSI アレイにログインしており、セッションを維持していることを確認します。
   ```
   iscsiadm -m session
   ```
   {: pre}

   出力例
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. デバイスが接続されていることを確認します。
   ```
   fdisk -l
   ```
   {: pre}

   出力例
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

   ボリュームはホスト上にマウント済みであり、アクセス可能です。

7. デバイスをリストして MPIO が正しく構成されているかどうかを確認します。 構成が正しい場合は、2 つの NETAPP デバイスのみが表示されます。

   ```
   # multipath -l
   ```
   {: pre}

   出力例
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
