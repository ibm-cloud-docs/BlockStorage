---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# cPanel을 사용하여 백업을 위해 {{site.data.keyword.blockstorageshort}} 구성

이 문서는 cPanel을 통해 {{site.data.keyword.blockstoragefull}}에 저장되는 백업 구성 지시사항을 제공합니다. root 또는 sudo SSH 및 전체 WHM(WebHost Manager) 액세스가 사용 가능한 것으로 가정합니다. 이 예제는 **CentOS 7** 호스트를 기반으로 합니다. 

**참고**: 백업 디렉토리 구성을 위한 cPanel 문서는 [여기](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}에서 제공됩니다. 

1. SSH를 통해 호스트에 연결하십시오. 

2. 마운트 위치 대상이 존재해야 합니다. <br />
   **참고**: 기본적으로 cPanel 시스템은 백업 파일을 로컬의 `/backup` 디렉토리에 저장합니다. 이 문서에서는 `/backup`이 이미 존재하고 백업을 포함하고 있다고 가정하기 때문에 `/backup2`를 새 마운트 지점으로 사용합니다. 
   
3. [Linux에서 MPIO iSCSI LUN에 연결](accessing_block_storage_linux.html)에서 설명하는 대로 {{site.data.keyword.blockstorageshort}}를 구성하십시오. `/backup2`에 마운트하고 부팅 시에 마운트가 사용되도록 이를 `/etc/fstab`에서 구성하십시오. 

4. **선택사항**: 기존 백업을 새 스토리지로 복사하십시오. 예를 들어, `rsync`를 사용하십시오. 
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **참고**: 이 명령은 가능한 많이 보존한 상태(하드링크는 제외)로 압축된 데이터를 전송하며 맨 끝에 간략한 설명이 포함된 전송 중인 파일 관련 정보를 제공합니다. 
    
5.  WebHost Manager에 로그인하고 **홈** > **백업** > **백업 구성**을 통해 백업 구성을 탐색하십시오. 

6.  백업이 새 마운트 지점에 저장되도록 구성을 편집하십시오.  
    - /backup/ 디렉토리 위치에 새 위치에 대한 절대 경로를 입력하여 기본 백업 디렉토리를 변경하십시오.  
    - **백업 드라이브 마운트 사용**을 선택하십시오. 이렇게 설정하면 백업 구성 프로세스가 `/etc/fstab` 파일에서 백업 마운트(`/backup2`)를 확인합니다. <br /> **참고**: 스테이징 디렉토리와 동일한 이름의 마운트가 있는 경우, 백업 구성 프로세스는 드라이브를 마운트하고 정보를 마운트에 백업합니다. 백업 프로세스가 완료되면 드라이브 마운트가 해제됩니다.  

7. **백업 구성** 인터페이스 맨 아래에 있는 **구성 저장**을 클릭하여 변경사항을 적용하십시오. 

8. **선택사항**: 특정 유스 케이스 및 비즈니스 요구로 지시되는 대로 서버에서 오래된 스토리지는 제거하고 계정에서 취소하십시오. 

