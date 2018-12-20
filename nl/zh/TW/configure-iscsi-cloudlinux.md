---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 在 CloudLinux 上連接至 MPIO iSCSI LUN

請遵循下列指示，以在 CloudLinux Server 6.10 版上按裝具有多路徑的 iSCSI LUN。

開始之前，請確定存取 {{site.data.keyword.blockstoragefull}} 磁區的主機先前已透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 獲得授權。
{:tip}

1. 登入 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window}。
2. 從 {{site.data.keyword.blockstorageshort}} 的清單頁面中，找出新的磁區，然後按一下**動作**。
3. 按一下**授權主機**。
4. 從清單中，選取可以存取磁區的主機，然後按一下**提交**。
5. 記下「主機 IQN」、使用者名稱、密碼及目標位址。

最好是在 VLAN 上執行儲存空間資料流量，這樣會略過防火牆。透過軟體防火牆執行儲存空間資料流量，會增加延遲，而且會對儲存空間效能造成不利的影響。
{:important}

## 裝載 {{site.data.keyword.blockstorageshort}} 磁區

1. 在主機上安裝 iSCSI 和多路徑公用程式，並啟動它們。
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

2. 建立或編輯您的配置檔。
   - 更新 '/etc/multipath.conf'。<br/>**附註**：黑名單下的所有資料都必須是您的系統所特有。
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

   - 透過新增使用者名稱、密碼，更新 CHAP 設定 `/etc/iscsi/iscsid.conf`。

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

     請對 CHAP 名稱使用大寫字體。請將其他 CHAP 設定保持註解狀態。{{site.data.keyword.BluSoftlayer_full}} 儲存空間僅會使用單向鑑別。請勿啟用 Mutual CHAP。
     {:important}


3. 重新啟動 `iscsi` 及 `multipathd` 服務。
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}

   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}

4. 使用從 {{site.data.keyword.slportal}} 取得的「目標」IP 位址來探索裝置。

     A. 針對 iSCSI 陣列執行探索。
       ```
       iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
       ```
       {: pre}

        範例輸出
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. 將主機設為自動登入 iSCSI 陣列。
       ```
       iscsiadm -m node -L automatic
       ```
       {: pre}

5. 驗證主機已登入 iSCSI 陣列，並維護其階段作業。
   ```
   iscsiadm -m session
   ```
   {: pre}

   範例輸出
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. 驗證裝置已連接。
   ```
   fdisk -l 
   ```
   {: pre}

   範例輸出
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

   磁區現在已裝載且可在主機上存取。

7. 列出裝置，以驗證 MPIO 是否配置正確。如果配置正確，則只會顯示兩台 NETAPP 裝置。

   ```
   # multipath -l
   ```
   {: pre}

   範例輸出
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
