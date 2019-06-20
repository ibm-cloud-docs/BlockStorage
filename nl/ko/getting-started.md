---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-08"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# 시작하기 튜토리얼
{: #getting-started}

{{site.data.keyword.blockstoragefull}}는 컴퓨팅 인스턴스와 상관없이 프로비저닝 및 관리되는 지속적인 고성능 iSCSI 스토리지입니다. iSCSI 기반의 {{site.data.keyword.blockstorageshort}} LUN은 중복 다중 경로 I/O(MPIO) 연결을 통해 권한 부여된 디바이스에 연결됩니다.

{{site.data.keyword.blockstorageshort}}는 일치하지 않는 기능 세트를 사용하여 최고의 내구성 및 가용성을 제공합니다. 업계 표준 및 우수 사례를 사용하여 빌드되었습니다. {{site.data.keyword.blockstorageshort}}는 일관성 있는 성능 기준을 제공하여 데이터 무결성을 보호하고 유지보수 이벤트 및 플랜되지 않은 실패를 통해 가용성은 유지하도록 디자인되었습니다.
{:shortdesc}

## 시작하기 전에
{: #prereqs}

{{site.data.keyword.blockstorageshort}} LUN은 20GB부터 12TB까지 프로비저닝 가능하며 다음과 같은 두 개의 옵션이 있습니다. <br/>
- 사전 정의된 성능 레벨과 스냅샷 및 복제와 같은 기타 기능을 제공하는 **Endurance** 티어를 프로비저닝합니다.
- 초당 할당된 입출력(I/O) 오퍼레이션(IOPS)이 있는 강력한 **Performance** 환경을 빌드합니다.

{{site.data.keyword.blockstorageshort}} 오퍼링에 대한 자세한 정보는 [{{site.data.keyword.blockstorageshort}}에 대해 알아보기](/docs/infrastructure/BlockStorage?topic=BlockStorage-About)를 참조하십시오.

## 프로비저닝 고려사항

### 블록 크기

Endurance 및 Performance의 IOPS는 50/50 읽기/쓰기 50/50 랜덤/순차 워크로드의 16KB 블록 크기를 기반으로 합니다. 16KB 블록은 볼륨에 한 번 쓰기와 동등합니다.
{:important}

애플리케이션에서 사용하는 블록 크기는 스토리지 성능에 직접적인 영향을 줍니다. 애플리케이션에서 사용하는 블록 크기가 16KB보다 작은 경우, IOPS 한계가 처리량 한계 이전에 실현됩니다. 반대로, 애플리케이션에서 사용하는 블록 크기가 16KB보다 큰 경우, 처리량 한계가 IOPS 한계 이전에 실현됩니다.

|블록 크기(KB) |IOPS |처리량(MB/s) |
|-----|-----|-----|
|4 |1,000 |16 |
|8 |1,000 |16 |
|16 |1,000 |16 |
|32 |500 |16 |
|64 |250 |16 |
|128 |128 |16 |
|512 |32 |16 |
|1024 |16 |16 |
{: caption="표 1에는 블록 크기 및 IOPS가 처리량에 미치는 영향에 대한 예가 표시되어 있습니다.<br/평균 IO 크기 x IOPS = 처리량(MB/초)" caption-side="top"}

### 권한 부여된 호스트

또한, 볼륨을 사용 중인 호스트 수도 고려해야 합니다. 볼륨에 액세스하는 단일 호스트가 있는 경우, 사용 가능한 최대 IOPS를 실현하는 것은 어려울 수 있으며 특히 10,000과 같이 IOPS 수가 큰 경우에는 더욱 그렇습니다. 워크로드에 높은 처리량이 요구되는 경우, 단일 서버 병목 현상을 방지하기 위해 최소한 두서너 개의 서버가 볼륨에 액세스하도록 구성하는 것이 바람직합니다.

### 네트워크 연결

이더넷 연결 속도는 볼륨의 최대 예상 처리량보다 빨라야 합니다. 일반적으로 이더넷 연결이 포화 상태가 되지 않으려면 사용 가능한 대역폭의 70%를 넘지 않아야 합니다. 예를 들어, IOPS가 6,000이며 16KB 블록 크기를 사용하는 경우 볼륨은 약 94MBps의 처리량을 처리할 수 있습니다. LUN에 대해 1Gbps의 이더넷 연결을 보유하고 있는 경우 서버가 사용 가능한 최대 처리량을 사용하려고 시도하면 병목 현상이 발생합니다. 이는 1Gbps 이더넷 연결(초당 125MB)에 대한 70퍼센트 이론적 한계가 초당 88MB까지만 허용하기 때문입니다.

최대 IOPS를 달성하려면 적절한 네트워크 리소스가 사용 가능해야 합니다. 그 외에도 스토리지 외부의 사설 네트워크 사용량과 호스트 측 및 애플리케이션 특정 튜닝(IP 스택 또는 [큐 깊이](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings) 및 기타 설정)도 고려해야 합니다.

스토리지 트래픽은 다른 트래픽 유형과 격리되어야 하며 방화벽 및 라우터를 통과하지 않도록 해야 합니다. 자세한 정보는 [FAQ](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic)를 참조하십시오.

스토리지 트래픽은 공용 Virtual Server의 총 네트워크 사용에 포함됩니다. 서비스에서 부과할 수 있는 한계에 관한 자세한 정보는 [Virtual Server 문서](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers)를 참조하십시오.
{:tip}

## 주문 제출
{: #submitorder}

주문을 제출할 준비가 되면 [콘솔](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole)이나 [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)를 통해 주문을 제출하십시오.

## 새 스토리지 연결
{: #mountingstorage}

프로비저닝 요청이 완료되면 새 스토리지에 액세스할 수 있는 권한을 호스트에 부여하고 연결을 구성하십시오. 호스트의 운영 체제에 따라 해당 링크를 따르십시오.
- [Linux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [cPanel을 사용하여 Block Storage 구성](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [cPanel을 사용하여 Block Storage 구성](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## 새 스토리지 관리

포털 또는 SLCLI를 통해 {{site.data.keyword.blockstorageshort}}의 다양한 측면(예: 호스트 권한 부여 및 취소)을 관리할 수 있습니다. 자세한 정보는 [{{site.data.keyword.blockstorageshort}} 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)를 참조하십시오.
