---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:  Block Storage, block storage, snapshot, snapshot space, snapshot schedule, create snapshot schedule, manual snapshot, view snapshot space, modify snapshot space, SLCLI, API, restore from snapshot

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# 스냅샷 관리
{: #managingSnapshots}

## 스냅샷 스케줄 작성

스냅샷 스케줄을 사용하여 스토리지 볼륨에 대한 특정 시점의 참조를 작성하는 빈도 및 시기를 결정합니다. 스토리지 볼륨별로 최대 50개의 스냅샷이 가능합니다. 스케줄은 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}의 **스토리지** > **{{site.data.keyword.blockstorageshort}}** 탭을 통해 관리합니다.

스토리지 볼륨의 초기 프로비저닝 중에 스냅샷 영역을 구매하지 않은 경우에는 초기 스케줄을 설정하기 전에 우선 이를 구매해야 합니다. 자세한 정보는 [스냅샷 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)을 참조하십시오.
{:important}

### 스냅샷 스케줄 추가
{: #addingschedule}

스냅샷 스케줄은 시간별, 일별, 주별 간격으로 설정할 수 있으며 이들 각각에는 개별적인 보유 주기가 있습니다. 스토리지 볼륨당 스냅샷의 최대 한계는 50개이며, 이는 시간별, 일별 및 주별 스케줄을 혼합할 수 있고 수동 스냅샷입니다.

1. 스토리지 볼륨을 클릭하고 **조치**를 클릭한 후 **스케줄 스냅샷**을 클릭하십시오.
2. 새 스케줄 스냅샷 창에서 세 가지 서로 다른 스냅샷 빈도 중에 선택할 수 있습니다. 이 세 개의 조합 중 임의의 조합을 사용하여 포괄적인 스냅샷 스케줄을 작성하십시오.
   - 시간별
      - 스냅샷을 작성할 각 시간의 분을 지정하십시오. 기본값은 현재 분입니다.
      - 가장 오래된 스냅샷을 삭제하기 전에 유지해야 하는 시간별 스냅샷 수를 지정하십시오.
   - 일별
      - 스냅샷을 작성할 각 시간과 분을 지정하십시오. 기본값은 현재 시간과 분입니다.
      - 가장 오래된 스냅샷을 삭제하기 전에 유지해야 하는 시간별 스냅샷 수를 선택하십시오.
   - 주별
      - 스냅샷을 작성할 각 주의 요일, 시간 및 분을 지정하십시오. 기본값은 현재 요일, 시간 및 분입니다.
      - 가장 오래된 스냅샷을 삭제하기 전에 유지해야 하는 주별 스냅샷 수를 선택하십시오.
3. **저장**을 클릭하고 다른 빈도로 다른 스케줄을 작성하십시오. 스케줄된 스냅샷의 총 수가 50개를 넘으면 경고 메시지가 수신되며 저장할 수 없게 됩니다.

스냅샷이 작성되면 **세부사항** 페이지의 **스냅샷** 섹션에 스냅샷 목록이 표시됩니다.

또한 다음 명령을 사용하여 SLCLI를 통해 스냅샷 스케줄 목록을 볼 수 있습니다.
```
# slcli block snapshot-schedule-list --help
사용법: slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

옵션:
  -h, --help  이 메시지를 표시하고 종료합니다.
```
{:codeblock}

## 수동 스냅샷 작성

수동 스냅샷은 애플리케이션 업그레이드 또는 유지보수 중에 다양한 시점에서 작성할 수 있습니다. 또한 애플리케이션 레벨에서 임시로 비활성화된 여러 서버에서도 스냅샷을 작성할 수 있습니다.

스토리지 볼륨당 스냅샷의 최대 한계는 50입니다.

1. 스토리지 볼륨을 클릭하십시오.
2. **조치**를 클릭하십시오.
3. **수동 스냅샷 작성**을 클릭하십시오.
스냅샷이 작성되고 **세부사항** 페이지의 **스냅샷** 섹션에 표시됩니다. 해당 스케줄은 수동입니다.

또는 다음 명령을 사용하여 SLCLI를 통해 스냅샷을 작성할 수 있습니다.
```
# slcli block snapshot-create --help
사용법: slcli block snapshot-create [OPTIONS] VOLUME_ID

옵션:
  -n, --notes TEXT  새 스냅샷에 설정되는 참고사항
  -h, --help        이 메시지를 표시하고 종료합니다.
```
{:codeblock}

## 사용된 영역 정보 및 관리 기능이 포함된 모든 스냅샷 나열

유지된 스냅샷 및 사용된 영역의 목록은 **세부사항** 페이지에서 볼 수 있습니다.  관리 기능(스케줄 편집 및 추가 영역 추가)은 **조치** 메뉴 또는 페이지에 있는 다양한 섹션의 링크를 사용하여 세부사항 페이지에서 수행됩니다.

## 유지된 스냅샷 목록 보기

유지된 스냅샷은 스케줄 설정 시에 **마지막 보존** 필드에 입력한 수를 기반으로 합니다. **스냅샷** 섹션에서 작성된 스냅샷을 볼 수 있습니다. 스냅샷은 스케줄로 나열됩니다.

또는 SLCLI에서 다음 명령을 사용하여 사용 가능한 스냅샷을 표시할 수 있습니다.
```
# slcli block snapshot-list --help
사용법: slcli block snapshot-list [OPTIONS] VOLUME_ID

옵션:
  --sortby TEXT   열 정렬 기준
  --columns TEXT  열 표시. 옵션: id, name, created, size_bytes
  -h, --help      이 메시지를 표시하고 종료합니다.
```
{:codeblock}

## 사용된 스냅샷 영역의 크기 보기

**세부사항** 페이지의 원형 차트에서는 사용된 영역의 양과 남은 영역의 양을 표시합니다. 영역 임계값(75%, 90%, 95%)에 도달하면 알림이 수신됩니다.

## 볼륨의 스냅샷 영역 크기 변경

이전에 스냅샷 영역이 없었던 또는 추가 스냅샷 영역이 필요한 볼륨에 스냅샷 영역을 추가해야 할 수도 있습니다. 필요에 맞게 5 - 4,000GB에서 추가할 수 있습니다.

스냅샷 영역은 늘리기만 가능합니다. 줄일 수는 없습니다. 필요한 영역의 양이 판별될 때까지는 보다 작은 양의 영역을 선택할 수 있습니다. 자동 및 수동 스냅샷이 영역을 공유한다는 점을 기억하십시오.
{:note}

스냅샷 영역은 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 통해 변경됩니다.

1. 스토리지 볼륨을 클릭하고 **조치**를 클릭한 후 **추가 스냅샷 영역 추가**를 클릭하십시오.
2. 프롬프트에서 크기 범위를 선택하십시오. 일반적으로 크기 범위는 0에서 사용자 볼륨 크기까지 입니다.
3. **계속**을 클릭하십시오.
4. 사용자에게 있는 임의 프로모션 코드를 입력하고 **다시 계산**을 클릭하십시오. 기본적으로 이 주문에 대한 비용 및 주문 검토 필드가 완료됩니다.
5. **마스터 서비스 계약을 읽었습니다…** 선택란을 클릭하고 **주문하기**를 클릭하십시오. 추가 스냅샷 영역이 몇 분 내에 프로비저닝됩니다.

## 스냅샷 영역 한계에 도달하고 스냅샷을 삭제할 때 알림 받기

세 개의 다른 영역 임계값(75%, 90% 및 95%)에 도달할 때 지원 케이스를 통해 사용자 계정의 마스터 사용자에게 알림이 전송됩니다.

- **75퍼센트 용량**에서 스냅샷 영역 사용량이 75퍼센트를 초과했다는 경고가 전송됩니다. 경고가 표시될 때 수동으로 영역을 추가하거나 유지된 스냅샷 및 불필요한 스냅샷을 삭제하면 조치가 기록되고 케이스는 닫힙니다. 아무 조치도 수행하지 않는 경우, 수동으로 케이스를 수신확인해야 티켓이 닫힙니다.
- **90퍼센트 용량**에서 스냅샷 영역 사용량이 90퍼센트를 초과하면 두 번째 경고가 전송됩니다. 75% 용량에 도달했을 때와 마찬가지로 사용된 영역을 줄이는 필수 조치를 수행하면 조치가 기록되고 티켓은 닫힙니다. 아무 조치도 수행하지 않는 경우, 수동으로 케이스를 수신확인해야 티켓이 닫힙니다.
- **95퍼센트 용량**에서 최종 경고가 전송됩니다. 영역 사용량을 임계값 아래로 떨어뜨리기 위한 어떠한 조치도 취하지 않으면 알림이 생성되며 향후 스냅샷이 작성될 수 있도록 자동 삭제가 수행됩니다. 가장 이전 스냅샷부터 시작하여 사용량이 95퍼센트 아래로 떨어질 때까지 스케줄된 스냅샷이 삭제됩니다. 스냅샷은 사용량이 95퍼센트를 초과할 때마다 임계값 아래로 떨어질 때까지 계속 삭제됩니다. 영역을 수동으로 늘리거나 스냅샷이 삭제되는 경우, 경고는 재설정되고 임계값을 다시 초과하면 다시 발행됩니다. 조치를 수행하지 않는 경우 이 경고만 수신됩니다.

## 스냅샷 스케줄 삭제

스냅샷 스케줄은 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 통해 취소됩니다.

1. **세부사항** 페이지의 **스냅샷 스케줄** 프레임에서 삭제할 스케줄을 클릭하십시오.
2. 삭제되는 스케줄 옆에 있는 선택란을 클릭하고 **저장**을 클릭하십시오.<br />

복제 기능을 사용 중인 경우에는 삭제 중인 스케줄이 복제에서 사용하는 스케줄이 아닌지 확인하십시오. 복제 스케줄의 삭제에 대한 자세한 정보는 [데이터 복제](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication)를 참조하십시오.
{:important}

## 스냅샷 삭제

더 이상 필요하지 않은 스냅샷은 수동으로 제거하여 이후의 스냅샷에 사용할 공간을 늘릴 수 있습니다. 삭제는 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 통해 수행됩니다.

1. 스토리지 볼륨을 클릭하고 **스냅샷** 섹션으로 스크롤하여 기존 스냅샷의 목록을 보십시오.
2. 특정 스냅샷 옆에 있는 **조치**를 클릭하고 **삭제**를 클릭하여 스냅샷을 삭제하십시오. 스냅샷 간에는 종속성이 없으므로 이 삭제는 동일한 스케줄의 향후 또는 이전 스냅샷에 영향을 주지 않습니다.

포털에서 수동으로 삭제되지 않은 수동 스냅샷은 영역 한계에 도달하면 가장 오래된 스냅샷부터 자동으로 삭제됩니다.

다음 명령을 사용하여 SLCLI를 통해 볼륨을 삭제할 수 있습니다.
```
# slcli block snapshot-delete
사용법: slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

옵션:
  -h, --help  이 메시지를 표시하고 종료합니다.
```
{:codeblock}


## 스냅샷을 사용하여 특정 시점으로 내 스토리지 볼륨 복원

사용자 오류 또는 데이터 손상으로 인해 특정 시점으로 스토리지 볼륨을 다시 돌려야 할 수도 있습니다.

볼륨을 복원하면 복원에 사용된 스냅샷 이후 작성된 모든 스냅샷이 삭제됩니다.
{:important}

1. 호스트에서 스토리지 볼륨을 마운트 해제하고 분리하십시오.
   - [Linux에서 iSCSI LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#unmounting)
   - [Microsoft Windows에서 iSCSI LUNS 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows#unmounting)
2. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}의 **스토리지**, **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
3. 아래로 스크롤한 후에 복원되는 볼륨을 클릭하십시오. **세부사항** 페이지의 **스냅샷** 섹션에 해당 크기 및 작성 날짜와 같이 저장된 모든 스냅샷 목록이 표시됩니다.
4. 사용할 스냅샷 옆에 있는 **조치**를 클릭하고 **복원**을 클릭하십시오. <br/>

   복원을 완료하면 스냅샷을 작성한 이후 수정되거나 작성된 데이터가 유실됩니다. 이와 같은 데이터 유실은 스토리지 볼륨이 스냅샷 작성 시와 동일한 상태로 돌아가기 때문에 발생합니다.
   {:note}
5. **예**를 클릭하여 복원을 시작하십시오.

   선택된 스냅샷을 사용하여 볼륨이 복원되고 있음을 알리는 메시지가 페이지에 표시됩니다. 또한, 활성 트랜잭션이 진행 중임을 나타내는 아이콘이 {{site.data.keyword.blockstorageshort}}의 볼륨 옆에 표시됩니다. 아이콘 위에 마우스 커서를 두면 트랜잭션을 표시하는 창이 작성됩니다. 아이콘은 트랜잭션이 완료되면 없어집니다.
   {:note}
6. 스토리지 볼륨을 호스트에 마운트하고 다시 접속하십시오.
   - [Linux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
   - [CloudLinux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Microsoft Windows에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

또는 호스트에서 볼륨이 분리된 후에 SLCLI에서 다음 명령을 사용하여 복원을 시작할 수 있습니다.
```
# slcli block snapshot-restore --help
사용법: slcli block snapshot-restore [OPTIONS] VOLUME_ID

옵션:
  -s, --snapshot-id TEXT  블록 볼륨을 복원하는 데 사용될 스냅샷의 ID
  -h, --help              이 메시지를 표시하고 종료합니다.
```
{:codeblock}  

복원이 완료되고 나면 스토리지 볼륨을 호스트로 마운트하여 다시 연결하십시오.
