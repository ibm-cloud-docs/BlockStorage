---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# SLCLI를 통해 {{site.data.keyword.blockstorageshort}} 주문
{: #orderingthroughCLI}

일반적으로 [{{site.data.keyword.cloud_notm}} 콘솔](https://{DomainName}/){: external}을 통해 주문하는 제품을 주문하는 데 SLCLI를 사용할 수 있습니다. SL API에서, 주문은 여러 주문 컨테이너로 구성됩니다. 주문 CLI는 하나의 주문 컨테이너와만 작동합니다.

SLCLI 설치 및 사용 방법에 대한 자세한 정보는 [Python API 클라이언트](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}를 참조하십시오.
{:tip}

## 사용 가능한 {{site.data.keyword.blockstorageshort}} 오퍼 검색

주문할 때 검색하는 첫 번째 컴포넌트가 패키지입니다. 패키지는 {{site.data.keyword.cloud}}에서 주문하는 데 사용할 수 있는 다양한 최상위 레벨 제품으로 구분됩니다. 몇 가지 예제 패키지는 VSI의 경우 CLOUD_SERVER, 베어메탈 서버의 경우 BARE_METAL_SERVER, {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}}의 경우 STORAGE_AS_A_SERVICE_STAAS입니다.

패키지에서 일부 항목은 카테고리로 분할됩니다. 일부 패키지에는 편의를 위한 사전 설정 세트가 있으며 다른 패키지에서는 항목을 개별적으로 지정해야 합니다. 패키지의 카테고리가 필요한 경우, 패키지를 주문하려면 해당 카테고리의 항목을 선택해야 합니다. 카테고리에 따라 카테고리에 있는 일부 항목은 상호 배타적일 수 있습니다.

각 주문에는 위치(데이터 센터)가 연관되어 있어야 합니다. {{site.data.keyword.blockstorageshort}}를 주문할 때 컴퓨팅 인스턴스와 동일한 위치에서 프로비저닝되는지 확인하십시오.
{:important}

`slcli order package-list` 명령을 사용하여 주문할 패키지를 찾을 수 있습니다. `–keyword` 옵션은 단순 검색 및 필터링을 수행하도록 제공됩니다. 이 옵션을 사용하면 필요한 패키지를 찾기가 쉬워집니다. **Storage-as-a-Service Package 759**를 검색하십시오.

```
$ slcli order package-list --help
사용법: slcli order package-list [OPTIONS]

  placeOrder API를 통해 주문할 수 있는 패키지를 나열합니다.

  예:
      # 주문 가능한 모든 패키지 나열
      slcli order package-list

  패키지를 더 쉽게 찾을 수 있도록 일부 간단한 필터링 기능에도 키워드에
  사용할 수 있습니다.

  예:
     # 이름에 "server"가 있는 모든 패키지 나열
      slcli order package-list --keyword server

옵션:
  --keyword TEXT  패키지 이름을 필터링하는 데 사용하는 단어(또는 문자열)입니다.
  -h, --help      이 메시지를 표시하고 종료합니다.
```

`slcli block volume-order` 명령을 사용할 수도 있습니다.

```
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Order a block storage volume.

Options:
 --storage-type [performance|endurance]
                                 Type of block storage volume  [required]
 --size INTEGER                  Size of block storage volume in GB.
                                 Permitted Sizes:
                                 20, 40, 80, 100, 250, 500,
                                 1000, 2000, 4000, 8000, 12000  [required]
 --iops INTEGER                  Performance Storage IOPs, between 100 and
                                 6000 in multiples of 100  [required for
                                 storage-type performance]
 --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                 [required for storage-type endurance]
 --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                 Operating System  [required]
 --location TEXT                 Datacenter short name (e.g.: dal09)
                                 [required]
 --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                 space along with endurance block storage;
                                 specifies the size (in GB) of snapshot space
                                 to order
 --service-offering [storage_as_a_service|enterprise|performance]
                                 The default is 'storage_as_a_service'
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

API를 통해 {{site.data.keyword.blockstorageshort}}를 주문하는 데 대한 자세한 정보는 [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}을 참조하십시오.
새 기능에 모두 액세스할 수 있으려면 `Storage-as-a-Service Package 759`를 주문하십시오.
{:tip}

Window OS 유형에 대한 자세한 정보는 [FAQ](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes)를 참조하십시오.


## 주문하기

다음 예에서는 20GB 스냅샷 영역 및 GB당 0.25IOPS를 포함한 80GB {{site.data.keyword.blockstorageshort}} 볼륨을 주문하는 방법을 보여줍니다.

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

기본적으로 사용자는 총 250개의 {{site.data.keyword.blockstorageshort}} 및 {{site.data.keyword.filestorage_short}} 볼륨을 프로비저닝할 수 있습니다. 볼륨 수를 늘리려면 영업 담당자에게 문의하십시오. 한계를 늘리는 데 관한 자세한 정보는 [스토리지 한계 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)를 참조하십시오.
{:important}

## 호스트에 새 스토리지에 액세스하는 권한 부여

```
slcli block access-authorize --help
사용법: slcli block access-authorize [OPTIONS] VOLUME_ID

주어진 볼륨에 액세스할 수 있는 권한을 호스트에 부여합니다.

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

사용자의 스토리지와 동일한 데이터 센터에 있는 호스트에 권한 부여하고 연결할 수 있습니다. 다수의 계정을 보유할 수는 있지만, 한 계정의 호스트에 권한 부여하여 다른 계정의 스토리지에 액세스할 수는 없습니다. 호스트는 동시에 서로 다른 OS 유형의 여러 LUN에 액세스할 수 없습니다. 호스트는 단일 OS 유형의 LUN에만 액세스할 수 있는 권한이 있습니다. 서로 다른 OS 유형의 여러 LUN에 액세스할 수 있는 권한을 부여하려는 경우 오퍼레이션에 오류가 발생합니다.
{:note}
{:important}

호스트에 API를 통해 {{site.data.keyword.blockstorageshort}}에 액세스하는 권한을 부여하는 데 대한 자세한 정보는 [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}을 참조하십시오.
{:tip}

동시 권한 부여 한계에 대해서는 [FAQs](/docs/infrastructure/BlockStorage?topic=block-storage-faqs)를 참조하십시오.
{:important}


## 새 스토리지 연결
{: #mountingCLI}

호스트의 운영 체제에 따라 해당 링크를 따르십시오.
- [Linux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows에서 LUN에 연결](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [cPanel을 사용하여 Block Storage 구성](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [cPanel을 사용하여 Block Storage 구성](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
