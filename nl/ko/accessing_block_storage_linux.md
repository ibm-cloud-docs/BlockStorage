---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL6, multipath, mpio, linux,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Linux에서 iSCSI LUN에 연결
{: #mountingLinux}

다음 지시사항은 주로 RHEL6 및 CentOS6에 해당됩니다. 다른 OS에 대한 참고사항이 추가되었지만 이 문서에는 모든 Linux 배포판이 포함되지는 **않습니다**. 다른 Linux 운영 체제를 사용 중인 경우에는 사용자에 해당하는 배포 문서를 참조하고 다중 경로가 경로 우선순위에 대해 ALUA를 지원하는지 확인하십시오.
{:note}

예를 들어, Ubuntu 세부사항에 대한 자세한 정보는 [iSCSI 이니시에이터 구성](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){: external} 및 [DM-Multipath](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){: external}를 참조하십시오.
{: tip}

시작하기 전에 {{site.data.keyword.blockstoragefull}} 볼륨에 액세스하는 호스트의 권한이 [{{site.data.keyword.cloud}} 콘솔](https://{DomainName}/classic){: external}을 통해 이전에 부여되었는지 확인하십시오.
{:important}

1. [{{site.data.keyword.cloud_notm}} 콘솔](https://{DomainName}/){: external}에 로그인하십시오. **메뉴**에서 **클래식 인프라**를 선택하십시오.
2. **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
3. 새 볼륨을 찾은 후 **...**를 클릭하십시오.
4. **호스트 권한 부여**를 클릭하십시오.
5. 사용 가능한 디바이스 또는 IP 주소의 목록을 확인하려면 먼저 디바이스 유형에 따라 액세스 권한을 부여할 것인지 또는 서브넷에 따라 액세스 권한을 부여할 것인지 여부를 선택하십시오.
   - 디바이스를 선택하는 경우 Bare Metal Server 또는 Virtual Server 인스턴스 중에서 선택할 수 있습니다.
   - IP 주소를 선택하는 경우 먼저 호스트가 상주하는 서브넷을 선택하십시오.
6. 필터링된 목록에서 볼륨에 액세스할 수 있는 호스트를 하나 이상 선택하고 **저장**을 클릭하십시오.

또는 SLCLI를 통해 호스트에 권한을 부여할 수 있습니다.
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

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트
{: #mountLin}

다음 단계를 완료하여 Linux 기반 {{site.data.keyword.cloud}} 컴퓨팅 인스턴스를 다중 경로 입출력(I/O)(MPIO) iSCSI(internet Small Computer System Interface) 논리 장치 번호(LUN)에 연결하십시오.

지시사항에서 참조되는 호스트 IQN, 사용자 이름, 비밀번호 및 대상 주소는 [{{site.data.keyword.cloud}} 콘솔](https://{DomainName}/classic/storage){: external}에서 **{{site.data.keyword.blockstorageshort}} 세부사항** 화면에서 얻을 수 있습니다.
{: tip}

방화벽을 우회하는 VLAN을 통해 스토리지 트래픽을 실행하는 것이 가장 좋습니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 실행하면 대기 시간이 늘어나서 결국 스토리지 성능이 저하됩니다.
{:important}

1. 호스트에 iSCSI 및 다중 경로 유틸리티를 설치하십시오.
  - RHEL 및 CentOS

    ```
   yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu 및 Debian

    ```
   sudo apt-get update
   sudo apt-get install multipath-tools
    ```
    {: pre}

2. 필요하면 다중 경로 구성 파일을 작성하거나 편집하십시오.
  - RHEL 6 및 CENTOS 6
    * 다음의 최소 구성으로 **/etc/multipath.conf**를 편집하십시오.

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

    - 변경사항이 적용되도록 `iscsi` 및 `iscsid` 서비스를 다시 시작하십시오.

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - RHEL7 및 CentOS7의 경우, OS에 기본 제공된 구성이 있으므로 `multipath.conf`는 공백일 수 있습니다.
  - `다중 경로 도구`에 빌드되어 있으므로 Ubuntu는 `multipath.conf`를 사용하지 않습니다.

3. 다중 경로 모듈을 로드하고 다중 경로 서비스를 시작한 후 부팅 시에 시작되도록 설정하십시오.
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

  - 다른 배포판의 경우, OS 공급업체 문서를 참조하십시오.

4. 다중 경로가 작동 중인지 확인하십시오.
  - RHEL 6
    ```
   multipath -l
    ```
    {: pre}

공백을 리턴하면 작동 중인 것입니다.
  - CentOS 7
    ```
     multipath -ll
    ```
    {: pre}

    RHEL 7 및 CentOS 7에서 "No fc_host device"를 리턴할 수 있지만 이는 무시해도 됩니다.

5. `/etc/iscsi/initiatorname.iscsi` 파일을 {{site.data.keyword.cloud}} 콘솔의 IQN으로 업데이트하십시오. 값은 소문자로 입력하십시오.

   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}

6. {{site.data.keyword.cloud}} 콘솔의 사용자 이름 및 비밀번호를 사용하여 `/etc/iscsi/iscsid.conf`에서 CHAP 설정을 편집하십시오. CHAP 이름에는 대문자를 사용하십시오.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   다른 CHAP 설정은 주석 처리된 상태로 두십시오. {{site.data.keyword.cloud}} 스토리지는 단방향 인증만 사용합니다. 상호 CHAP를 사용하지 마십시오.
   {:important}

   Ubuntu 사용자의 경우 `iscsid.conf` 파일을 확인할 때 `node.startup` 설정이 수동인지 자동인지 확인하십시오. 수동인 경우 자동으로 변경하십시오.
   {:tip}

7. iSCSI가 부팅 시에 시작되도록 설정하고 지금 시작하십시오.
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

   - 다른 배포판의 경우, OS 공급업체 문서를 참조하십시오.

8. {{site.data.keyword.cloud}} 콘솔에서 확보한 대상 IP 주소를 사용하여 디바이스를 검색하십시오.

   A. iSCSI 배열에 대해 검색을 실행하십시오..
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
    ```
    {: pre}

   B. iSCSI 배열에 자동 로그인되도록 호스트를 설정하십시오.
    ```
         iscsiadm -m node -L automatic
    ```
    {: pre}

9. 호스트가 iSCSI 배열에 로그인되어 해당 세션을 유지보수하는지 확인하십시오.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   이 명령은 경로를 보고합니다.

10. 디바이스가 연결되었는지 확인하십시오. 기본적으로, 디바이스는 `/dev/mapper/mpathX`에 연결되며 여기서 X는 연결된 디바이스에 대해 생성된 ID입니다.
    ```
   fdisk -l | grep /dev/mapper
    ```
    {: pre}

    이 명령은 다음 예와 비슷한 내용을 보고합니다.
    ```
        Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```

    이제 볼륨이 마운트되어 호스트에서 액세스 가능합니다.

## 파일 시스템 작성(선택사항)

다음 단계에 따라 새로 마운트된 볼륨에서 파일 시스템을 작성하십시오. 파일 시스템은 볼륨을 사용하는 대부분의 애플리케이션에서 필요합니다. [2TB 미만인 드라이브의 경우 `fdisk`](#fdisk)를 사용하고 [2TB보다 큰 디스크의 경우 `parted`](#parted)를 사용하십시오.

### `fdisk`로 파일 시스템 작성
{: #fdisk}

1. 디스크 이름을 가져오십시오.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   리턴되는 디스크 이름은 `/dev/mapper/XXX`와 비슷하게 표시됩니다.

2. 디스크에서 파티션을 작성하십시오.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX는 1단계에서 리턴된 디스크 이름을 나타냅니다. <br />

   더 아래로 스크롤하여 `fdisk` 명령 표에 나열된 명령 코드를 찾으십시오.
   {: tip}

3. 새 파티션에서 파일 시스템을 작성하십시오.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - 새 파티션은 `XXXlp1`과 유사하게 디스크와 함께 나열되며 크기, 유형(83), Linux 다음에 표시됩니다.
   - 파티션 이름은 다음 단계에서 필요하기 때문에 기록해 두십시오. (XXXlp1이 파티션 이름입니다.)
   - 파일 시스템을 작성하십시오.

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. 파일 시스템에 대한 마운트 지점을 작성하고 이를 마운트하십시오.
   - 파티션 이름 `PerfDisk` 또는 파일 시스템을 마운트하려는 위치를 작성하십시오.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - 파티션 이름을 사용하여 스토리지를 마운트하십시오.
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 새 파일 시스템이 나열되는지 확인하십시오.
     ```
     df -h
     ```
     {: pre}

5. 새 파일 시스템을 시스템의 `/etc/fstab` 파일에 추가하여 부팅 시에 자동 마운팅이 사용되도록 설정하십시오.
   - 다음 행을 `/etc/fstab`의 끝에 추가하십시오(3단계의 파티션 이름 사용). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### `fdisk` 명령 표

|명령 |결과 |
|-----|-----|
| `Command: n`|파티션을 작성합니다. * |
| `Command action: p` |파티션이 1차 파티션이 됩니다. |
| `Partition number (1-4): 1` |디스크에서 파티션 1이 됩니다. |
| `First cylinder (1-8877): 1 (default)` |실린더 1에서 시작합니다. |
| `Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)` |Enter를 눌러 마지막 실린더로 이동하십시오. |
| `Command: t` |파티션 유형을 설정합니다. * |
| `Select partition 1.` |파티션 1이 특정 유형으로 설정되도록 선택합니다. |
| `Hex code: 83` |유형으로 Linux를 선택합니다(83은 Linux의 16진 코드). ** |
| `Command: w` |디스크에 새 파티션 정보를 기록합니다. ** |
{: caption="표 1 - <code>fdisk</code> 명령 표의 왼쪽에는 명령이 표시되고 오른쪽에는 예상되는 결과가 표시됩니다." caption-side="top"}

(`*`)도움말을 보려면 m을 입력하십시오.

(`**`)16진 코드를 나열하려면 L을 입력하십시오.

### `parted`로 파일 시스템 작성
{: #parted}

많은 Linux 배포판에서 `parted`는 미리 설치되어 제공됩니다. distro에 포함되어 있지 않다면 다음으로 설치할 수 있습니다.
- Debian 및 Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL 및 CentOS
  ```
  yum install parted  
  ```
  {: pre}


`parted`를 사용하여 파일 시스템을 작성하려면 다음 단계를 수행하십시오.

1. `parted`를 실행하십시오.

   ```
   parted
   ```
   {: pre}

2. 디스크에서 파티션을 작성하십시오.
   1. 다르게 지정된 경우가 아니면 `parted`에서는 기본 드라이브를 사용합니다. 기본 드라이브는 대부분의 경우 `/dev/sda`입니다. **select** 명령을 사용하여 파티션하려는 디스크로 전환하십시오. **XXX**는 새 디바이스 이름으로 대체하십시오.

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. `print`를 실행하여 올바른 디스크에서 작업 중인지 확인하십시오.

      ```
      print
      ```
      {: pre}

   3. GPT 파티션 표를 작성하십시오.

      ```
      mklabel gpt
      ```
      {: pre}

   4. `Parted`는 기본 및 논리 디스크 파티션 작성에 사용할 수 있으며 수행 단계는 동일합니다. 파티션을 작성하기 위해 `parted`에서는 `mkpart`를 사용합니다. 작성하려는 파티션 유형에 따라 **primary** 또는 **logical**과 같은 기타 매개변수를 제공할 수도 있습니다.<br />

   나열된 단위는 기본적으로 메가바이트(MB) 단위입니다. 10GB 파티션을 작성하려면 1에서 시작하여 10000에서 끝납니다. 원하면 `unit TB`를 입력하여 크기 단위를 TB로 변경할 수도 있습니다.
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. `quit`로 `parted`를 종료하십시오.

      ```
      quit
      ```
      {: pre}

3. 새 파티션에서 파일 시스템을 작성하십시오.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   이 명령을 실행할 때는 반드시 올바른 디스크와 파티션을 선택해야 합니다.<br />파티션 표를 인쇄하여 결과를 확인하십시오. 파일 시스템 열에 ext3이 표시되어야 합니다.
   {:important}

4. 파일 시스템에 대한 마운트 지점을 작성하고 이를 마운트하십시오.
   - 파티션 이름 `PerfDisk` 또는 파일 시스템을 마운트하려는 위치를 작성하십시오.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - 파티션 이름을 사용하여 스토리지를 마운트하십시오.

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 새 파일 시스템이 나열되는지 확인하십시오.

     ```
     df -h
     ```
     {: pre}

5. 새 파일 시스템을 시스템의 `/etc/fstab` 파일에 추가하여 부팅 시에 자동 마운팅이 사용되도록 설정하십시오.
   - 다음 행을 `/etc/fstab`의 끝에 추가하십시오(3단계의 파티션 이름 사용). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## MPIO 구성 확인
{: #verifyMPIOLinux}

1. 다중 경로에서 디바이스를 선택하는지 확인하려면 디바이스를 나열하십시오. 올바르게 구성된 경우 두 개의 NETAPP 디바이스만 표시됩니다.

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

2. 디스크가 있는지 확인하십시오. ID가 동일한 2개의 디스크와 ID가 동일한 같은 크기의 `/dev/mapper` 목록이 있어야 합니다. `/dev/mapper` 디바이스는 다중 경로에서 설정하는 디바이스입니다.
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - 올바른 구성의 예제 출력.

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - 올바르지 않은 구성의 예제 출력.

    ```
No multipath output root@server:~# multipath -l root@server:~#
    ```

    ```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. 로컬 디스크가 다중 경로 디바이스에 포함되지 않는지 확인하십시오. 다음 명령은 블랙리스트에 있는 디바이스를 표시합니다.
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

## {{site.data.keyword.blockstorageshort}} 볼륨 마운트 해제
{: #unmountingLin}

1. 파일 시스템을 마운트 해제하십시오.
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. 해당되는 대상 포털에 기타 볼륨이 없으면 대상에서 로그아웃할 수 있습니다.
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. 해당되는 대상 포털에 기타 볼륨이 없으면 추가 로그인 시도의 방지를 위해 대상 포털 레코드를 삭제하십시오.
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   자세한 정보는 [`iscsiadm` 매뉴얼](https://linux.die.net/man/8/iscsiadm)을 참조하십시오.
   {:tip}
