---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}}の管理
{: #managingstorage}

{{site.data.keyword.blockstoragefull}} のボリュームは、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}で管理できます。 **「メニュー」**から、クラシック・サービスとのやり取りのための**「クラシック・インフラストラクチャー」**を選択します。

## {{site.data.keyword.blockstorageshort}} LUN の詳細の表示

ストレージに追加されたスナップショットとレプリケーションの機能など、選択したストレージ LUN に関する主要情報の要約を表示できます。

1. **「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. リストから適切なボリューム名をクリックします。

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

2. **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックします。
3. ボリュームを見つけて、**「...」**をクリックします。
4. **「ホストの許可」**をクリックします。
5. 使用可能なデバイスまたは IP アドレスのリストを表示するには、最初に、デバイス・タイプまたはサブネットのどちらに基づいてアクセス権限を許可するかを選択します。
   - 「デバイス」を選択した場合、「ベアメタル・サーバー」または「Virtual Server インスタンス」から選択できます。
   - 「IP アドレス」を選択した場合、最初に、ホストがあるサブネットを選択します。
6. フィルターされたリストから、ボリュームにアクセスできる 1 つ以上のホストを選択して、**「保存」**をクリックします。

あるいは、次の SLCLI コマンドを使用できます。
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

## {{site.data.keyword.blockstorageshort}} LUN へのアクセスを許可されたホストのリストの表示

1. **「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックし、使用するボリューム名をクリックします。
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

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}で、**「デバイス」** -> **「デバイス・リスト」**をクリックし、目的のデバイスをクリックします。
2. **「ストレージ」**タブを選択します。

この特定のホストがアクセス権を持っているストレージ LUN のリストが表示されます。 リストはストレージ・タイプ (ブロック、ファイル、その他) ごとにグループ化されています。 **「アクション」**をクリックすると、追加のストレージを許可したり、アクセス権を削除したりできます。

OS タイプの異なる複数の LUN に同時にアクセスすることをホストに許可できません。 単一の OS タイプの LUN へのアクセスのみをホストに許可できます。 OS タイプの異なる複数の LUN へのアクセスを許可しようとすると、操作がエラーとなります。
{:note}

## {{site.data.keyword.blockstorageshort}} のマウントとアンマウント

ホストのオペレーティング・システムに基づいて、該当する手順に従います。

- [Linux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [cPanel を使用したバックアップ用の Block Storage の構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Plesk を使用したバックアップ用の Block Storage の構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## {{site.data.keyword.blockstorageshort}}に対するホストのアクセス権の取り消し

ホストから特定のストレージ LUN へのアクセスを停止する場合は、アクセス権を取り消すことができます。 アクセス権を取り消すと、ホスト接続が LUN からドロップされます。 そのホスト上のオペレーティング・システムとアプリケーションは、LUN と通信できなくなります。

ホスト側での問題を回避するには、アクセス権を取り消す前に、ストレージ LUN をオペレーティング・システムからアンマウントして、ドライブの欠落やデータ破損の発生を防止してください。
{:important}

**「デバイス・リスト」**または**「ストレージ」**ビューからアクセス権を取り消すことができます。

### デバイス・リストからのアクセス権の取り消し

1. [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}から、「クラシック・インフラストラクチャー」アイコンをクリックします。その後、**「デバイス」**、**「デバイス・リスト」**をクリックし、目的のデバイスをダブルクリックします。
2. **「ストレージ」**タブを選択します。
3. この特定のホストがアクセス権を持っているストレージ LUN のリストが表示されます。 リストはストレージ・タイプ (ブロック、ファイル、その他) ごとにグループ化されています。 ボリューム名の横にある**「アクション」**をクリックし、**「アクセス権の取り消し」**をクリックします。
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
  -h, --hardware-id TEXT    The ID of a hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of a virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of an IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  --help                    Show this message and exit.
```

## ストレージ LUN のキャンセル

特定の LUN が不要になった場合は、その LUN をいつでもキャンセルできます。

ストレージ LUN をキャンセルするには、最初にすべてのホストのアクセス権を取り消す必要があります。
{:important}

1. **「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. キャンセルするボリュームを選択し、**「アクション」**をクリックして、**「{{site.data.keyword.blockstorageshort}} のキャンセル」**を選択します。
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

LUN は、少なくとも 24 時間 (即時キャンセルの場合)、または支払い日まで、ストレージ・リストにそのまま表示されます。 特定の機能が使用できなくなりますが、ボリュームは再利用処理が施されるまで引き続き表示されます。 ただし、「削除」/「キャンセル」をクリックした直後に課金は停止されます。

アクティブなレプリカがあると、ストレージ・ボリュームの再利用処理がブロックされます。 ボリュームがマウントされていないこと、ホストの許可が取り消されていること、レプリケーションがキャンセルされていることを確認した後で、元のボリュームのキャンセルを試みてください。
