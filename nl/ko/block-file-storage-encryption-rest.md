---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 제공자 관리 저장 시 암호화
{: #encryption}

## {{site.data.keyword.blockstorageshort}} 저장 시 암호화

{{site.data.keyword.BluSoftlayer_full}}는 보안을 중시하고 데이터를 안전하게 유지하기 위한 암호화의 중요성에 대해 이해하고 있습니다. 제공자 관리 암호화를 사용하면 Endurance 또는 Performance로 프로비저닝되는 {{site.data.keyword.blockstoragefull}}가 기본적으로 성능에 영향을 주지 않으면서 추가 비용 없이 암호화됩니다.

제공자 관리 저장 시 암호화 기능은 다음과 같은 산업 표준 프로토콜을 사용합니다.

* 산업 표준 AES-256 암호화
* 키는 산업 표준 KMIP(Key Management Interoperability Protocol)를 사용하여 내부에서 관리됩니다.
* 스토리지는 FIPS(Federal Information Processing Standard) Publication 140-2, FISMA(Federal Information Security Management Act) 및 HIPAA(Health Insurance Portability and Accountability Act)에 대해 유효성 검증됩니다. 또한 스토리지는 PCI(Payment Card Industry), Basel II, California Security Breach Information Act(SB 1386) 및 EU Data Protection Directive 95/46/EC 준수에 대해서도 유효성 검증됩니다.

## 스냅샷 및 복제 스토리지를 위한 저장 시 암호화 제공  

암호화된 {{site.data.keyword.blockstorageshort}}의 모든 스냅샷과 복제본은 기본적으로 암호화됩니다. 볼륨별로 이 기능을 끌 수는 없습니다.

## 암호화를 사용하여 스토리지 프로비저닝

제공자 관리 저장 시 암호화 기능은 [데이터 센터](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)에서 프로비저닝된 {{site.data.keyword.blockstorageshort}}에 사용할 수 있습니다. 해당 데이터 센터에서 주문되는 모든 스토리지는 암호화를 사용하여 자동으로 프로비저닝됩니다.

{{site.data.keyword.blockstorageshort}} 주문 시, 별표(`*`)가 있는 데이터 센터를 선택하십시오. 볼륨이 암호화되어 있음을 나타내는 잠금 아이콘이 LUN/볼륨 이름 필드의 오른쪽에 표시됩니다.

![잠금 아이콘은 LUN이 암호화되어 있음을 나타냅니다.](/images/encryptedstorage.png)
<caption>그림 1. LUN이 암호화되었음을 나타내는 잠금 아이콘 예제</caption>



데이터 센터가 업그레이드되기 전에 프로비저닝된 암호화되지 않은 스토리지는 자동으로 암호화되지 **않습니다**. 업그레이드된 데이터 센터에 암호화되지 않은 스토리지가 있지만 암호화된 스토리지를 원하는 경우, 새 볼륨을 작성하고 데이터를 마이그레이션해야 합니다. 자세한 정보는 [{{site.data.keyword.blockstorageshort}} 업그레이드된 데이터 센터에서 마이그레이션](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage)을 참조하십시오.
{:important}
