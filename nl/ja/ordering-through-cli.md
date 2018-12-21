---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# SL CLI を使用した {{site.data.keyword.blockstorageshort}} の注文

通常は、[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} を介して注文する製品を、SL CLI を使用して発注することができます。SL API では 1 つの注文が複数の注文コンテナーで構成されている可能性があります。注文の CLI は、1 つの注文コンテナーに対してのみ適用されます。

SL CLI をインストールして使用する方法について詳しくは、[Python API クライアント![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}を参照してください。
{:tip}

## 入手可能な {{site.data.keyword.blockstorageshort}} オファーの検索

発注する場合にまず検索するコンポーネントはパッケージです。パッケージは、{{site.data.keyword.BluSoftlayer_full}} で注文できる、さまざまなトップレベルの製品別に分けられています。パッケージの例として、VSI 用の CLOUD_SERVER、ベアメタル・サーバー用の BARE_METAL_SERVER、および {{site.data.keyword.blockstorageshort}} と {{site.data.keyword.filestorage_short}} 用の STORAGE_AS_A_SERVICE_STAA などがあります。

パッケージ内で、いくつかの項目はさらにカテゴリーに分けられています。一部のパッケージではユーザーの便宜のために事前設定が含まれており、その他のパッケージでは項目を個別に指定する必要があります。パッケージの任意のカテゴリーが必須である場合、パッケージを発注するにはそのカテゴリーから項目を選択する必要があります。カテゴリーによっては、カテゴリー内のいくつかの項目が相互に排他的である可能性があります。

注文ごとに、関連付けられたロケーション (データ・センター) が必要です。{{site.data.keyword.blockstorageshort}} を注文する場合、コンピューティング・インスタンスと同じ場所にプロビジョンされることを確認してください。
{:important}

`slcli order package-list` コマンドを使用すると、発注するパッケージを見つけることができます。簡単に検索やフィルター操作を行うための `–keyword` オプションが提供されています。このオプションを使用すると、必要なパッケージを見つけやすくなります。

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

*「Storage-as-a-Service Package 759」を見つける方法に関する指示が必要です*

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

`item-list` コマンドを使用して注文の残りの項目を選択します。パッケージには、選択対象の項目が多数含まれているため、`–category` オプションを使用して必要なカテゴリーの項目のみを取得します。

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

API を使用した {{site.data.keyword.blockstorageshort}} の注文について詳しくは、[order_block_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}を参照してください。
すべての新規機能を利用できるようにするには、「`Storage-as-a-Service Package 759`」を発注してください。{:tip}

## 注文の検証

注文で必須カテゴリーが欠落しているかどうか確信が持てない場合は、`–verify` フラグを設定した `place` コマンドを使用できます。欠落しているカテゴリーがある場合には画面に出力されます。


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

出力には、発注されている各項目と、その項目に関連するコストが表示されます。注文が検証にパスした場合、その注文には競合している項目が存在せず、必須カテゴリーすべてに指定された項目が含まれていることを示します。

## 発注

次のステップは発注です。

```
$ slcli order place .....

This action will incur charges on your account. Continue? [y/N]: y

API response
```

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} ボリュームをプロビジョンできます。 ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 制限の引き上げについて詳しくは、[ストレージ制限の管理](managing-storage-limits.html)を参照してください。{:important}

## 新規ストレージにアクセスするためのホストの許可

TBD

API を使用した {{site.data.keyword.blockstorageshort}} にアクセスするためのホストの許可について詳しくは、[authorize_host_to_volume ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}を参照してください。
{:tip}

同時許可の制限については、[FAQ](faqs.html) を参照してください。
{:important}

## 新規ストレージの接続

ホストのオペレーティング・システムに応じて、適切なリンクをたどってください。
- [Linux での MPIO iSCSI LUN への接続](accessing_block_storage_linux.html)
- [CloudLinux での MPIO iSCSI LUN への接続](configure-iscsi-cloudlinux.html)
- [Microsoft Windows での MPIO iSCSI LUN への接続](accessing-block-storage-windows.html)
- [cPanel を使用したバックアップ用のブロック・ストレージの構成](configure-backup-cpanel.html)
- [Plesk を使用したバックアップ用のブロック・ストレージの構成](configure-backup-plesk.html)
