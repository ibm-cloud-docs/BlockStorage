---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 透過 SLCLI 訂購 {{site.data.keyword.blockstorageshort}}
{: #orderingthroughCLI}

您可以使用 SLCLI 來訂購通常是透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 來訂購的產品。在 SL API 中，一張訂單可能是由多重訂單容器所組成。訂單 CLI 只能用於一個訂單容器。

若要進一步瞭解如何安裝及使用 SLCLI，請參閱 [Python API 用戶端](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}。
{:tip}

## 搜尋可用的 {{site.data.keyword.blockstorageshort}} 供應項目

當您下訂單時，所要尋找的第一個元件是套件。套件分散在 {{site.data.keyword.cloud}} 中可供訂購的不同頂層產品之間。套件的部分範例包括適用於 VSI 的 CLOUD_SERVER、適用於裸機伺服器的 BARE_METAL_SERVER，以及適用於 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 的 STORAGE_AS_A_SERVICE_STAAS。

在套件中，有些項目會再細分為種類。有些套件有預設項目以方便您使用，有些套件則需要個別指定項目。如果需要套件的種類，則必須從該種類中選擇項目來訂購套件。種類中的某些項目可能會互斥，視種類而定。

每個訂單都必須要有相關聯的位置（資料中心）。當您訂購 {{site.data.keyword.blockstorageshort}} 時，請確定其佈建在與運算實例相同的位置。
{:important}

您可以使用 `slcli order package-list` 指令來尋找您想要訂購的套件。有提供 `–keyword` 選項來執行簡單的搜尋和過濾。此選項可讓您更容易找到所需的套件。請尋找 **Storage-as-a-Service Package 759**。

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

您也可以使用 `slcli block volume-order` 指令。

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
                                 The service offering package to use for
                                 placing the order [optional, default is
                                 'storage_as_a_service']
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

如需透過 API 來訂購 {{site.data.keyword.blockstorageshort}} 的相關資訊，請參閱 [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}。若要能夠存取所有新增特性，請訂購 `Storage-as-a-Service Package 759`。
{:tip}


## 下訂單

下列範例顯示如何訂購具有 20 GB Snapshot 空間及每 GB 具有 0.25 IOPS 的 80 GB {{site.data.keyword.blockstorageshort}} 磁區。

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 磁區。若要增加磁區數目，請與業務代表聯絡。如需增加限制的相關資訊，請參閱[管理儲存空間限制](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)。
{:important}

## 授權主機存取新的儲存空間

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

若要進一步瞭解如何授權主機透過 API 來存取 {{site.data.keyword.blockstorageshort}}，請參閱 [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}。
{:tip}

如需同時授權的限制，請參閱[常見問題](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs)。
{:important}

## 連接新的儲存空間
{: #mountingCLI}

根據主機的作業系統而定，遵循適當的鏈結。
- [在 Linux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上連接至 LUNS](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [配置 Block Storage 以便使用 cPanel 進行備份](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [配置 Block Storage 以便使用 Plesk 進行備份](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
