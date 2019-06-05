---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 기존 {{site.data.keyword.blockstorageshort}}을 개선된 {{site.data.keyword.blockstorageshort}}로 업그레이드
{: #migratestorage}

개선된 {{site.data.keyword.blockstoragefull}}는 이제 데이터 센터 선택에서 사용할 수 있습니다. 업그레이드된 데이터 센터 및 사용 가능한 기능(예: 조정 가능한 IOPS 속도 및 확장 가능한 볼륨) 목록을 보려면 [여기](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)를 클릭하십시오. 제공자 관리 암호화된 스토리지에 대한 자세한 정보는 [{{site.data.keyword.blockstorageshort}} 저장 시 암호화](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)를 참조하십시오.

선호하는 마이그레이션 경로는 두 LUN 모두에 동시 연결되고 임의의 LUN에서 다른 LUN으로 데이터를 직접 전송합니다. 스펙은 운영 체제 및 데이터가 복사 오퍼레이션 중에 변경되는지 여부에 따라 다릅니다.

이 경우, 이미 호스트에 암호화되지 않은 LUN을 연결했다고 가정합니다. 그렇지 않으면, 이 태스크를 완료하기 위해 운영 체제에 가장 적합한 지시사항에 따라 수행하십시오.

- [Linux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

이러한 데이터 센터에서 프로비저닝된 모든 개선된 {{site.data.keyword.blockstorageshort}} 볼륨에는 암호화되지 않은 볼륨과는 다른 마운트 지점이 있습니다. 두 스토리지 볼륨에 올바른 마운트 지점을 사용 중임을 확인하기 위해 콘솔의 **볼륨 세부사항** 페이지에서 마운트 지점 정보를 볼 수 있습니다. 또한 API 호출(`SoftLayer_Network_Storage::getNetworkMountAddress()`)을 통해 올바른 마운트 지점에 액세스할 수도 있습니다.
{:tip}

## {{site.data.keyword.blockstorageshort}} 작성

API를 사용하여 주문하는 경우 새 스토리지로 업그레이드된 기능을 가져오는지 확인하기 위해 "서비스로서의 스토리지" 패키지를 지정하십시오.
{:important}

IBM Cloud 콘솔 및 {{site.data.keyword.slportal}}을 통해 향상된 LUN을 주문할 수 있습니다. 새 LUN은 마이그레이션을 수행하기 위해 원본 볼륨 크기 이상이어야 합니다.

- [사전 정의된 IOPS 티어(Endurance)가 있는 {{site.data.keyword.blockstorageshort}} 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [사용자 정의 IOPS(Performance)가 있는 {{site.data.keyword.blockstorageshort}} 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

몇 분 내에 새 스토리지가 마운트할 수 있게 제공됩니다. 리소스 목록과 {{site.data.keyword.blockstorageshort}} 목록에서 새 스토리지를 볼 수 있습니다.

## 호스트에 새 {{site.data.keyword.blockstorageshort}} 연결

"권한 부여된" 호스트는 볼륨에 대한 액세스 권한이 부여된 호스트입니다. 호스트 권한이 없는 경우, 시스템에서 스토리지에 액세스하거나 이를 사용할 수 없습니다. 볼륨에 액세스하도록 호스트에 권한을 부여하면 사용자 이름, 비밀번호, 다중 경로 I/O(MPIO) iSCSI 연결을 마운트하는 데 필요한 iSCSI 규정된 이름(IQN)이 생성됩니다.

1. **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하고 LUN 이름을 클릭하십시오.
2. **권한 부여된 호스트**로 스크롤하십시오.
3. 오른쪽에 있는 **호스트 권한 부여**를 클릭하십시오. 볼륨에 액세스할 수 있는 호스트를 선택하십시오.


## 스냅샷 및 복제

원본 LUN에 대해 스냅샷 및 복제가 설정되어 있습니까? 설정된 경우, 복제, 스냅샷 영역을 설정해야 하며 원본 볼륨과 동일한 설정으로 새 LUN에 대해 스냅샷 스케줄을 작성해야 합니다.

복제 대상 데이터 센터가 아직 업그레이드되지 않은 경우, 해당 데이터 센터를 업그레이드해야 새 볼륨에 대해 복제를 설정할 수 있습니다.


## 데이터 마이그레이션

1. 원본과 새 {{site.data.keyword.blockstorageshort}} LUN에 모두 연결하십시오.

   두 개의 LUN을 호스트에 연결하는 데 도움이 필요한 경우에는 지원 케이스를 여십시오.
   {:tip}

2. 원본 {{site.data.keyword.blockstorageshort}} LUN에 있는 데이터 유형 및 새 LUN에 복사하기 위한 최선의 방법을 고려하십시오.
  - 백업, 정적 컨텐츠, 기타 항목이 복사 중에 변경될 것으로 예상하지 않는 경우, 특별히 고려해야 할 사항은 없습니다.
  - {{site.data.keyword.blockstorageshort}}에서 데이터베이스 또는 가상 머신을 실행 중인 경우, 데이터 손상이 발생하지 않도록 데이터가 복사 중에 수정되지 않도록 하십시오.
  - 대역폭 관련 문제가 있는 경우, 최대 활동 시간이 아닌 시간에 마이그레이션을 수행하십시오.
  - 이러한 고려사항에 관련된 도움이 필요한 경우에는 지원 케이스를 여십시오.

3. 데이터를 복사하십시오.
   - **Microsoft Windows**의 경우 새 스토리지를 포맷하고 Windows Explorer를 사용하여 원본 {{site.data.keyword.blockstorageshort}} LUN에서 새 LUN으로 데이터를 복사하십시오.
   - **Linux**의 경우 `rsync`를 사용하여 데이터를 복사할 수 있습니다.
   ```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   경로가 제대로 설정되도록 `--dry-run` 플래그와 같이 이전 명령을 사용하는 것이 좋습니다. 이 프로세스가 인터럽트되는 경우 복사 중이던 마지막 대상 파일을 삭제하여 처음부터 새 위치에 복사되도록 할 수 있습니다.<br/>
      `--dry-run` 플래그를 사용하지 않고 이 명령을 완료하면 데이터가 암호화된 {{site.data.keyword.blockstorageshort}} LUN으로 복사되어야 합니다. 명령을 다시 실행하여 누락된 항목이 없게 하십시오. 두 위치를 수동으로 검토하여 누락된 항목이 없는지 살펴볼 수도 있습니다.<br/>
      마이그레이션이 완료되면 프로덕션을 새 LUN으로 이동할 수 있습니다. 그런 다음 원본 LUN을 구성에서 분리하고 삭제할 수 있습니다. 삭제하면 원본 LUN에 연관되어 있는 대상 사이트에서 모든 스냅샷과 복제본도 제거됩니다.
