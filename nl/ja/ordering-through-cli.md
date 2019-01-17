---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# SL CLI を使用した {{site.data.keyword.blockstorageshort}} の注文

通常は、[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} を介して注文する製品を、SL CLI を使用して発注することができます。 SL API では 1 つの注文が複数の注文コンテナーで構成されている可能性があります。 注文の CLI は、1 つの注文コンテナーに対してのみ適用されます。

SL CLI をインストールして使用する方法について詳しくは、[Python API クライアント![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}を参照してください。
{:tip}

## 入手可能な {{site.data.keyword.blockstorageshort}} オファーの検索

発注する場合にまず検索するコンポーネントはパッケージです。 パッケージは、{{site.data.keyword.BluSoftlayer_full}} で注文できる、さまざまなトップレベルの製品別に分けられています。 パッケージの例として、VSI 用の CLOUD_SERVER、ベアメタル・サーバー用の BARE_METAL_SERVER、および {{site.data.keyword.blockstorageshort}} と {{site.data.keyword.filestorage_short}} 用の STORAGE_AS_A_SERVICE_STAA などがあります。

パッケージ内で、いくつかの項目はさらにカテゴリーに分けられています。 一部のパッケージではユーザーの便宜のために事前設定が含まれており、その他のパッケージでは項目を個別に指定する必要があります。 パッケージの任意のカテゴリーが必須である場合、パッケージを発注するにはそのカテゴリーから項目を選択する必要があります。 カテゴリーによっては、カテゴリー内のいくつかの項目が相互に排他的である可能性があります。

注文ごとに、関連付けられたロケーション (データ・センター) が必要です。 {{site.data.keyword.blockstorageshort}} を注文する場合、コンピューティング・インスタンスと同じ場所にプロビジョンされることを確認してください。
{:important}

`slcli order package-list` コマンドを使用すると、発注するパッケージを見つけることができます。簡単に検索やフィルター操作を行うための `–keyword` オプションが提供されています。 このオプションを使用すると、必要なパッケージを見つけやすくなります。**Storage-as-a-Service Package 759** を探します。

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  placeOrder API によって注文できるパッケージをリストします。

  Example:
      # List out all packages for ordering
      slcli order package-list

  一部の簡易フィルター機能では、キーワードを使用してより簡単に
  パッケージを検索することもできます。

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

`slcli block volume-order` コマンドを使用することもできます。

```
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Block Storage ボリュームを注文します。

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

API を使用した {{site.data.keyword.blockstorageshort}} の注文について詳しくは、[order_block_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}を参照してください。
すべての新規機能を利用できるようにするには、「`Storage-as-a-Service Package 759`」を発注してください。
{:tip}


## 発注

次の例は、80 GB の {{site.data.keyword.blockstorageshort}} ボリュームを 20 GB のスナップショット・スペースおよび 0.25 IOPS/GB と合わせて注文する方法を示しています。

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} ボリュームをプロビジョンできます。ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 制限の引き上げについて詳しくは、[ストレージ制限の管理](managing-storage-limits.html)を参照してください。
{:important}

## 新規ストレージにアクセスするためのホストの許可

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

  ホストに指定ボリュームへのアクセスを許可します

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

API を使用した {{site.data.keyword.blockstorageshort}} にアクセスするためのホストの許可について詳しくは、[authorize_host_to_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}を参照してください。
{:tip}

同時許可の制限については、[FAQ](faqs.html) を参照してください。
{:important}

## 新規ストレージの接続

ホストのオペレーティング・システムに応じて、適切なリンクをたどってください。
- [Linux での iSCSI LUN への接続](accessing_block_storage_linux.html)
- [CloudLinux での iSCSI LUN への接続](configure-iscsi-cloudlinux.html)
- [Microsoft Windows での iSCSI LUN への接続](accessing-block-storage-windows.html)
- [cPanel を使用したバックアップ用のブロック・ストレージの構成](configure-backup-cpanel.html)
- [Plesk を使用したバックアップ用のブロック・ストレージの構成](configure-backup-plesk.html)
