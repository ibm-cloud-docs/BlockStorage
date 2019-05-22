---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, block storage, snapshot, snapshot space, snapshot best practices, snapshot usage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 스냅샷
{: #snapshots}

스냅샷은 {{site.data.keyword.blockstoragefull}}의 기능입니다. 스냅샷은 특정 시점의 볼륨 컨텐츠를 나타냅니다. 스냅샷을 사용하면 성능에 영향을 미치지 않고 공간을 최소한으로 이용하면서 데이터를 보호할 수 있습니다. 스냅샷은 데이터 보호를 위한 1차 방어선으로 간주됩니다. 사용자가 볼륨에서 중요한 데이터를 실수로 수정하거나 삭제한 경우, 데이터는 스냅샷 사본에서 쉽고 빠르게 복원할 수 있습니다.

{{site.data.keyword.blockstorageshort}}에서는 다음과 같은 두 가지 방법으로 스냅샷을 작성할 수 있습니다.

* 첫 번째로, 각 스토리지 볼륨에 대해 자동으로 스냅샷 사본을 작성하고 삭제하는 구성 가능한 스냅샷 스케줄을 통해 작성합니다. 사용자의 요구사항에 따라 추가로 스냅샷 스케줄을 작성하거나 수동으로 사본을 삭제 및 스케줄을 관리할 수도 있습니다.
* 두 번째 방법은 수동 스냅샷을 작성하는 방법입니다.

스냅샷 사본은 특정 시점의 볼륨 상태를 캡처하는 {{site.data.keyword.blockstorageshort}} LUN의 읽기 전용 이미지입니다. 스냅샷 사본은 작성 시간 및 스토리지 영역 모두에서 효율적입니다. {{site.data.keyword.blockstorageshort}} 스냅샷 사본 작성은 몇 초밖에 걸리지 않습니다. 볼륨 크기 또는 스토리지에서 활동 레벨에 상관없이 일반적으로 1초 미만입니다. 스냅샷 사본이 작성되면 데이터 오브젝트에 대한 변경사항은 스냅샷 사본이 존재하지 않는 것처럼 오브젝트의 현재 버전에 대한 업데이트로 반영됩니다. 그러면서도 데이터의 사본은 안전하게 유지됩니다.

스냅샷 사본은 성능 저하를 발생시키지 않습니다. 사용자는 {{site.data.keyword.blockstorageshort}} 볼륨별로 최대 50개의 스케줄된 스냅샷 및 50개의 수동 스냅샷을 쉽게 저장할 수 있으며 이들 모두 데이터의 읽기 전용 및 온라인 버전으로 액세스할 수 있습니다.

스냅샷으로 다음을 수행할 수 있습니다.

- 시스템 중단 없이도 특정 시점의 복구 지점 작성
- 볼륨을 이전의 특정 시점으로 되돌리기

스냅샷을 작성할 수 있도록 먼저 볼륨의 일부 스냅샷 영역을 구매해야 합니다. 스냅샷 영역은 초기 주문 중 또는 이후에 **볼륨 세부사항** 페이지를 통해 추가할 수 있습니다. 스케줄된 스냅샷과 수동 스냅샷은 스냅샷 영역을 공유하므로 충분한 스냅샷 영역을 주문하십시오. 자세한 정보는 [스냅샷 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)을 참조하십시오.

## 스냅샷 우수 사례

스냅샷 디자인은 고객 환경에 따라 달라집니다. 다음 디자인 고려사항을 사용하여 스냅샷 사본을 계획하고 구현할 수 있습니다.
- 각 볼륨 또는 LUN에서 스케줄을 통해 최대 50개의 스냅샷을 작성하고, 수동으로 최대 50개를 작성할 수 있습니다.
- 너무 많은 스냅샷을 작성하지는 마십시오. 시간별, 일별 또는 주별 스냅샷을 스케줄하여 스케줄된 스냅샷 빈도가 RTP와 RPO의 필요에 적합하고 애플리케이션 비즈니스 요구사항을 만족시키도록 하십시오.
- 스냅샷 AutoDelete는 스토리지 이용 확장 제어에 사용되어야 합니다. <br/>

  AutoDelete 임계값은 95퍼센트로 고정되어 있습니다.
  {:note}

스냅샷은 실제 오프사이트 재해 복구 복제 또는 장기 보유 백업을 위한 대체가 아닙니다.
{:important}

## 보안

암호화된 {{site.data.keyword.blockstorageshort}}의 모든 스냅샷과 복제본은 기본적으로 암호화됩니다. 볼륨별로 이 기능을 끌 수는 없습니다. 제공자 관리 저장 시 암호화에 대한 자세한 정보는 [데이터 보안](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)을 참조하십시오.

## 스냅샷이 디스크 영역에 미치는 영향

스냅샷 사본은 전체 파일이 아니라 개별 블록을 유지하여 디스크 이용을 최소화합니다. 스냅샷 사본은 활성 파일 시스템의 파일이 변경 또는 삭제되는 경우에만 추가 영역을 사용합니다.

활성 파일 시스템에서, 변경된 블록은 디스크의 다른 위치에 다시 기록되거나 활성 파일 블록으로 완전히 제거됩니다. 파일이 변경되거나 삭제되면 원래 파일 블록이 하나 이상의 스냅샷 사본의 일부로서 보관됩니다. 결과적으로, 원본 블록에서 사용되는 디스크 영역도 여전히 변경 전의 활성 파일 시스템 상태를 반영하도록 예약됩니다. 이 영역은 수정된 활성 파일 시스템의 블록에서 사용되는 디스크 영역에 추가로 예약됩니다.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">스냅샷 복사 전후 디스크 영역 사용량</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="스냅샷 복사 전"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="스냅샷 복사 후"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="스냅샷 복사 후 변경"></td>
     </tr><tr>
        <td style="border: 0.0px;">임의 스냅샷 사본이 작성되기 전에는 활성 파일 시스템만 디스크 영역을 사용합니다.</td>
        <td style="border: 0.0px;">스냅샷 사본이 작성되면 활성 파일 시스템 및 스냅샷 사본이 동일한 디스크 블록을 지시합니다. 스냅샷 사본은 추가 디스크 영역을 사용하지 않습니다.</td>
        <td style="border: 0.0px;">활성 파일 시스템에서 <i>myfile.txt</i>가 삭제된 후에도 스냅샷 사본은 여전히 파일을 포함하고 해당 디스크 블록을 참조합니다. 그렇기 때문에 활성 파일 시스템 데이터를 삭제해도 디스크 영역이 항상 다시 사용 가능하지는 않습니다.</td>
      </tr>
</table>

스냅샷 영역 사용량에 대한 자세한 정보는 [스냅샷 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)를 참조하십시오.
