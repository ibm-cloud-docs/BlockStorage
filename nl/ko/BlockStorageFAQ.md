---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-24"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} 자주 질문되는 내용(FAQ)

## 프로비저닝된 {{site.data.keyword.blockstorageshort}} 볼륨의 사용을 몇 개의 인스턴스에서 공유할 수 있습니까?
블록 볼륨별 권한 부여 수에 대한 기본 한계는 8입니다. 이 한계를 늘리려면 영업 담당자에게 문의하십시오.

## 성능(Performance) 또는 내구성(Endurance) {{site.data.keyword.blockstorageshort}} 프로비저닝 시 할당된 IOPS는 인스턴스로 적용됩니까, 아니면 볼륨으로 적용됩니까?
IOPS는 볼륨 레벨에서 적용됩니다. 다시 말해 6000 IOPS로 볼륨에 연결된 두 개의 호스트가 해당 6000 IOPS를 공유합니다.

## IOPS는 어떻게 측정됩니까?
IOPS는 랜덤 50% 읽기 및 50% 쓰기의 16KB 블록 로드 프로파일을 기반으로 합니다. 이 프로파일과 다른 워크로드에서는 성능이 저하됩니다.

## 성능 측정 시 더 작은 블록 크기를 사용하면 어떻게 됩니까?
더 작은 블록 크기를 사용해도 최대 IOPS를 얻을 수는 있지만 처리량이 떨어집니다. 예를 들어, 6000 IOPS의 볼륨의 경우 다양한 블록 크기에서 처리량은 다음과 같습니다.

- 16 KB * 6000 IOPS == ~93.75 MB/sec 
-  8 KB * 6000 IOPS == ~46.88 MB/sec
-  4 KB * 6000 IOPS == ~23.44 MB/sec

##  예상 처리량을 얻기 위해 볼륨을 예열해야 합니까?
예열할 필요는 없습니다. 볼륨을 프로비저닝하는 즉시 지정된 처리량을 관찰할 수 있습니다.

## 일부 데이터 센터에서는 내구성(Endurance) 10 IOPS 티어로 {{site.data.keyword.blockstorageshort}}를 프로비저닝할 수 있지만, 다른 데이터 센터에서는 프로비저닝할 수 없는 이유는 무엇입니까?
{{site.data.keyword.blockstorageshort}} 내구성(Endurance) 스토리지 유형 10 IOPS/GB 티어는 조만간 새 데이터 센터가 추가되는 데이터 센터 선택 시에만 사용할 수 있습니다.  여기에서 업그레이드된 전체 데이터 센터 및 사용 가능한 기능의 목록을 확인할 수 있습니다.

## 내 {{site.data.keyword.blockstorageshort}} LUN/볼륨이 암호화되었는지 어떻게 알 수 있습니까?
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에 {{site.data.keyword.blockstorageshort}} 목록을 표시하면 암호화된 LUN/볼륨 이름의 오른쪽에는 잠금 아이콘이 표시됩니다.

## 업그레이드된 데이터 센터에서 {{site.data.keyword.blockstorageshort}}의 프로비저닝 여부를 어떻게 확인합니까?
{{site.data.keyword.blockstorageshort}} 프로비저닝 시에 모든 업그레이드된 데이터 센터에는 암호화를 사용하여 스토리지가 프로비저닝되는 것을 나타내기 위해 별표(`*`)가 표기됩니다. 스토리지가 프로비저닝되면, 하나 이상의 볼륨이 암호화되었음을 보여주는 아이콘이 스토리지 목록에 표시됩니다. 암호화된 볼륨 및 볼륨이 모두 업그레이드된 데이터 센터에서만 프로비저닝됩니다. 여기에서 업그레이드된 전체 데이터 센터 및 사용 가능한 기능의 목록을 확인할 수 있습니다.

## 일부 데이터 센터에서는 내구성(Endurance) 10 IOPS 티어로 {{site.data.keyword.blockstorageshort}}를 프로비저닝할 수 있지만, 다른 데이터 센터에서는 프로비저닝할 수 없는 이유는 무엇입니까?
내구성(Endurance) 유형 10 IOPS/GB 티어는 조만간 새 데이터 센터가 추가되는 데이터 센터 선택 시에만 사용할 수 있습니다.  업그레이드된 전체 데이터 센터 및 사용 가능한 기능을 [여기](new-ibm-block-and-file-storage-location-and-features.html)에서 확인할 수 있습니다.

