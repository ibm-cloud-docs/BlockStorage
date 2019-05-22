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

# 通过 SLCLI 订购 {{site.data.keyword.blockstorageshort}}
{: #orderingthroughCLI}

您可以使用 SLCLI 以针对通常通过 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 订购的产品下订单。在 SL API 中，订单可由多个订单容器组成。订单 CLI 仅使用一个订单容器。

有关如何安装和使用 SLCLI 的更多信息，请参阅 [Python API 客户机](https://softlayer-python.readthedocs.io/en/latest/cli.html){: external}。
{:tip}

## 搜索可用的 {{site.data.keyword.blockstorageshort}} 产品

下订单时要查找的第一个组件是包。在可供在 {{site.data.keyword.BluSoftlayer_full}} 中进行订购的不同顶级产品之间拆分包。一些示例包为 CLOUD_SERVER（用于 VSI）、BARE_METAL_SERVER（用于裸机服务器）和 STORAGE_AS_A_SERVICE_STAAS（用于 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}}）。

利用包，可将某些项细分到类别。为方便起见，某些包有预设项，而其他包需要单独指定项。如果包的类别是必需的，那么必须选择该类别的项来订购包。根据类别，类别中的某些项可能互斥。

每个订单必须具有关联的位置（数据中心）。在订购 {{site.data.keyword.blockstorageshort}} 时，确保在与计算实例相同的位置中进行供应。
{:important}

要查找想要订购的包，可以使用 `slcli order package-list` 命令。提供了 `–keyword` 选项以用于执行简单搜索和过滤。通过此选项，可以更轻松地查找所需包。请查找 **Storage-as-a-Service Package 759**。

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

此外，还可以使用 `slcli block volume-order` 命令。

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

有关通过 API 订购 {{site.data.keyword.blockstorageshort}} 的更多信息，请参阅 [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}。
要能够访问所有新功能，请订购 `Storage-as-a-Service Package 759`。
{:tip}


## 下订单

以下示例显示了如何订购一个 80 GB 的 {{site.data.keyword.blockstorageshort}} 卷（包含 20 GB 快照空间和 0.25 IOPS/GB）。

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 卷。要增加卷的数量，请联系销售代表。有关提高限制的更多信息，请参阅[管理存储限制](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)。
{:important}

## 授权主机访问新存储器

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

选项：
  -h, --hardware-id TEXT    要授权的 SoftLayer_Hardware 的标识
  -v, --virtual-id TEXT     要授权的 SoftLayer_Virtual_Guest 的标识
  -i, --ip-address-id TEXT  要授权的 SoftLayer_Network_Subnet_IpAddress 的标识
  --ip-address TEXT         要授权的 IP 地址
  --help                    显示此消息并退出。
```

有关通过 API 授权主机访问 {{site.data.keyword.blockstorageshort}} 的更多信息，请参阅 [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}
{:tip}

有关同时授权的限制，请参阅[常见问题](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs)。
{:important}

## 连接新存储器
{: #mountingCLI}

根据主机的操作系统，访问相应的链接。
- [在 Linux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上连接到 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [使用 cPanel 配置 Block Storage 进行备份](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [使用 Plesk 配置 Block Storage 进行备份](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
