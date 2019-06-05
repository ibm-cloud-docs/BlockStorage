---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-22"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# {{site.data.keyword.blockstorageshort}} 정보
{: #About}

{{site.data.keyword.cloud}} {{site.data.keyword.blockstorageshort}}는 컴퓨팅 인스턴스와 상관없이 프로비저닝 및 관리되는 지속적인 고성능 iSCSI 스토리지입니다. iSCSI 기반의 {{site.data.keyword.blockstorageshort}} LUN은 중복 다중 경로 I/O(MPIO) 연결을 통해 권한 부여된 디바이스에 연결됩니다.

{{site.data.keyword.blockstorageshort}}는 일치하지 않는 기능 세트를 사용하여 최고의 내구성 및 가용성을 제공합니다. 업계 표준 및 우수 사례를 사용하여 빌드되었습니다. {{site.data.keyword.blockstorageshort}}는 일관성 있는 성능 기준을 제공하여 데이터 무결성을 보호하고 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성은 유지하도록 디자인되었습니다.

## 핵심 기능
{: #corefeatures}

{{site.data.keyword.blockstorageshort}}의 다음 기능을 사용해 보십시오.

- **일관된 성능 기준선**
   - 개별 볼륨에 대해 프로토콜 레벨의 IOPS 할당을 통해 제공됩니다.
- **뛰어난 내구성 및 복원성**
   - 데이터 무결성을 보호하고 개별 디스크(RAID) 배열에 대한 운영 체제 레벨의 중복 배열을 작성 및 관리할 필요없이 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성을 유지보수합니다.
- **비활성 데이터(Data-At-Rest) 암호화**([데이터 센터 선택 시 사용 가능](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - 비활성 데이터(Data-At-Rest)에 대한 제공자 관리 암호화
- **모든 플래시 지원 스토리지**([데이터 센터 선택 시 사용 가능](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - 2 IOPS/GB 이상 레벨로 Endurance및 Performance를 사용하여 프로비저닝된 볼륨에 대한 모든 플래시 스토리지
- **스냅샷**([데이터 센터 선택 시 사용 가능](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - 중단 없이 특정 시점의 데이터 스냅샷 캡처
- **복제**([데이터 센터 선택 시 사용 가능](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - 파트너 {{site.data.keyword.cloud}} 데이터 센터에 스냅샷 자동 복사
- **고가용성의 연결**
   - 가용성을 극대화하기 위해 중복 네트워킹 연결 사용
   - iSCSI 기반 {{site.data.keyword.blockstorageshort}}에서는 다중 경로 I/O(MPIO)를 사용합니다.
- **동시 액세스**
   - 클러스터된 구성을 위해 여러 호스트가 동시에 블록 볼륨(최대 8)에 액세스하는 것을 허용합니다.
- **클러스터된 데이터베이스**
   - 클러스터된 데이터베이스와 같은 고급 유스 케이스를 지원합니다.


## 프로비저닝
{: #provisioning}

{{site.data.keyword.blockstorageshort}} LUN은 20GB부터 12TB까지 프로비저닝 가능하며 다음과 같은 두 개의 옵션이 있습니다. <br/>
- 사전 정의된 성능 레벨과 스냅샷 및 복제와 같은 기타 기능을 제공하는 **Endurance** 티어를 프로비저닝합니다.
- 초당 할당된 입출력(I/O) 오퍼레이션(IOPS)이 있는 강력한 **Performance** 환경을 빌드합니다.


### Endurance 티어를 사용하여 프로비저닝
{: #provendurance}

Endurance{{site.data.keyword.blockstorageshort}}는 다양한 애플리케이션의 요구를 지원하기 위해 4개의 IOPS 성능 티어에서 사용할 수 있습니다. <br />

- **0.25 IOPS/GB**는 I/O 집약도가 낮은 워크로드를 대상으로 디자인되었습니다. 일반적으로 해당 워크로드는 임의 시점에 비활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어 메일함 저장 또는 부서 레벨 파일 공유가 이에 해당합니다.

- **2 IOPS/GB**는 가장 일반적인 용도에 맞게 디자인되었습니다. 예를 들어 웹 애플리케이션 또는 하이퍼바이저를 위한 VM 디스크 이미지를 지원하는 소규모 데이터베이스의 호스팅이 이에 해당합니다.

- **4 IOPS/GB**는 집약도 높은 워크로드를 대상으로 디자인되었습니다. 일반적으로 해당 워크로드는 임의 시점에 활성 데이터의 비율이 높은 특성이 있습니다. 예를 들어, 트랜잭션 데이터베이스 또는 그 외 성능에 민감한 데이터베이스가 이에 해당합니다.

- **10 IOPS/GB**는 NoSQL 데이터베이스로 작성된 워크로드 및 분석을 위한 데이터 처리와 같이 가장 수요가 많은 워크로드를 대상으로 디자인되었습니다. 이 티어는 [데이터 센터 선택](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations) 시에만 최대 4TB의 스토리지 프로비저닝에 사용할 수 있습니다.

12TB Endurance 볼륨으로 최대 48,000IOPS까지 사용 가능합니다.

워크로드에 맞는 Endurance 티어를 선택하는 것이 중요합니다. 최대 성능 달성에 필요한 올바른 블록 크기, 이더넷 연결 속도 및 호스트 수를 사용하는 것도 매우 중요합니다. 이들 중 하나라도 제대로 맞지 않는 경우, 처리 결과에 심각한 영향을 줄 수 있습니다.


### Performance를 사용하여 프로비저닝
{: #provperformance}

Performance는 Endurance티어 내에는 잘 맞지 않는 성능 요구사항이 있는 높은 I/O의 애플리케이션을 지원하도록 설계된 {{site.data.keyword.blockstorageshort}}의 클래스입니다. 예측 가능한 성능은 프로토콜 레벨의 IOPS를 개별 볼륨에 할당하여 얻을 수 있습니다. 100 - 48,000의 다양한 IOPS 속도는 20GB - 12TB 범위의 스토리지 크기로 프로비저닝할 수 있습니다.

{{site.data.keyword.blockstorageshort}}용 Performance는 다중 경로 I/O(MPIO) iSCSI(internet Small Computer System Interface) 연결을 통해 액세스하고 마운트합니다. 일반적으로 {{site.data.keyword.blockstorageshort}}는 단일 서버에서 볼륨에 액세스하는 경우에 사용됩니다. 다중 볼륨은 더 큰 볼륨 및 더 높은 IOPS 수를 얻기 위해 호스트에 마운트되어 같이 스트라이프 가능합니다. Performance 볼륨은 표 3의 크기 및 IOPS 속도에 따라 주문 가능합니다(Linux, XEN 및 Windows 운영 체제).

|크기(GB) |최소 IOPS |최대 IOPS
|-----|-----|-----|
|20 |100 |1,000 |
|40 |100 |2,000  |
|80 |100 |4,000 |
|100 |100 |6,000 |
|250 |100 |6,000 |
|500 |100  |6,000 또는 10,000 |
|1,000 |100 |6,000 또는 20,000 ![각주](/images/numberone.png) |
|2,000 |200 |6,000 또는 40,000 ![각주](/images/numberone.png) |
|3,000 - 7,000 |300 |6,000 또는 48,000 ![각주](/images/numberone.png) |
|8,000-9,000 |500 |6,000 또는 48,000 ![각주](/images/numberone.png) |
|10,000-12,000 |1,000 |6,000 또는 48,000 ![각주](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="표 비교" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![각주](/images/numberone.png) *6,000보다 큰 IOPS 한계는 특정 데이터 센터에서 사용 가능합니다.*

Performance 볼륨은 프로비저닝된 IOPS 레벨과 계속 근접하게 작동하도록 디자인되었습니다. 일관성이 있으며 특정 성능 레벨로 애플리케이션 환경을 크기 조정하고 스케일링하기 쉬워집니다. 또한 이상적인 가격 대 성능 비율의 볼륨을 빌드하여 환경을 최적화할 수 있습니다.

## 비용 청구
{: #billing}

블록 LUN에 대해 시간별 또는 월별 비용 청구를 선택할 수 있습니다. LUN에 대해 선택한 비용 청구 유형은 해당 스냅샷 영역 및 복제본에 적용됩니다. 예를 들어, LUN을 시간별 비용 청구로 프로비저닝하는 경우, 모든 스냅샷 및 복제본 비용은 시간별로 청구됩니다. LUN을 월별 비용 청구로 프로비저닝하는 경우, 모든 스냅샷 및 복제본 비용은 월별로 청구됩니다.

 * **시간별 비용 청구**를 사용하면 계정에서 블록 LUN이 존재하는 시간은 LUN이 삭제되는 시점 또는 비용 청구 주기 종료 시점(둘 중 빠른 경우)에 계산합니다. 시간별 비용 청구는 며칠 정도 또는 한 달 미만으로 사용되는 스토리지에 적합합니다. 시간별 비용 청구는 [데이터 센터 선택](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations)에서 프로비저닝된 스토리지에만 사용할 수 있습니다.

 * **월별 비용 청구**를 사용하는 경우, 비용 계산은 작성 날짜부터 비용 청구 주기 종료 시점까지 비례 배분되며 즉시 청구됩니다. 비용 청구 주기 종료 전에 LUN이 삭제되는 경우에는 환불되지 않습니다. 월별 비용 청구는 장시간(한달 이상) 저장 및 액세스해야 하는 데이터를 사용하는 프로덕션 워크로드에서 사용되는 스토리지에 적합합니다.

### Endurance
{: #pricing-comparison-endurance}

| 사전 정의된 IOPS 티어의 가격 옵션 |0.25IOPS |2 IOPS/GB |4 IOPS/GB |10 IOPS/GB |
|-----|-----|-----|-----|-----|
|월별 가격 |$0.06/GB |$0.15/GB |$0.20/GB |$0.58/GB |
|시간별 가격 |$0.0001/GB |$0.0002/GB |$0.0003/GB |$0.0009/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="표 비교" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Performance
{: #pricing-comparison-performance}

| 사용자 정의 IOPS의 가격 옵션 | 가격 계산 |
|-----|-----|
|월별 가격 |$0.10/GB + $0.07/IOP |
|시간별 가격 |$0.0001/GB + $0.0002/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="표 비교" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
