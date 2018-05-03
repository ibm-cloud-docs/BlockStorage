---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# IOPS 조정

이 새 기능을 사용하여 {{site.data.keyword.blockstoragefull}} 스토리지 사용자는 복제를 작성하거나 데이터를 새 스토리지로 마이그레이션하지 않고도 기존 {{site.data.keyword.blockstorageshort}}의 IOPS를 바로 조정할 수 있습니다. 조정 중에도 스토리지가 가동 중단되거나 액세스 불가능하게 되는 경우는 없습니다. 

스토리지에 대한 비용 청구는 현재 비용 청구 주기에 대해 새 가격의 비례 배분된 금액 차이가 추가되도록 업데이트되고 다음 비용 청구 주기에는 신규 비용 전체가 청구됩니다.

이 기능은 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서만 사용할 수 있습니다. 

##  조정 가능한 IOPS의 이점이 생기는 이유는 무엇입니까?

- 비용 관리 - 일부 고객은 최대 사용 시간 중에 높은 IOPS만 필요할 수가 있습니다. 예를 들어, 대형 마트의 경우 휴일에 최대 사용량이 발생하고 한여름에 비해 스토리지에서 더 큰 IOPS가 필요할 수도 있습니다. 이 기능을 사용하여 비용을 관리하고 실제로 필요할 때만 더 높은 IOPs 비용을 지불할 수 있습니다.

## 제한사항이 있습니까?

이 기능은 개선된 기능을 사용하여 [데이터 센터](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝된 스토리지에만 사용할 수 있습니다. 

클라이언트는 IOPS 조정 시에 내구성(Endurance)과 성능(Performance) 사이에서 전환할 수 없습니다. 사용자는 다음과 같은 기준/제한사항을 바탕으로 스토리지에 대해 새 IOPS 티어 또는 IOPS 레벨을 지정할 수 있습니다. 

- 원본 볼륨이 내구성(Endurance) 0.25 티어인 경우, IOPS 티어는 업데이트할 수 없습니다.
- 원본 볼륨이 < 0.30 IOPS/GB의 성능(Performance)인 경우, 사용 가능한 옵션은 < 0.30 IOPS/GB가 되는 크기 및 IOPS 조합만 포함해야 합니다. 
- 원본 볼륨이 >= 0.30 IOPS/GB인 성능(Performance)인 경우, 사용 가능한 옵션은 >= 0.30 IOPS/GB가 되는 크기 및 IOPS 조합만 포함해야 합니다. 크기(원본 볼륨 이상)



##  IOPS 조정 시 복제에 어떤 영향을 줍니까?

볼륨에 복제본이 있는 경우, 복제본은 기본의 IOPS 선택에 일치하도록 자동 업데이트됩니다. 

## 내 스토리지에서 IOPS는 어떻게 조정할 수 있습니까?

1. {{site.data.keyword.slportal}}에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라** > **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 목록에서 LUN을 선택하고 **조치** > **LUN 수정**을 클릭하십시오.
3. **스토리지 IOPS 옵션**에서 새로 선택하십시오.
    - 내구성(Endurance)(티어가 있는 IOPS): 스토리지가 0.25 IOPS/GB보다 큰 IOPS 티어를 선택하십시오. IOPS 티어는 언제든지 늘릴 수 있습니다. 그렇지만 줄이는 것은 한 달에 한 번만 가능합니다.
    - 성능(Performance)(할당된 IOPS): 100 - 48,000 IOPS 사이의 값을 입력하여 스토리지에 대해 새 IOPS 옵션을 지정하십시오. (주문 양식에서 크기별로 필요한 특정 경계를 확인하십시오.)
4. 선택사항 및 새 가격 책정을 검토하십시오.
5. **마스터 서비스 계약을 읽었습니다...** 선택란을 클릭하고 **주문하기**를 클릭하십시오.
6. 몇 분 내에 새 스토리지 할당이 사용 가능해야 합니다.
