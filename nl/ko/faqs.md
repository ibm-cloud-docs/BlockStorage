---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-05"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# FAQ

## {{site.data.keyword.blockstorageshort}} 볼륨의 사용을 몇 개의 인스턴스에서 공유할 수 있습니까?
{: faq}

블록 볼륨당 권한 부여 수에 대한 기본 한계는 8개입니다. 이는 블록 스토리지 LUN에 액세스하도록 최대 8개의 호스트에 권한을 부여할 수 있음을 의미합니다. 한계를 늘리도록 요청하려면 영업 담당자에게 문의하십시오.

## 주문할 수 있는 볼륨은 얼마나 됩니까?
{: faq}

기본적으로 총 250개의 결합된 {{site.data.keyword.blockstorageshort}} 볼륨을 프로비저닝할 수 있습니다. 볼륨 한계를 늘리려면 영업 담당자에게 문의하십시오. 자세한 정보는 [스토리지 한계 관리](managing-storage-limits.html)를 참조하십시오.

## 호스트에 마운트할 수 있는 {{site.data.keyword.blockstorageshort}} 볼륨은 몇 개입니까?
{: faq}

호스트 운영 체제에서 처리할 수 있는 항목에 따라 달라지며 {{site.data.keyword.BluSoftlayer_full}}에서 제한하는 것은 아닙니다. 마운트할 수 있는 볼륨 수에 대한 한계는 OS 문서를 참조하십시오.

## 내 블록 스토리지 LUN에 대해 어떤 Windows 버전을 선택해야 합니까? 
{: faq}

LUN을 작성할 때 OS 유형을 지정해야 합니다. OS 유형은 LUN에 액세스하는 호스트가 사용하는 운영 체제를 기반으로 해야 합니다. OS 유형은 LUN 작성 이후 수정될 수 없습니다. LUN의 실제 크기는 LUN의 OS 유형에 따라 약간 다를 수 있습니다. 

**Windows 2008+**
- LUN에서 Windows 2008 이상 버전의 Windows 데이터를 저장합니다. 호스트 운영 체제가 Windows Server 2008, Windows Server 2012, Windows Server 2016인 경우에는 이 OS 옵션을 사용하십시오. MBR 및 GPT 파티셔닝 방법이 둘 다 지원됩니다. 
 
**Windows 2003**
- LUN에서 MBR(Master Boot Record) 파티셔닝 스타일을 사용하여 단일 파티션 Windows 디스크에 원시 디스크 유형을 저장합니다. 호스트 운영 체제가 MBR 파티셔닝 방법을 사용하는 Windows 2000 Server, Windows XP 또는 Windows Server 2003인 경우에만 이 옵션을 사용하십시오. 

**Windows GPT**
-  LUN에서 GPT(GUID Partition Type) 파티셔닝 스타일을 사용하여 Windows 데이터를 저장합니다. GPT 파티셔닝 방법을 사용하고자 하며 호스트가 이를 사용할 수 있는 경우에는 이 옵션을 사용하십시오. Windows Server 2003, 서비스팩 1 이상은 GPT 파티셔닝 방법을 사용할 수 있으며, 모든 64비트 Windows 버전은 이를 지원합니다. 

## 할당된 IOPS 한계는 인스턴스로 적용됩니까, 아니면 볼륨으로 적용됩니까?
{: faq}

IOPS는 볼륨 레벨에서 적용됩니다. 다시 말해 6000 IOPS로 볼륨에 연결된 두 개의 호스트가 해당 6000 IOPS를 공유합니다.

## IOPS 측정
{: faq}

IOPS는 랜덤 50% 읽기 및 50% 쓰기의 16KB 블록 로드 프로파일을 기반으로 합니다. 이 프로파일과 다른 워크로드에서는 성능이 저하될 수 있습니다.

## 성능을 측정하는 데 더 작은 블록 크기를 사용하면 어떻게 됩니까?
{: faq}

더 작은 블록 크기를 사용해도 최대 IOPS를 얻을 수 있습니다. 그러나 처리량은 작아집니다. 예를 들어, 6000 IOPS의 볼륨의 경우 다양한 블록 크기에서 처리량은 다음과 같습니다.

- 16 KB * 6000 IOPS == ~93.75 MB/sec 
- 8 KB * 6000 IOPS == ~46.88 MB/sec
- 4 KB * 6000 IOPS == ~23.44 MB/sec

## 예상 처리량을 얻기 위해 볼륨을 미리 준비해야 합니까?
{: faq}

미리 준비할 필요는 없습니다. 볼륨을 프로비저닝하는 즉시 지정된 처리량을 관찰할 수 있습니다.

## 더 빠른 이더넷 연결을 사용하면 더 많은 처리량을 달성할 수 있습니까?
{: faq}

처리량 한계는 볼륨/LUN 레벨별로 설정되기 때문에 더 빠른 이더넷 연결을 사용해도 설정 한계는 늘어나지 않습니다. 그렇지만 더 느린 이더넷 연결을 사용하는 경우 대역폭에서 잠재적으로 병목 현상이 발생할 수 있습니다.