## 암호화를 위해 업그레이드되는 데이터 센터에서 암호화되지 않은 프로비저닝된 {{site.data.keyword.blockstorageshort}}가 있는 경우 내 {{site.data.keyword.blockstorageshort}}를 암호화할 수 있습니까?
데이터 센터 업그레이드 전에 프로비저닝된 {{site.data.keyword.blockstorageshort}}는 암호화할 수 없습니다. 
업그레이드된 데이터 센터에서 프로비저닝된 새 {{site.data.keyword.blockstorageshort}}는 자동으로 암호화됩니다. 그렇기 때문에 선택 가능한 암호화 설정이 없으며 이는 자동으로 수행됩니다. 
업그레이드된 데이터 센터에서 암호화되지 않은 스토리지의 데이터는 새 블록 LUN을 작성하고 호스트 기반 마이그레이션을 통해 데이터를 암호화된 새 LUN으로 복사하여 암호화할 수 있습니다. 마이그레이션 수행 방법에 대한 지시사항은 [문서](migrate-block-storage-encrypted-block-storage)를 참조하십시오.

## 프로비저닝할 수 있는 볼륨은 몇 개입니까?
기본적으로 총 250개의 결합된 {{site.data.keyword.blockstorageshort}} 볼륨을 프로비저닝할 수 있습니다.  볼륨을 늘리려면 영업 담당자에게 문의하십시오.

##  더 빠른 이더넷 연결을 사용하는 경우 더 높은 처리량을 얻을 수 있습니까?
처리량 한계는 볼륨/LUN 레벨별로 설정되기 때문에 더 빠른 이더넷 연결을 사용해도 설정 한계는 늘어나지 않습니다. 그렇지만 더 느린 이더넷 연결을 사용하는 경우 대역폭에서 잠재적으로 병목 현상이 발생할 수 있습니다.

##  방화벽/보안 그룹이 성능에 영향을 줍니까?
우수 사례로 방화벽을 우회하는 VLAN을 통해 스토리지 트래픽을 실행하도록 권장합니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 실행하면 대기 시간이 늘어나서 결국 스토리지 성능이 저하됩니다.

## 내 {{site.data.keyword.blockstorageshort}}에서 예상할 수 있는 성능 대기 시간은 어느 정도입니까?   

스토리지 내의 대상 대기 시간은 <1ms입니다. 자사의 스토리지는 공유 네트워크의 컴퓨팅 인스턴스에 연결되기 때문에 정확한 성능 대기 시간은 지정된 시간 범위 내의 네트워크 트래픽에 따라 달라집니다.

## Db2 pureScale에 대한 I/O 펜싱을 구현하기 위한 {{site.data.keyword.blockstorageshort}}의 SCSI-3 Persistent Reserve를 지원합니까?
예, {{site.data.keyword.blockstorageshort}}는  SCSI-2 및 SCSI-3 Persistent Reserve를 지원합니다.

## {{site.data.keyword.blockstorageshort}} 볼륨이 삭제되면 내 데이터는 어떻게 됩니까?

삭제되면, 해당 볼륨에서 데이터에 대한 모든 포인터가 제거되기 때문에 데이터에는 전혀 액세스할 수 없습니다. 물리적 스토리지가 다른 계정으로 다시 프로비저닝되면 새 포인터 세트가 지정됩니다. 새 계정이 물리적 스토리지에 있었을 수도 있는 임의 데이터에 액세스할 수 있는 방법은 없으며 새 포인터 세트는 모두 0을 표시합니다. 새 데이터가 볼륨/LUN에 작성되면 여전히 존재하던 모든 액세스 불가능하던 데이터는 겹쳐쓰기됩니다.

## 클라우드 데이터 센터에서 사용 중지된 드라이브는 어떻게 됩니까? 

드라이브가 사용 중지되는 경우, IBM은 더 이상 사용될 수 없도록 처리 전에 이를 파기합니다. 해당 드라이브에 쓰여진 모든 데이터는 더 이상 액세스가 불가능합니다. 
