---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-26"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} 주문

{{site.data.keyword.blockstorageshort}} 스토리지를 프로비저닝하고 용량과 IOPS 요구사항을 충족하도록 미세 조정할 수 있습니다. 성능을 지정하는 두 가지 옵션을 사용하여 스토리지를 최대한 활용하십시오.

- 성능 요구사항이 제대로 정의되지 않은 워크로드에 맞게 사전 정의된 성능 레벨을 제공하는 EnduranceIOP 티어를 선택할 수 있습니다. 
- 성능과 총 IOPS 수를 지정하여 매우 구체적인 성능 요구사항에 맞게 스토리지를 미세 조정할 수 있습니다.

## 사전 정의된 IOPS 티어(Endurance)가 있는 {{site.data.keyword.blockstorageshort}} 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지** > **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라 > 스토리지 > {{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 오른쪽 상단에서 **{{site.data.keyword.blockstorageshort}} 주문**을 클릭하십시오.
3. 배치 **위치**(데이터 센터)를 선택하십시오.
   - 사용자가 보유하고 있는 하나 이상의 컴퓨팅 호스트와 같은 위치에 새 스토리지를 추가하십시오.
4. 비용 청구. 향상된 기능(별표로 표시)이 포함된 데이터 센터를 선택하는 경우, 월별 또는 시간별 비용 청구 중에서 선택할 수 있습니다. 
     1. **시간별** 비용 청구를 사용하면 계정에서 블록 LUN이 있는 시간은 LUN이 삭제되는 시점 또는 비용 청구 주기 종료 시점에 계산합니다. 둘 중 빠른 시점을 이용합니다. 시간별 비용 청구는 며칠 정도 또는 한 달 미만으로 사용되는 스토리지에 적합합니다. 시간별 비용 청구는 해당 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝되는 스토리지에서만 사용할 수 있습니다. 
     2. **월별** 비용 청구를 사용하는 경우, 비용 계산은 작성 날짜부터 비용 청구 주기 종료 시점까지 비례 배분되며 즉시 청구됩니다. 비용 청구 주기 종료 전에 블록 LUN이 삭제되는 경우에는 환불되지 않습니다. 월별 비용 청구는 장시간(한달 이상) 저장 및 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 적합합니다.
        >**참고** - 월별 비용 청구 유형은 기본적으로 향상된 기능으로 업데이트되지 **않는** 데이터 센터에서 프로비저닝되는 스토리지에 대해 사용됩니다.
5. **새 스토리지 크기** 필드에 스토리지 크기를 입력하십시오.
6. **스토리지 IOPS 옵션** 섹션에서 **Enduance(티어로 구분된 IOPS)**를 선택하십시오.
7. 애플리케이션에 필요한 IOPS 티어를 선택하십시오.
    - **0.25 IOPS/GB**는 I/O 집약도가 낮은 워크로드를 대상으로 디자인되었습니다. 일반적으로 해당 워크로드는 일정 시점에 비활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어 메일함 저장 또는 부서 레벨 파일 공유가 이에 해당합니다.
    - **2 IOPS/GB**는 가장 일반적인 용도에 맞게 디자인되었습니다. 예를 들어 웹 애플리케이션 또는 하이퍼바이저를 위한 가상 머신 디스크 이미지를 지원하는 소규모 데이터베이스의 호스팅이 이에 해당합니다.
    - **4 IOPS/GB**는 집약도 높은 워크로드를 대상으로 디자인되었습니다. 일반적으로 해당 워크로드는 일정 시점에 활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어, 트랜잭션 데이터베이스 또는 그 외 성능에 민감한 데이터베이스가 이에 해당합니다.
    - **10 IOPS/GB**는 NoSQL 데이터베이스로 작성된 워크로드 및 분석을 위한 데이터 처리와 같이 가장 수요가 많은 워크로드를 대상으로 디자인되었습니다. 이 티어는 최대 4TB까지 프로비저닝되는 스토리지의 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서 사용 가능합니다.
8. **스냅샷 영역 크기 지정**을 클릭하고 목록에서 스냅샷 크기를 선택하십시오. 이 영역은 사용 가능한 영역 이외의 영역입니다. 스냅샷 영역 고려사항 및 권장사항에 대해서는 [스냅샷 주문](ordering-snapshots.html)을 참조하십시오.
9. 목록에서 **OS 유형**을 선택하십시오.
10. **이용 약관** 선택란을 선택하고 **주문하기**를 클릭하십시오.
11. 몇 분 내에 새 스토리지 할당이 사용 가능해야 합니다.

>**참고** - 기본적으로 결합된 총 250개의 {{site.data.keyword.blockstorageshort}} 볼륨을 프로비저닝할 수 있습니다. 볼륨 수를 늘리려면 영업 담당자에게 문의하십시오. 한계 증가에 대해서는 [여기](managing-storage-limits.html)를 참조하십시오.<br/><br/>동시 권한 부여 한계에 대해서는 [FAQs](BlockStorageFAQ.html)를 참조하십시오.
 