## 방화벽/보안 그룹이 성능에 영향을 줍니까?
{: faq}

방화벽을 우회하는 VLAN을 통해 스토리지 트래픽을 실행하는 것이 가장 좋습니다. 소프트웨어 방화벽을 통해 스토리지 트래픽을 실행하면 대기 시간이 늘어나서 결국 스토리지 성능이 저하됩니다.

## {{site.data.keyword.blockstorageshort}}의 예상 대기 시간은 얼마입니까?   
{: faq}

스토리지 내의 대상 대기 시간은 <1밀리초입니다. 스토리지는 공유 네트워크의 컴퓨팅 인스턴스에 연결되기 때문에 정확한 성능 대기 시간은 오퍼레이션 중의 네트워크 트래픽에 따라 달라집니다.

## Endurance10 IOPS/GB 티어가 있는 {{site.data.keyword.blockstorageshort}}는 일부 데이터 센터에서는 주문할 수 있지만 다른 데이터 센터에서는 주문할 수 없는 이유는 무엇입니까?
{: faq}

Endurance유형 {{site.data.keyword.blockstorageshort}}의 10 IOPS/GB 티어는 데이터 센터 선택 시에만 사용할 수 있으며, 새 데이터 센터는 단계적으로 추가됩니다. 업그레이드된 전체 데이터 센터 및 사용 가능한 기능을 [여기](new-ibm-block-and-file-storage-location-and-features.html)에서 확인할 수 있습니다.

## 암호화된 {{site.data.keyword.blockstorageshort}} LUN/볼륨을 어떻게 알 수 있습니까?
{: faq}

[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}에서 {{site.data.keyword.blockstorageshort}}의 목록을 보는 경우, 암호화된 LUN의 볼륨 이름 옆에 잠금 아이콘이 나타납니다. 

## 업그레이드된 데이터 센터에서 {{site.data.keyword.blockstorageshort}}를 프로비저닝하는 경우를 어떻게 알 수 있습니까?
{: faq}

{{site.data.keyword.blockstorageshort}} 주문 시에 모든 업그레이드된 데이터 센터에는 암호화를 사용하여 스토리지가 프로비저닝되는 것을 나타내기 위해 별표(`*`)가 표기됩니다. 스토리지가 프로비저닝되면, 하나 이상의 볼륨이 암호화되었음을 보여주는 아이콘이 스토리지 목록에 표시됩니다. 암호화된 볼륨 및 LUN이 모두 업그레이드된 데이터 센터에서만 프로비저닝됩니다. 업그레이드된 전체 데이터 센터 및 사용 가능한 기능을 [여기](new-ibm-block-and-file-storage-location-and-features.html)에서 확인할 수 있습니다.

## 최근에 업그레이드된 데이터 센터에 암호화되지 않은 {{site.data.keyword.blockstorageshort}}가 있으면 {{site.data.keyword.blockstorageshort}}를 암호화할 수 있습니까?
{: faq}

데이터 센터가 업그레이드되기 전에 프로비저닝되는 {{site.data.keyword.blockstorageshort}}를 암호화할 수 없습니다. 
업그레이드된 데이터 센터에서 프로비저닝된 새 {{site.data.keyword.blockstorageshort}}는 자동으로 암호화됩니다. 선택 가능한 암호화 설정이 없으며 이는 자동으로 수행됩니다. 
업그레이드된 데이터 센터에서 암호화되지 않은 스토리지의 데이터는 새 블록 LUN을 작성하고 호스트 기반 마이그레이션을 통해 데이터를 암호화된 새 LUN으로 복사하여 암호화할 수 있습니다. 자세한 내용은 [여기](migrate-block-storage-encrypted-block-storage.html)를 참조하십시오.

## Db2 pureScale에 대한 I/O 펜싱을 구현하기 위한 {{site.data.keyword.blockstorageshort}}의 SCSI-3 Persistent Reserve를 지원합니까?
{: faq}

예, {{site.data.keyword.blockstorageshort}}는  SCSI-2 및 SCSI-3 Persistent Reserve를 지원합니다.

## {{site.data.keyword.blockstorageshort}} 볼륨이 삭제되면 내 데이터는 어떻게 됩니까?
{: faq}

{{site.data.keyword.blockstoragefull}}는 재사용 전에 삭제되는 물리적 스토리지의 고객에게 블록 볼륨을 표시합니다. 미디어 무결 처리에 대한 NIST 800-88 가이드라인과 같이 규제 준수에 대한 특별 요구사항이 있는 고객은 스토리지를 삭제하기 전에 데이터 무결 처리 프로시저를 수행해야 합니다.

## 클라우드 데이터 센터에서 사용 중지된 드라이브는 어떻게 됩니까?
{: faq}

드라이브가 사용 중지되는 경우, 처분되기 전에 IBM에서 파기합니다. 드라이브를 사용할 수 없게 됩니다. 해당 드라이브에 쓰여진 모든 데이터는 더 이상 액세스가 불가능합니다.
