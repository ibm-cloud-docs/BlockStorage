---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Block Storage 용량 확장
{: #expandingcapacity}

이 새 기능을 사용하면 현재 {{site.data.keyword.blockstoragefull}} 사용자는 기존 {{site.data.keyword.blockstorageshort}} 크기를 GB 단위로 최대 12TB까지 바로 확장 가능합니다. 복제본을 작성하거나 수동으로 데이터를 더 큰 볼륨으로 마이그레이션할 필요가 없습니다. 크기 조정 중에도 스토리지가 가동 중단되거나 액세스 불가능하지 않습니다.

볼륨에 대한 비용 청구는 현재 비용 청구 주기에 대해 새 가격의 비례 배분된 금액 차이가 추가되도록 자동으로 업데이트됩니다. 그런 다음 비용 청구 주기에는 신규 비용 전체가 청구됩니다.

이 기능은 [데이터 센터 선택](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)에 사용할 수 있습니다.

## 확장 가능 스토리지의 이점

- **비용 관리** – 데이터의 확장 가능성이 있지만, 시작할 때는 작은 크기의 스토리지여야 한다는 점을 알고 있습니다. 확장 기술을 사용하면 고객은 스토리지 비용을 절약하고 이후의 요구사항에 맞게 확장할 수 있습니다.  

- **스토리지 요구 확장** - 급속도의 데이터 확장을 경험 중인 고객에게는 이런 확장을 관리하기 위해 스토리지 크기를 쉽고 빠르게 늘리는 방법이 필요합니다.

## 복제 시 스토리지 용량 확장 영향

기본 스토리지에서 확장 조치로 인해 복제본의 크기는 자동으로 조정됩니다.

## 제한사항
{: #limitsofexpandingstorage}

이 기능은 [데이터 센터 선택](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)에서 프로비저닝된 스토리지에 사용할 수 있습니다.

이 기능이 릴리스되기 전에 **2017년 4월 - 2017년 12월 14일** 동안 이러한 데이터 센터에서 프로비저닝된 스토리지는 원래 크기의 10배로만 늘어날 수 있습니다. **2017년 12월 14일** 이후에 프로비저닝된 스토리지는 최대 12TB까지 확장이 가능합니다.

Endurance로 프로비저닝된 {{site.data.keyword.blockstorageshort}}에 대한 기존 크기 제한은 여전히 적용됩니다(10 IOPS 티어에 대해 최대 4TB, 다른 모든 티어에 대해 최대 12TB).

## 스토리지 크기 조정
{: #resizingsteps}

1. {{site.data.keyword.slportal}}에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 콘솔에서 **인프라** > **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 목록에서 LUN을 선택하고 **조치** > **LUN 수정**을 클릭하십시오.
3. GB 단위로 새 스토리지 크기를 입력하십시오.
4. 선택사항 및 새 가격을 검토하십시오.
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
6. 몇 분 내에 새 스토리지 할당이 사용 가능해야 합니다.

또는 SLCLI를 통해 볼륨의 크기를 조정할 수 있습니다.

```
# slcli block volume-modify --help
사용법: slcli block volume-modify [OPTIONS] VOLUME_ID

옵션:
  -c, --new-size INTEGER        블록 볼륨의 새 크기(GB). ***크기가 제공되지
                                않은 경우 볼륨의 원래 크기가 사용됩니다.***
                                가능한 크기: [20, 40, 80, 100, 250, 500,
                                1000, 2000, 4000, 8000, 12000]
                                최소: [볼륨의 원래 크기]
  -i, --new-iops INTEGER        Performance 스토리지 IOPS(100 - 6000 사이의
                                100의 배수) [Performance 볼륨에만 해당]
                                ***IOPS 값이 지정되지 않은 경우 볼륨의
                                원래 IOPS 값이 사용됩니다.***
                                요구사항: [볼륨의 원래 IOPS/GB가 0.3 미만인
                                경우 새 IOPS/GB도 0.3 미만이어야 합니다. 볼륨의 원래 IOPS/GB가 0.3 이상인 경우 볼륨의
                                새 IOPS/GB도 0.3 이상이어야 합니다.]
  -t, --new-tier [0.25|2|4|10]  Endurance 스토리지 티어(IOPS/GB) [Endurance
                                볼륨에만 해당] ***티어가 지정되지 않은 경우
                                볼륨의 원래 티어가 사용됩니다.***
                                요구사항: [볼륨의 원래 IOPS/GB가 0.25인 경우
                                볼륨의 새 IOPS/GB도 0.25여야 합니다. 볼륨의 원래 IOPS/GB가 0.25보다 큰 경우
                                볼륨의 새 IOPS/GB도 0.25보다 커야 합니다.]
  -h, --help                    이 메시지를 표시하고 종료합니다.
```
{:codeblock}

새 영역을 사용하기 위해 볼륨에 파일 시스템( 및 파티션이 있는 경우)을 확장하는 자세한 정보는 OS 문서를 참조하십시오.
{:tip}