## 사전 정의 IOPS 티어(Performance)가 있는 {{site.data.keyword.blockstorageshort}} 주문

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 **스토리지**, **{{site.data.keyword.blockstorageshort}}**를 클릭하거나 {{site.data.keyword.BluSoftlayer_full}} 카탈로그에서 **인프라 > 스토리지 > {{site.data.keyword.blockstorageshort}}**를 클릭하십시오.
2. 오른쪽 상단에서 **{{site.data.keyword.blockstorageshort}} 주문**을 클릭하십시오.
3. **위치**를 클릭하고 데이터 센터를 선택하십시오.
   - 사용자가 보유하고 있는 하나 이상의 컴퓨팅 호스트와 같은 위치에 새 스토리지를 추가하십시오.
4. 비용 청구. 향상된 기능(별표로 표시)이 포함된 데이터 센터를 선택하는 경우, 월별 또는 시간별 비용 청구 중에서 선택할 수 있습니다.
     1. **시간별** 비용 청구를 사용하면 계정에서 블록 LUN이 있는 시간은 LUN이 삭제되는 시점 또는 비용 청구 주기 종료 시점에 계산합니다. 둘 중 빠른 시점을 이용합니다. 시간별 비용 청구는 며칠 정도 또는 한 달 미만으로 사용되는 스토리지에 적합합니다. 시간별 비용 청구는 해당 [데이터 센터 선택](new-ibm-block-and-file-storage-location-and-features.html)에서 프로비저닝되는 스토리지에서만 사용할 수 있습니다. 
     2. **월별** 비용 청구를 사용하는 경우, 비용 계산은 작성 날짜부터 비용 청구 주기 종료 시점까지 비례 배분되며 즉시 청구됩니다. 비용 청구 주기 종료 전에 블록 LUN이 삭제되는 경우에는 환불되지 않습니다. 월별 비용 청구는 장시간(한달 이상) 저장 및 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 적합합니다.
        >**참고** - 월별 비용 청구 유형은 기본적으로 향상된 기능으로 업데이트되지 **않는** 데이터 센터에서 프로비저닝되는 스토리지에 대해 사용됩니다.
5. **새 스토리지 크기** 필드에 스토리지 크기를 입력하십시오.
6. **스토리지 IOPS 옵션** 섹션에서 **Performance(할당된 IOPS)**를 선택하십시오.
7. **Performance(할당된 IOPS)** 필드에 IOPS를 입력하십시오.
8. **스냅샷 영역 크기 지정**을 클릭하고 목록에서 스냅샷 크기를 선택하십시오. 이 영역은 사용 가능한 영역 이외의 영역입니다. 스냅샷 영역 고려사항 및 권장사항에 대해서는 [스냅샷 주문](ordering-snapshots.html)을 참조하십시오.
9. 목록에서 **OS 유형**을 선택하십시오.
10. 몇 분 내에 새 스토리지 할당이 사용 가능해야 합니다.

>**참고** - 기본적으로 결합된 총 250개의 {{site.data.keyword.blockstorageshort}} 볼륨을 프로비저닝할 수 있습니다. 볼륨 수를 늘리려면 영업 담당자에게 문의하십시오. 한계 증가에 대해서는 [여기](managing-storage-limits.html)를 참조하십시오.<br/><br/>동시 권한 부여 한계에 대해서는 [FAQs](BlockStorageFAQ.html)를 참조하십시오.

## 새 스토리지 연결

프로비저닝 요청이 완료되면 새 스토리지에 액세스하고 연결을 구성하도록 호스트에 권한을 부여하십시오. 호스트의 운영 체제에 따라 해당 링크를 따르십시오.
- [Linux에서 MPIO iSCSI LUN에 연결](accessing_block_storage_linux.html)
- [CloudLinux에서 MPIO iSCSI LUN에 연결](configure-iscsi-cloudlinux.html)
- [Microsoft Windows에서 MPIO iSCSI LUNS 연결](accessing-block-storage-windows.html)
- [cPanel을 사용하여 Block Storage 구성](configure-backup-cpanel.html)
- [Plesk를 사용하여 Block Storage 구성](configure-backup-plesk.html)

## 송장에서 {{site.data.keyword.blockstorageshort}} 식별

모든 LUN은 송장에 개별 항목으로 표시됩니다. Endurance는 "Endurance 스토리지 서비스"로 표시되고 Performance는 "Performance 스토리지 서비스"로 표시됩니다. 비율은 스토리지 레벨에 따라 달라집니다. 그런 다음 Endurance또는 Performance를 펼쳐서 {{site.data.keyword.blockstorageshort}}인지 확인할 수 있습니다.
