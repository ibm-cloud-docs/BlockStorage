---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 通过 SL CLI 订购 {{site.data.keyword.blockstorageshort}}

您可以使用 SL CLI 以针对通常通过 [{{site.data.keyword.slportal}} ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://control.softlayer.com/){:new_window} 订购的产品下订单。在 SL API 中，订单可由多个订单容器组成。订单 CLI 仅使用一个订单容器。

有关如何安装和使用 SL CLI 的更多信息，请参阅 [Python API 客户机 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}。
{:tip}

## 搜索可用的 {{site.data.keyword.blockstorageshort}} 产品

下订单时要查找的第一个组件是包。在可供在 {{site.data.keyword.BluSoftlayer_full}} 中进行订购的不同顶级产品之间拆分包。一些示例包为 CLOUD_SERVER（用于 VSI）、BARE_METAL_SERVER（用于裸机服务器）和 STORAGE_AS_A_SERVICE_STAAS（用于 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}}）。

利用包，可将某些项细分到类别。为方便起见，某些包有预设项，而其他包需要单独指定项。如果包的类别是必需的，那么必须选择该类别的项来订购包。根据类别，类别中的某些项可能互斥。

每个订单必须具有关联的位置（数据中心）。在订购 {{site.data.keyword.blockstorageshort}} 时，确保在与计算实例相同的位置中进行供应。
{:important}

您可以使用 `slcli order package-list` 命令以查找想要订购的包。提供了 `–keyword` 选项以用于执行简单搜索和过滤。通过此选项，可以更轻松地查找所需包。

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

*需要有关如何查找 Storage-as-a-Service Package 759 的指示信息*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         name        :       keyName       :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               name               :    categoryCode   : isRequired :
:..................................:...................:............:
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:..................................:...................:............:
```

通过使用 `item-list` 命令，为订单选择其余项。包通常具有众多项以供选择，因此使用 `–category` 选项以仅从感兴趣的类别中检索项。

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         keyName          :                description                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

有关通过 API 订购 {{site.data.keyword.blockstorageshort}} 的更多信息，请参阅 [order_block_volume ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}。
要能够访问所有新功能，请订购 `Storage-as-a-Service Package 759`。
{:tip}

## 验证订单

如果您不确定订单中可能缺少的必需类别，可以使用带 `–verify` 标志的 `place` 命令。如果缺少任何类别，会在屏幕上将其打印出来。


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                keyName                       :                   description                   : cost :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

输出显示所要订购的每个项以及与该项相关联的成本。如果订单通过验证，那么意味着无冲突项，并且所有必需类别具有在订单中指定的项。

## 下订单

下一步是下订单。

```
$ slcli order place .....

This action will incur charges on your account. Continue? [y/N]: y

API response
```

缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 卷。要增加卷的数量，请联系销售代表。有关提高限制的更多信息，请参阅[管理存储限制](managing-storage-limits.html)。
{:important}

## 授权主机访问新存储器

TBD

有关通过 API 授权主机访问 {{site.data.keyword.blockstorageshort}} 的更多信息，请参阅 [authorize_host_to_volume ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}
{:tip}

有关同时授权的限制，请参阅[常见问题](faqs.html)。
{:important}

## 连接新存储器

根据主机的操作系统，访问相应的链接。
- [在 Linux 上连接到 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上连接到 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上连接到 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [使用 cPanel 配置 Block Storage 进行备份](configure-backup-cpanel.html)
- [使用 Plesk 配置 Block Storage 进行备份](configure-backup-plesk.html)
