---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: IBM Block Storage, MPIO, iSCSI, LUN, mount secondary storage, mount storage in CloudLinux

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# CloudLinux에서 iSCSI LUN에 연결
{: #mountingCloudLinux}

다음 지시사항을 따라 CloudLinux Server 릴리스 6.10에서 다중 경로를 사용하여 iSCSI LUN을 설치하십시오.

시작하기 전에 {{site.data.keyword.blockstoragefull}} 볼륨에 액세스하는 호스트의 권한이 [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}을 통해 이전에 부여되었는지 확인하십시오.
{:tip}

1. [{{site.data.keyword.slportal}} ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://control.softlayer.com/){:new_window}에 로그인하십시오.
2. {{site.data.keyword.blockstorageshort}} 나열 페이지에서 새 볼륨을 찾고 **조치**를 클릭하십시오.
3. **호스트 권한 부여**를 클릭하십시오.
4. 목록에서 볼륨에 대한 액세스 권한이 있는 호스트를 선택하고 **제출**을 클릭하십시오.
5. 호스트 IQN, 사용자 이름, 비밀번호 및 대상 주소를 기록해 두십시오.

또는 SLCLI를 통해 호스트에 권한을 부여할 수 있습니다.
```
# slcli block access-authorize --help
사용법: slcli block access-authorize [OPTIONS] VOLUME_ID

옵션:
  -h, --hardware-id TEXT    권한 부여할 하나의 SoftLayer_Hardware ID
  -v, --virtual-id TEXT     권한 부여할 하나의 SoftLayer_Virtual_Guest ID
  -i, --ip-address-id TEXT  권한 부여할 하나의 SoftLayer_Network_Subnet_IpAddress
                            ID
  --ip-address TEXT         권한 부여할 IP 주소
  --help                    이 메시지를 표시하고 종료합니다.
```
{:codeblock}

방화벽을 우회하는 VLAN을 통해 스토리지 트래픽을 실행하는 것이 가장 좋습니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 실행하면 대기 시간이 늘어나서 결국 스토리지 성능이 저하됩니다.
{:important}

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트
{: #mountingCloudLin}

1. 호스트에서 iSCSI 및 다중 경로 유틸리티를 설치하고 활성화하십시오.
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

2. 구성 파일을 작성하거나 편집하십시오.
   - '/etc/multipath.conf'를 업데이트하십시오. <br/>**참고** - 블랙리스트 아래 모든 데이터는 시스템마다 고유해야 합니다.
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

   - 사용자 이름과 비밀번호를 추가하여 CHAP 설정 `/etc/iscsi/iscsid.conf`를 업데이트하십시오.

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

     CHAP 이름에는 대문자를 사용하십시오. 다른 CHAP 설정은 주석 처리된 상태로 두십시오. {{site.data.keyword.BluSoftlayer_full}} 스토리지는 단방향 인증만 사용합니다. 상호 CHAP를 사용하지 마십시오.
     {:important}


3. `iscsi` 및 `multipathd` 서비스를 다시 시작하십시오.
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}

   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}

4. {{site.data.keyword.slportal}}에서 확보한 대상 IP 주소를 사용하여 디바이스를 검색하십시오.

     A. iSCSI 배열에 대해 검색을 실행하십시오..
       ```
            iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
       ```
       {: pre}

        출력 예제
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. iSCSI 배열에 자동 로그인되도록 호스트를 설정하십시오.
       ```
            iscsiadm -m node -L automatic
       ```
       {: pre}

5. 호스트가 iSCSI 배열에 로그인되어 해당 세션을 유지보수하는지 확인하십시오.
   ```
   iscsiadm -m session
   ```
   {: pre}

   출력 예제
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. 디바이스가 연결되었는지 확인하십시오.
   ```
   fdisk -l
   ```
   {: pre}

   출력 예제
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

     이제 볼륨이 마운트되어 호스트에서 액세스 가능합니다.

7. 디바이스를 나열하여 MPIO가 제대로 구성되었는지 확인하십시오. 올바르게 구성된 경우 두 개의 NETAPP 디바이스만 표시됩니다.

   ```
# multipath -l
   ```
   {: pre}

   출력 예제
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
