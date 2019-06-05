---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block storage, Plesk, backups, mountpoint, ISCSI

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Plesk로 백업을 위한 {{site.data.keyword.blockstorageshort}} 구성
{: #PleskBackups}

이 지시사항을 사용하여 Plesk를 통해 백업하도록 {{site.data.keyword.blockstoragefull}}를 구성할 수 있습니다. root 또는 sudo SSH 및 전체 관리 레벨 Plesk 액세스가 사용 가능한 것으로 가정합니다. 해당 지시사항은 CentOS 7 호스트를 기반으로 합니다.

자세한 정보는 [백업 및 복원에 관한 Plesk 문서](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){: external}를 참조하십시오.
{:tip}

1. SSH를 통해 호스트에 연결하십시오.
2. 마운트 지점 대상이 존재해야 합니다.

   Plesk에는 백업을 저장하는 두 가지 옵션이 있습니다. 한 옵션은 Plesk 서버에 있는 백업 스토리지인 내부 Plesk 스토리지입니다. 다른 옵션은 웹 또는 사용자의 로컬 네트워크에서 일부 외부 서버에 있는 백업 스토리지인 외부 FTP 스토리지입니다. Plesk 박스에서는 일반적으로 내부 백업은 `/var/lib/psa/dumps`에 저장되고 `/tmp`를 임시 디렉토리로 사용합니다. 이 예제에서는 임시 디렉토리는 로컬로 유지하지만 덤프 디렉토리는 {{site.data.keyword.blockstorageshort}} 대상(`/backup/psa/dumps`)으로 이동합니다. FTP 사용자 인증 정보는 필요하지 않습니다.
   {:note}   
3. [Linux에서 MPIO iSCSI LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux)에서 설명하는 대로 {{site.data.keyword.blockstorageshort}}를 구성하십시오. {{site.data.keyword.blockstorageshort}}를 `/backup`에 마운트하고 시작 시에 마운트되도록 `/etc/fstab`를 구성하십시오.
4. **선택사항**: 기존 백업을 새 스토리지로 복사하십시오. `rsync`를 사용할 수 있습니다.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    이 명령은 데이터를 압축하고 전송하며 최대한 많은 양을 유지합니다(하드링크의 경우는 제외). 맨 끝에 간략한 설명이 포함된 전송 중인 파일 관련 정보를 제공합니다.
    {:tip}    
5. 새 대상에서 `DUMP_D` 값을 지시하도록 `/etc/psa/psa.conf`를 편집하십시오.
    - `DUMP_D /backup/psa/dumps`로 표시됩니다.
6. **선택사항**: 특정 유스 케이스 및 비즈니스 요구사항에 따라 서버에서 오래된 스토리지는 제거하고 계정에서 취소하십시오.
