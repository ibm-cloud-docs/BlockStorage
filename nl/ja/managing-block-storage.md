---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}}の管理
{: #managingstorage}

{{site.data.keyword.blockstoragefull}} のボリュームは、[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window}で管理できます。

## {{site.data.keyword.blockstorageshort}} LUN の詳細の表示

ストレージに追加されたスナップショットとレプリケーションの機能など、選択したストレージ LUN に関する主要情報の要約を表示できます。

1. **「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. リストから適切な LUN 名をクリックします。

あるいは、次の SLCLI コマンドを使用できます。
```
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## {{site.data.keyword.blockstorageshort}}にアクセスするホストの許可

「許可」ホストとは、特定の LUN に対するアクセス権が付与されたホストのことです。 ホストの許可がなければ、システムからストレージにアクセスすることも、ストレージを使用することもできません。 ホストに LUN へのアクセスを許可すると、ユーザー名とパスワードの他に、iSCSI 修飾名 (IQN) が生成されます。これは、マルチパス入出力 (MPIO) iSCSI 接続をマウントするために必要です。

ご使用のストレージと同じデータ・センターにあるホストを許可および接続できます。 複数のアカウントを持つことはできますが、あるアカウントのホストから別のアカウントのストレージへのアクセスを許可することはできません。
{:important}

1. **「ストレージ」** -> **「{{site.data.keyword.blockstorageshort}}」**をクリックし、LUN 名をクリックします。
2. 当該ページの**「許可ホスト (Authorized Hosts)」**セクションにスクロールします。
3. 右側で、**「ホストの許可」**をクリックします。 その特定の LUN にアクセスできるホストを選択します。

あるいは、次の SLCLI コマンドを使用できます。
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

## {{site.data.keyword.blockstorageshort}} LUN へのアクセスを許可されたホストのリストの表示

1. **「ストレージ」** -> **「{{site.data.keyword.blockstorageshort}}」**をクリックし、LUN 名をクリックします。
2. **「許可ホスト (Authorized Hosts)」**セクションまでスクロールダウンします。

LUN へのアクセスが現在許可されているホストのリストが表示されます。 また、接続を確立するために必要な認証情報 (ユーザー名、パスワード、ホスト IQN) も表示されます。 ターゲット・アドレスは、**「ストレージの詳細」**ページにリストされます。 NFS の場合、ターゲット・アドレスは DNS 名として示され、iSCSI の場合は「ターゲット ポータルの探索」の IP アドレスとして示されます。

あるいは、次の SLCLI コマンドを使用できます。
```
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## ホストに許可された{{site.data.keyword.blockstorageshort}}の表示

ホストがアクセス権を持っている LUN を表示できます。接続の確立に必要な情報 (LUN 名、ストレージ・タイプ、ターゲット・アドレス、容量、ロケーション) も表示されます。

1. [{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://control.softlayer.com/){:new_window} で、**「デバイス」** > **「デバイス・リスト」** をクリックし、目的のデバイスをクリックします。
2. **「ストレージ」**タブを選択します。

この特定のホストがアクセス権を持っているストレージ LUN のリストが表示されます。 リストはストレージ・タイプ (ブロック、ファイル、その他) ごとにグループ化されています。 **「アクション」**をクリックすると、追加のストレージを許可したり、アクセス権を削除したりできます。

## {{site.data.keyword.blockstorageshort}} のマウントとアンマウント

ホストのオペレーティング・システムに基づいて、該当する手順に従います。

- [Linux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [cPanel を使用したバックアップ用のブロック・ストレージの構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Plesk を使用したバックアップ用のブロック・ストレージの構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## {{site.data.keyword.blockstorageshort}}に対するホストのアクセス権の取り消し

ホストから特定のストレージ LUN へのアクセスを停止する場合は、アクセス権を取り消すことができます。 アクセス権を取り消すと、ホスト接続が LUN からドロップされます。 そのホスト上のオペレーティング・システムとアプリケーションは、LUN と通信できなくなります。

ホスト側での問題を回避するには、アクセス権を取り消す前に、ストレージ LUN をオペレーティング・システムからアンマウントして、ドライブの欠落やデータ破損の発生を防止してください。
{:important}

**「デバイス・リスト」**または**「ストレージ」**ビューからアクセス権を取り消すことができます。

### デバイス・リストからのアクセス権の取り消し

1. [{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} で、**「デバイス」**、**「デバイス・リスト」**をクリックし、目的のデバイスをダブルクリックします。
2. **「ストレージ」**タブを選択します。
3. この特定のホストがアクセス権を持っているストレージ LUN のリストが表示されます。 リストはストレージ・タイプ (ブロック、ファイル、その他) ごとにグループ化されています。 LUN 名の横にある**「アクション」**を選択し、「アクセスの取り消し」**をクリックします。
4. このアクションは元に戻すことができないため、LUN に対するアクセス権を取り消すことを確認します。 LUN のアクセス権を取り消すには**「はい」**をクリックし、アクションをキャンセルする場合は**「いいえ」**をクリックします。

特定のホストから複数の LUN を切断する場合は、LUN ごとに「アクセス権の取り消し」アクションを繰り返す必要があります。
{:tip}


### 「ストレージ」ビューからのアクセス権の取り消し

1. **「ストレージ」**、
**「{{site.data.keyword.blockstorageshort}}」**をクリックし、アクセス権を取り消す LUN を選択します。
2. **「許可ホスト (Authorized Hosts)」**にスクロールします。
3. アクセス権を取り消すホストの隣にある**「アクション」**をクリックして、**「アクセス権の取り消し」**を選択します。
4. このアクションは元に戻すことができないため、LUN に対するアクセス権を取り消すことを確認します。 LUN のアクセス権を取り消すには**「はい」**をクリックし、アクションをキャンセルする場合は**「いいえ」**をクリックします。

特定の LUN から複数のホストを切断する場合は、ホストごとに「アクセス権の取り消し」アクションを繰り返す必要があります。
{:tip}

### SLCLI を使用したアクセス権の取り消し

あるいは、次の SLCLI コマンドを使用できます。
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to revoke
                            authorization
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to revoke
                            authorization
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to revoke authorization
  --ip-address TEXT         An IP address to revoke authorization
  --help                    Show this message and exit.
```

## ストレージ LUN のキャンセル

特定の LUN が不要になった場合は、その LUN をいつでもキャンセルできます。

ストレージ LUN をキャンセルするには、最初にすべてのホストのアクセス権を取り消す必要があります。
{:important}

1. **「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. キャンセルする LUN の**「アクション」**をクリックし、**「{{site.data.keyword.blockstorageshort}}のキャンセル」**を選択します。
3. LUN を即時にキャンセルするか、LUN がプロビジョンされた支払い日にキャンセルするかを確認します。

   応当日に LUN をキャンセルするオプションを選択した場合は、応当日の前にキャンセル要求を無効にすることができます。
   {:tip}
4. **「続行」**または**「閉じる」**をクリックします。
5. **「確認応答」**チェック・ボックスをクリックし、**「確認」**をクリックします。

あるいは、次の SLCLI コマンドを使用できます。
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```
