---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, snapshot space, ordering snapshots,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# スナップショットの注文
{: #orderingsnapshots}

ストレージ・ボリュームのスナップショットを作成するには、自動作成と手動作成のいずれでも、作成したスナップショットを保持するためのスペースを購入する必要があります。 容量は、(ボリュームを初めて購入するとき、またはここで説明するステップを使用して後からでも) ご使用のストレージ・ボリューム量まで購入できます。

## 注文するスナップショット・スペース量の決定

一般的に、スナップショット・スペースは、以下に示す 2 つの主要な要因に基づいて、スナップショットによって使用されます。
- アクティブ・ファイル・システムの一定時間にわたる変化量。
- スナップショットの予定保存期間。  

必要なスペースの量を計算する方法は、**(変更率)** x **(データを保存する時間/日/週/月の数)** です。

最初のスナップショットは、アクティブなファイル・システム・ブロックを示すメタデータ (ポインター) のコピーに過ぎないので、使用するスペースはごくわずかです。
{:note}

多数の変更が加えられ、保存期間が長いボリュームには、適度な変更と保持スケジュールがあるボリュームよりも多くのスペースが必要です。 最初のタイプの例は、変更率の高いデータベースです。 2 番目のタイプの例は、VMware データ・ストアです。

実際のデータが 500 GB あるボリュームの 1 時間ごとに取ったスナップショットが 12 個あり、各スナップショット間で 1% の変化がある場合、最終的に、スナップショット用に 60 GB 必要です。

*(5 GB 変更率) x (1 時間ごとのスナップショット 12 個) = (60 GB の使用スペース)*

一方、その 500 GB の実データで、1 時間ごとに取ったスナップショットが 12 個あり、1 時間ごとに 10% の変化がある場合、使用されるスナップショット・スペースは 600 GB になります。

*(50 GB 変更率) x (1 時間ごとのスナップショット 12 個) = (600 GB の使用スペース)*

そのため、必要なスナップショット・スペースの量を決定する際は、変更率を慎重に検討してください。 変更率は、必要なスナップショット・スペースの量に多大な影響を与えます。 ボリュームが大きくなるほど、頻繁に変更する可能性が高くなります。 ただし、500 GB のボリュームで変更が 5 GB の場合と 10 TB のボリュームで変更が 5 GB の場合では、同じ量のスナップショット・スペースが使用されます。

さらに、ほとんどのワークロードでは、ボリュームが大きいほど、最初に確保する必要のあるスペースが小さくなります。 これは主に、基になるデータの効率性と、環境でのスナップショットの仕組みに起因しています。

## {{site.data.keyword.cloud_notm}} コンソールを使用したスナップショット・スペースの注文

1. [{{site.data.keyword.cloud_notm}} コンソール](https://{DomainName}/catalog){: external}にログインし、左上の「メニュー」アイコンをクリックします。そして、**「クラシック・インフラストラクチャー」**を選択します。
2. **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**を使用して、ストレージ LUN にアクセスします。
2. 「スナップショット」フレームにある**「スナップショット・スペースの変更 (Change Snapshot Space)」**をクリックします。
3. 必要なスペースの量と支払方法を選択します。
4. **「続行」**をクリックします。
5. お持ちの**プロモーション・コード**を入力して、**「再計算」**をクリックします。 「この注文の課金」フィールドと「注文の検討」フィールドは、デフォルトで入力されています。

   注文の処理時に割引が適用されます。
   {:note}
6. **「マスター・サービス契約を読み、その契約条件に同意します」**ボックスにチェック・マークを付け、**「注文の実行」**をクリックします。 スナップショット・スペースは数分後にプロビジョンされます。

## SLCLI を使用したスナップショット・スペースの注文

```
# slcli block snapshot-order --help
Usage: slcli block snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the block
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
{:codeblock}
