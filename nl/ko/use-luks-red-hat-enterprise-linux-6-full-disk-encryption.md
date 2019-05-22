---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block storage, encryption, LUKS, RHEL, Linux, security, auxiliary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Red Hat Enterprise Linux에서 LUKS를 사용하여 전체 디스크 암호화 달성
{: #LUKSencryption}

LUKS(Linux Unified Key Setup-on-disk-format)를 사용하면 Red Hat Enterprise Linux 6 서버에서 파티션을 암호화할 수 있으며 이는 모바일 컴퓨터 및 이동식 매체에서 사용하는 경우에 중요합니다. LUKS를 사용하면 다중 사용자 키로 파티션의 벌크 암호화에 사용되는 마스터 키를 복호화할 수 있습니다.

이 단계에서는 서버가 형식화되지 않았거나 마운트되지 않았으며 암호화되지 않은 새 {{site.data.keyword.blockstoragefull}} 볼륨에 액세스할 수 있는 것으로 가정합니다. Linux 호스트에 {{site.data.keyword.blockstorageshort}} 연결에 대한 자세한 정보는 [Linux에서 iSCSI LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)을 참조하십시오.

[데이터 센터 선택](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)에서 프로비저닝된 {site.data.keyword.blockstorageshort}}는 저장 중에 제공자 관리 암호화를 통해 자동으로 프로비저닝됩니다. 자세한 정보는 [저장 중 데이터 제공자 관리 암호화 보안](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)을 참조하십시오.
{:note}

## LUKS가 수행하는 작업

- 전체 블록 디바이스를 암호화하기 때문에 모바일 디바이스(예: 이동식 스토리지 매체 또는 노트북 디스크 드라이브)의 컨텐츠 보호에 매우 적합합니다.
- 암호화된 블록 디바이스의 기본 컨텐츠는 스왑 디바이스 암호화에 유용한 임의의 컨텐츠입니다. 암호화는 데이터 스토리지에 대해 특수하게 형식화된 블록 디바이스를 사용하는 특정 데이터베이스에도 유용할 수 있습니다.
- 기존 디바이스 맵퍼 커널 서브시스템을 사용합니다.
- 사전 첨부(dictionary attach)에 대해 보호하는 비밀번호 문구 강화를 제공합니다.
- LUKS 디바이스가 다중 키 슬롯을 포함하기 때문에 사용자는 백업 키 또는 비밀번호 문구를 추가할 수 있습니다.


## LUKS가 수행하지 않는 작업

- 다수의(9명 이상) 사용자가 동일한 디바이스에 대해 개별 액세스 키를 가져야 하는 애플리케이션을 허용합니다.
- 파일 레벨 암호화가 필요한 애플리케이션에 대해 작업합니다. 자세한 정보는 [RHEL 보안 안내서](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){: external}를 참조하십시오.

## Endurance{{site.data.keyword.blockstorageshort}}를 사용한 LUKS 암호화 볼륨 설정

데이터 암호화 프로세스는 잠재적으로 성능에 영향을 줄 수 있는 호스트의 로드를 생성합니다.
{:note}

1. 쉘 프롬프트에서 루트로 다음 명령을 입력하여 필요한 패키지를 설치하십시오.   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. 디스크 ID를 가져오십시오.<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. 목록에서 볼륨을 찾으십시오.
4. 블록 디바이스를 암호화하십시오.

   1. 이 명령을 통해 볼륨을 초기화하고 비밀번호 문구를 설정할 수 있습니다. <br/>

      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. YES(모두 대문자)로 응답하십시오.

   3. 이제 디바이스는 암호화된 볼륨으로 표시됩니다.

      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```

5. 볼륨을 열고 맵핑을 작성하십시오.<br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. 비밀번호 문구를 입력하십시오.
7. 맵핑을 확인하고 암호화된 볼륨의 상태를 보십시오.   <br/>
   ```
   # cryptsetup -v status cryptData
   /dev/mapper/cryptData is active.
     type:  LUKS1
     cipher:  aes-cbc-essiv:sha256
     keysize: 256 bits
     device:  /dev/mapper/3600a0980383034685624466470446564
     offset:  4096 sectors
     size:    41938944 sectors
     mode:    read/write
     Command successful
   ```
8. 암호화된 디바이스의 `/dev/mapper/cryptData`에 랜덤 데이터를 기록하십시오. 이 조치를 수행하면 외부에서 해당 데이터는 랜덤 데이터로 표시됩니다. 즉, 사용 패턴을 공개하지 않도록 보호됩니다. 이 단계는 시간이 다소 걸릴 수 있습니다.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. 볼륨을 포맷하십시오.<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. 볼륨을 마운트하십시오.<br/>
   ```
   # mkdir /cryptData
   ```
   {: pre}
   ```
   # mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}
   ```
   # df -H /cryptData
   ```
   {: pre}

### 암호화된 볼륨을 안전하게 마운트 해제 및 종료
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### 기존 LUKS 암호화된 패턴을 다시 마운트 및 마운트
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   # mount /dev/mapper/cryptData /cryptData
   # df -H /cryptData
   # lsblk
   NAME                                       MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
   xvdb                                       202:16   0    2G  0 disk
   └─xvdb1                                    202:17   0    2G  0 part  [SWAP]
   xvda                                       202:0    0   25G  0 disk
   ├─xvda1                                    202:1    0  256M  0 part  /boot
   └─xvda2                                    202:2    0 24.8G  0 part  /
   sda                                          8:0    0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   ```
