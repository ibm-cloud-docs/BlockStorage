---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IOPS 조정
{: #adjustingIOPS}

이 새 기능을 사용하면 {{site.data.keyword.blockstoragefull}} 스토리지 사용자가 기존 {{site.data.keyword.blockstorageshort}}의 IOPS를 즉시 조정할 수 있습니다. 복제본을 작성하거나 수동으로 데이터를 새 스토리지에 복사할 필요가 없습니다. 조정 중에도 스토리지가 가동 중단되거나 액세스 불가능하게 되지 않습니다.

스토리지에 대한 비용 청구는 현재 비용 청구 주기에 대해 새 가격의 비례 배분된 금액 차이가 추가되도록 업데이트됩니다. 다음 비용 청구 주기에는 신규 비용 전체가 청구됩니다.


## 조정 가능 IOPS의 이점

- 비용 관리 - 일부 클라이언트에는 최대 사용 시간 중에 높은 IOPS만 필요할 수 있습니다. 예를 들어, 대형 마트의 경우 휴일에 최대 사용량이 발생하고 스토리지에서 더 큰 IOPS 속도가 필요할 수도 있습니다. 그러나 한여름에는 더 큰 iOPS가 필요하지 않습니다. 이 기능을 사용하여 비용을 관리하고 필요할 때 더 높은 IOPS 비용을 지불할 수 있습니다.

## 제한사항

이 기능은 [데이터 센터 선택](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)에서만 사용할 수 있습니다.

클라이언트는 IOPS 조정 시에 Endurance/Performance 사이에서 전환할 수 없습니다. 그러나 다음과 같은 기준 및 제한사항을 바탕으로 스토리지에 대해 새 IOPS 티어 또는 IOPS 레벨을 지정할 수 있습니다.

- 원본 볼륨이 Endurance0.25 티어인 경우, IOPS 티어는 업데이트할 수 없습니다.
- 원래 볼륨이 0.30IOPS/GB 이하의 Performance인 경우, 사용 가능한 옵션에는 결과가 0.30IOPS/GB 이하인 크기 및 IOPS 조합만 포함됩니다.
- 원래 볼륨이 0.30IOPS/GB를 초과하는 Performance인 경우, 사용 가능한 옵션에는 결과가 0.30IOPS/GB를 초과하는 크기 및 IOPS 조합만 포함됩니다.

## 복제 시 IOPS 조정 효과

볼륨에 복제본이 있는 경우, 복제본은 기본의 IOPS 선택에 일치하도록 자동 업데이트됩니다.

## 스토리지에서 IOPS 조정
{: #steps}

1. {{site.data.keyword.blockstorageshort}} 목록으로 이동하십시오.
   - {{site.data.keyword.slportal}}에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
   - {{site.data.keyword.BluSoftlayer_full}} 콘솔에서 **인프라** > **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 목록에서 LUN을 선택하고 **조치** > **LUN 수정**을 클릭하십시오.
3. **스토리지 IOPS 옵션**에서 새로 선택하십시오.
    - Endurance(계층 IOPS)의 경우 스토리지의 0.25IOPS/GB보다 큰 IOPS 계층을 선택하십시오. IOPS 티어는 언제든지 늘릴 수 있습니다. 그렇지만 줄이는 것은 한 달에 한 번만 가능합니다.
    - Performance(할당된 IOPS)의 경우 100 - 48,000 IOPS 범위의 값을 입력하여 스토리지에 대해 새 IOPS 옵션을 지정하십시오.

주문 양식에서 크기별로 필요한 특정 경계를 확인하십시오.
    {:tip}
4. 선택사항 및 새 가격을 검토하십시오.
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
6. 몇 분 내에 새 스토리지 할당이 사용 가능해야 합니다.


또는 SLCLI를 통해 IOPS를 조정할 수 있습니다.
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
                                경우 새 IOPS/GB도 0.3 미만이어야 합니다.
                                볼륨의 원래 IOPS/GB가 0.3 이상인 경우 볼륨의
                                새 IOPS/GB도 0.3 이상이어야 합니다.]
  -t, --new-tier [0.25|2|4|10]  Endurance 스토리지 티어(IOPS/GB) [Endurance
                                볼륨에만 해당] ***티어가 지정되지 않은 경우
                                볼륨의 원래 티어가 사용됩니다.***
                                요구사항: [볼륨의 원래 IOPS/GB가 0.25인 경우
                                볼륨의 새 IOPS/GB도 0.25여야 합니다.
                                볼륨의 원래 IOPS/GB가 0.25보다 큰 경우
                                볼륨의 새 IOPS/GB도 0.25보다 커야 합니다.]
  -h, --help                    이 메시지를 표시하고 종료합니다.
```
{:codeblock}
