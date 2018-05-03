---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Plesk를 사용하여 백업을 위해 {{site.data.keyword.blockstorageshort}} 구성

이 문서에서는 Plesk에서 백업하기 위해 {{site.data.keyword.blockstoragefull}}를 구성하는 지시사항을 제공합니다. root 또는 sudo SSH 및 전체 관리 레벨 Plesk 액세스가 사용 가능한 것으로 가정합니다. 이 예제는 CentOS7 호스트를 기반으로 합니다.

**참고**: 백업 및 복원에 대한 Plesk 문서는 [여기](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}에서 제공됩니다.

1. SSH를 통해 호스트에 연결하십시오.

2. 마운트 위치 대상이 존재해야 합니다. <br />
   **참고**: Plesk에는 백업을 저장하는 두 개의 옵션이 있습니다. 하나는 Plesk 서버에 있는 백업 스토리지인 내부 Plesk 스토리지이며 다른 하나는 웹 또는 사용자의 로컬 네트워크에서 일부 외부 서버에 있는 백업 스토리지인 외부 FTP 스토리지입니다. Plesk 박스에서는 일반적으로 내부 백업은 `/var/lib/psa/dumps`에 저장되고 `/tmp`를 임시 디렉토리로 사용합니다. 이 예제에서는 임시 디렉토리는 로컬로 유지하지만 덤프 디렉토리는 STaaS 대상(`/backup/psa/dumps`)으로 이동합니다. FTP 사용자 신임 정보는 필요하지 않습니다.
   
3. [Linux에서 MPIO iSCSI LUN에 연결](accessing_block_storage_linux.html)에서 설명하는 대로 {{site.data.keyword.blockstorageshort}}를 구성하십시오. {{site.data.keyword.blockstorageshort}}를 `/backup`에 마운트하고 부팅 시에 마운트되도록 `/etc/fstab`를 구성하십시오.

4. **선택사항**: 기존 백업을 새 스토리지로 복사하십시오. 예를 들어, `rsync`를 사용하십시오.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **참고**: 이 명령은 가능한 많이 보존한 상태(하드링크는 제외)로 압축된 데이터를 전송하며 맨 끝에 간략한 설명이 포함된 전송 중인 파일 관련 정보를 제공합니다.
    
5. 새 대상에서 `DUMP_D` 값을 지시하도록 `/etc/psa/psa.conf`를 편집하십시오. 
    -  `DUMP_D /backup/psa/dumps`와 같이 표시되어야 합니다. 

6. **선택사항**: 특정 유스 케이스 및 비즈니스 요구로 지시되는 대로 서버에서 오래된 스토리지는 제거하고 계정에서 취소하십시오.


