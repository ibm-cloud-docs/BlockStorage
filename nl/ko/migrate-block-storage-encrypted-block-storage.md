---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 기존 {{site.data.keyword.blockstorageshort}}을 개선된 {{site.data.keyword.blockstorageshort}}로 업그레이드

개선된 {{site.data.keyword.blockstoragefull}}는 이제 데이터 센터 선택에서 사용할 수 있습니다. 업그레이드된 데이터 센터 및 사용 가능한 기능(예: 조정 가능한 IOPS 속도 및 확장 가능한 볼륨) 목록을 보려면 [여기](new-ibm-block-and-file-storage-location-and-features.html)를 클릭하십시오. 제공자 관리 암호화된 스토리지에 대한 자세한 정보는 [{{site.data.keyword.blockstorageshort}} 저장 시 암호화](block-file-storage-encryption-rest.html)를 참조하십시오. 

선호하는 마이그레이션 경로는 두 LUN 모두에 동시 연결되고 임의의 LUN에서 다른 LUN으로 데이터를 직접 전송합니다. 스펙은 운영 체제 및 데이터가 복사 오퍼레이션 중에 변경되는지 여부에 따라 다릅니다.

이 경우, 이미 호스트에 암호화되지 않은 LUN을 접속했다고 가정합니다. 그렇지 않으면, 이 태스크를 완료하기 위해 운영 체제에 가장 적합한 지시사항에 따라 수행하십시오.

- [Linux에서 MPIO iSCSI LUN에 연결](accessing_block_storage_linux.html)
- [CloudLinux에서 MPIO iSCSI LUN에 연결](configure-iscsi-cloudlinux.html)
- [Microsoft Windows에서 MPIO iSCSI LUNS 연결](accessing-block-storage-windows.html)

## 새 {{site.data.keyword.blockstorageshort}} 작성

API를 사용하여 주문하는 경우 새 스토리지로 업그레이드된 기능을 가져오는지 확인하기 위해 "서비스로서의 스토리지" 패키지를 지정하십시오.
{:important}

다음 지시사항을 참조하여 {{site.data.keyword.slportal}}을 통해 개선된 LUN을 주문할 수 있습니다. 새 LUN은 마이그레이션을 수행하기 위해 원본 볼륨 크기 이상이어야 합니다.

### EnduranceLUN 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라 > 스토리지 > {{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 오른쪽 상단에서 **{{site.data.keyword.blockstorageshort}} 주문**을 클릭하십시오.
3. **스토리지 유형 선택** 목록에서 **Endurance**를 선택하십시오.
4. 배치 **위치**(데이터 센터)를 선택하십시오.
   - 새 스토리지가 이전 볼륨과 동일한 위치에 추가되는지 확인하십시오.
5. 비용 청구 옵션을 선택하십시오. 시간별 비용 청구와 월별 비용 청구 중에서 선택할 수 있습니다.
6. IOPS 티어를 선택하십시오.
7. **스토리지 크기 선택**을 클릭하고 목록에서 스토리지 크기를 선택하십시오.
8. **스냅샷 영역 크기 지정**을 클릭하고 목록에서 스냅샷 크기를 선택하십시오. 이 영역은 사용 가능한 영역 이외의 영역입니다.
   스냅샷 영역 고려사항과 권장사항에 대한 자세한 정보는 [스냅샷 주문](ordering-snapshots.html)을 참조하십시오.
   {:tip}
9. 목록에서 **OS 유형**을 선택하십시오.
10. **계속**을 클릭하십시오. 마지막으로 주문 세부사항을 검토할 수 있도록 월별 및 비례 배분된 비용이 표시됩니다.
11. **마스터 서비스 계약을 읽었습니다.** 선택란을 클릭하고 **주문하기**를 클릭하십시오.

### Performance LUN 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지**, **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라 > 스토리지 > {{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 오른쪽에서 **{{site.data.keyword.blockstorageshort}} 주문**을 클릭하십시오.
3. **스토리지 유형 선택** 목록에서 **Performance**를 선택하십시오.
4. **위치**를 클릭하고 데이터 센터를 선택하십시오.
   - 사용자가 이전에 주문한 호스트와 같은 위치에 새 스토리지를 추가하십시오.
5. 비용 청구 옵션을 선택하십시오. 시간별 비용 청구와 월별 비용 청구 중에서 선택할 수 있습니다.
6. 적절한 **스토리지 크기**를 선택하십시오.
7. **IOPS 지정** 필드에 IOPS를 입력하십시오.
8. **계속**을 클릭하십시오. 마지막으로 주문 세부사항을 검토할 수 있도록 월별 및 비례 배분된 비용이 표시됩니다. 주문을 변경하려면 **이전**을 클릭하십시오.
9. **마스터 서비스 계약을 읽었습니다.** 선택란을 클릭하고 **주문하기**를 클릭하십시오.

스토리지는 1분 내에 프로비저닝되며 {{site.data.keyword.slportal}}의 {{site.data.keyword.blockstorageshort}} 페이지에 표시됩니다.



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
  - 백업, 정적 컨텐츠, 복사 중에 변경되지 않는 것이 있는 경우, 특별히 고려해야 할 사항은 없습니다.
  - {{site.data.keyword.blockstorageshort}}에서 데이터베이스 또는 가상 머신을 실행 중인 경우, 데이터 손상이 발생하지 않도록 데이터가 복사 중에 수정되지 않도록 하십시오. 대역폭 관련 문제가 있는 경우, 최대 활동 시간이 아닌 시간에 마이그레이션을 수행하십시오. 이런 고려사항에 관련된 도움이 필요한 경우에는 지원 티켓을 여십시오.

3. 데이터를 복사하십시오.
   - **Microsoft Windows** - 원본 {{site.data.keyword.blockstorageshort}} LUN에서 새 LUN으로 데이터를 복사하려면 새 스토리지를 포맷하고 Windows Explorer를 사용하여 파일을 복사하십시오.
   - **Linux** - `rsync`를 사용하여 데이터를 복사할 수 있습니다. 예를 들면 다음과 같습니다.
   ```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   경로가 제대로 설정되도록 `--dry-run` 플래그와 같이 이전 명령을 사용하는 것이 좋습니다. 이 프로세스가 인터럽트되는 경우 최종 대상 파일을 삭제하여 처음부터 새 위치에 복사되도록 할 수 있습니다.<br/>
   `--dry-run` 플래그를 사용하지 않고 이 명령을 완료하면 데이터가 암호화된 {{site.data.keyword.blockstorageshort}} LUN으로 복사되어야 합니다. 명령을 다시 실행하여 누락된 항목이 없게 하십시오. 누락된 항목이 없는지 확인하기 위해 두 위치를 수동으로 검토할 수도 있습니다.<br/>
   마이그레이션이 완료되면 프로덕션을 새 LUN으로 이동할 수 있습니다. 그런 다음 원본 LUN을 구성에서 분리하고 삭제할 수 있습니다. 삭제하면 원본 LUN에 연관되어 있는 대상 사이트에서 모든 스냅샷과 복제본도 제거됩니다.
