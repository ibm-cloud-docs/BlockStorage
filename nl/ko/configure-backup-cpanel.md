---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# cPanel로 백업을 위한 {{site.data.keyword.blockstorageshort}} 구성

다음 지시사항을 사용하여 cPanel의 백업을 {{site.data.keyword.blockstoragefull}}에 저장하도록 구성하십시오. root 또는 sudo SSH 및 전체 WHM(WebHost Manager) 액세스가 사용 가능한 것으로 가정합니다. 해당 지시사항은 **CentOS 7** 호스트를 기반으로 합니다.

자세한 정보는 [cPanel - 백업 디렉토리 구성 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}을 참조하십시오.
{:tip}

1. SSH를 통해 호스트에 연결하십시오.

2. 마운트 지점 대상이 존재해야 합니다. <br />
   기본적으로, cPanel 시스템은 `/backup` 디렉토리에 백업 파일을 로컬로 저장합니다. 이 문서에서는 `/backup`이 있고 백업을 포함한다고 가정하며 `/backup2`를 새 마운트 지점으로 사용합니다.
   {:note}

3. [Linux에서 MPIO iSCSI LUN에 연결](accessing_block_storage_linux.html)에서 설명하는 대로 {{site.data.keyword.blockstorageshort}}를 구성하십시오. `/backup2`에 마운트하고 시작 시에 마운트가 사용되도록 이를 `/etc/fstab`에서 구성하십시오.

4. **선택사항**: 기존 백업을 새 스토리지로 복사하십시오. `rsync`를 사용할 수 있습니다.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    이 명령은 데이터를 압축하고 전송하며, 이는 최대한 많은 양을 유지합니다(하드링크의 경우는 제외). 맨 끝에 간략한 설명이 포함된 전송 중인 파일 관련 정보를 제공합니다.
    {:tip}

5. WHM에 로그인하고 **홈** > **백업** > **백업 구성**을 클릭하여 백업 구성으로 이동하십시오.

6. 백업이 새 마운트 지점에 저장되도록 구성을 편집하십시오.
    - /backup/ 디렉토리 위치에 새 위치에 대한 절대 경로를 입력하여 기본 백업 디렉토리를 변경하십시오.
    - **백업 드라이브 마운트 사용**을 선택하십시오. 이렇게 설정하면 백업 구성 프로세스가 `/etc/fstab` 파일에서 백업 마운트(`/backup2`)를 확인합니다. <br />

    스테이징 디렉토리와 동일한 이름의 마운트가 존재하는 경우, 백업 구성 프로세스는 드라이브를 마운트하고 정보를 드라이브에 백업합니다. 백업 프로세스가 완료되면 드라이브 마운트가 해제됩니다.
    {:note}

7. **구성 저장**을 클릭하여 변경사항을 적용하십시오.

8. **선택사항**: 특정 유스 케이스 및 비즈니스 요구사항에 따라 서버에서 오래된 스토리지는 제거하고 계정에서 취소하십시오.
