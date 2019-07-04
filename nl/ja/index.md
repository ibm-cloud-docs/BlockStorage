---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# {{site.data.keyword.blockstorageshort}} 製品情報
{: #About}

{{site.data.keyword.blockstoragefull}} は、コンピューティング・インスタンスから独立してプロビジョンおよび管理される、永続的で高性能な iSCSI ストレージです。 iSCSI ベースの {{site.data.keyword.blockstorageshort}} LUN は、冗長マルチパス入出力 (MPIO) 接続を介して、許可されたデバイスに接続されます。

{{site.data.keyword.blockstorageshort}} は、他に類のない機能によりクラス最高レベルの耐久性および可用性をもたらします。 この製品は、業界標準とベスト・プラクティスを使用して構築されています。 {{site.data.keyword.blockstorageshort}} は、保守イベントや計画外の障害においてもデータの保全性を保護し、可用性を維持し、一貫性のあるパフォーマンス・ベースラインを提供するように設計されています。

## コア機能
{: #corefeatures}

{{site.data.keyword.blockstorageshort}} では以下の機能を利用できます。

- **一貫性のあるパフォーマンス・ベースライン**
   - 個々のボリュームに対するプロトコル・レベルの IOPS の割り振りによって提供されます。
- **強力な耐久性と回復力**
   - データの保全性を保護し、保守イベントおよび計画外の障害の際に可用性を維持します。そのため、独立ディスク (RAID) アレイを使用してオペレーティング・システム・レベルの冗長アレイを作成および管理する必要はありません。
- **Data at Rest (保存されたデータ) の暗号化** ([ほとんどのデータ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Data at Rest (保存されたデータ) のためのプロバイダー管理の暗号化を追加コストなしで利用できます。
- **オール・フラッシュ・バックアップ・ストレージ** ([ほとんどのデータ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - 2 IOPS/GB 以上のレベルの「エンデュランス」または「パフォーマンス」でプロビジョンされるボリューム用のオール・フラッシュ・ストレージ。
- **スナップショット** ([ほとんどのデータ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - 処理を中断せずに、特定時点のデータ・スナップショットをキャプチャーします。
- **レプリケーション** ([ほとんどのデータ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - パートナーの {{site.data.keyword.cloud}} データ・センターにスナップショットを自動的にコピーします。
- **可用性の高い接続**
   - 冗長ネットワーキング接続を使用して可用性を最大化します。
   - iSCSI ベースの {{site.data.keyword.blockstorageshort}} では、マルチパス入出力 (MPIO) を使用します。
- **同時アクセス**
   - クラスター構成の場合、複数のホストがブロック・ボリューム (最大 8 個) に同時にアクセスできます。
- **クラスター・データベース**
   - クラスター・データベースなどの高度なユース・ケースをサポートします。


## プロビジョニング
{: #provisioning}

{{site.data.keyword.blockstorageshort}} LUN は、20 GB から 12 TB までプロビジョンできます。これには次の 2 つのオプションがあります。 <br/>
- 事前定義されたパフォーマンス・レベル、およびスナップショットやレプリケーションなどの機能を備えた**エンデュランス**層をプロビジョンする。
- 割り振り済みの 1 秒当たりの入出力操作 (IOPS) を使用して高出力の**パフォーマンス**環境を構築する。


### エンデュランス層を持つプロビジョニング
{: #provendurance}

「エンデュランス」{{site.data.keyword.blockstorageshort}} は、さまざまなアプリケーションのニーズをサポートするよう 4 つの IOPS パフォーマンス層で使用できます。 <br />

- **0.25 IOPS/GB** は、入出力負荷が低いワークロード用に設計されています。 通常、これらのワークロードは、どの時点においても非アクティブになっているデータの割合が高いという特徴があります。 アプリケーションの例として、メールボックスの保管や部門レベルのファイル共有などが挙げられます。

- **2 IOPS/GB** は、大部分の一般的な用途のために設計されています。 アプリケーションの例として、Web アプリケーションをバッキングする小規模なデータベースのホスティングや、ハイパーバイザーの VM ディスク・イメージが挙げられます。

- **4 IOPS/GB** は、高負荷ワークロード用に設計されています。 通常、これらのワークロードには、いつも大部分のデータがアクティブであるという特徴があります。 アプリケーションの例として、トランザクション・データベースやその他の高いパフォーマンスを必要とするデータベースが挙げられます。

- **10 IOPS/GB** は、NoSQL データベースや Analytics のデータ処理などで作成される最も厳しいワークロード用に設計されています。 この層は、[ほとんどのデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC)でプロビジョンされる最大 4 TB のストレージに対して使用できます。

12 TB のエンデュランス・ボリュームでは、最大 48,000 IOPS が使用可能です。

ワークロードに見合った適切なエンデュランス層を選択することが重要です。 それと同じくらいに、最高のパフォーマンスを達成するために適切なブロック・サイズ、イーサネット接続速度、およびホストの数を使用することも重要です。 これらの部分のいずれかが他の部分とうまく調整が取れていない場合、結果のスループットに大きな影響を与える可能性があります。


### 「パフォーマンス」でのプロビジョニング
{: #provperformance}

「パフォーマンス」は、エンデュランス層にうまく適合しないがパフォーマンス要件は解明されている高入出力アプリケーションをサポートするように設計された {{site.data.keyword.blockstorageshort}} のクラスです。 予測可能なパフォーマンスは、個々のボリュームに対するプロトコル・レベル IOPS の割り振りによって達成されます。 100 から 48,000 の範囲のさまざまな IOPS レートを、20 GB から 12 TB までの範囲のストレージ・サイズでプロビジョンできます。

{{site.data.keyword.blockstorageshort}} の「パフォーマンス」は、マルチパス入出力 (MPIO) internet Small Computer System Interface (iSCSI) 接続を介してアクセスおよびマウントされます。 {{site.data.keyword.blockstorageshort}}は、通常、単一のサーバーからボリュームにアクセスする場合に使用されます。 複数のボリュームを 1 つのホストにマウントし、まとめてストライピングすることで、ボリュームを大きくし、IOPS 数を増やすことができます。 表 3 のサイズと IOPS レートに従って、Linux、XEN、および Windows の各オペレーティング・システム用のパフォーマンス・ボリュームを注文できます。

| サイズ (GB) | 最小 IOPS | 最大 IOPS
|-----|-----|-----|
| 20 | 100 | 1,000 |
| 40 | 100 | 2,000  |
| 80 | 100 | 4,000 |
| 100 | 100 | 6,000 |
| 250 | 100 | 6,000 |
| 500 | 100  | 6,000 または 10,000 |
| 1,000 | 100 | 6,000 または 20,000 ![脚注](/images/numberone.png) |
| 2,000 | 200 | 6,000 または 40,000 ![脚注](/images/numberone.png) |
| 3,000 から 7,000 | 300 | 6,000 または 48,000 ![脚注](/images/numberone.png) |
| 8,000 から 9,000 | 500 | 6,000 または 48,000 ![脚注](/images/numberone.png) |
| 10,000 から 12,000 | 1,000 | 6,000 または 48,000 ![脚注](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="表による比較" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![脚注](/images/numberone.png) *ほとんどのデータ・センターで、6,000 を超える IOPS 制限を使用できます。*

パフォーマンス・ボリュームは、プロビジョンされた IOPS レベルに一貫して近いレベルで動作するように設計されています。 一貫性により、特定レベルのパフォーマンスを持つアプリケーション環境のサイズ変更やスケーリングが容易になります。 さらに、理想的な価格対パフォーマンスの比率でボリュームを構築することによって、環境を最適化することもできます。

## 請求
{: #billing}

ブロック LUN に対して、1 時間ごとまたは月ごとの請求処理を選択できます。 LUN に対して選択した請求のタイプは、その LUN のスナップショット・スペースおよびレプリカに適用されます。 例えば、毎時請求を指定して LUN をプロビジョンすると、スナップショット料金またはレプリカ料金は時間単位で請求されます。 月次請求を指定して LUN をプロビジョンすると、スナップショット料金またはレプリカ料金は月単位で請求されます。

 * **毎時請求**では、ブロック LUN がアカウント上に存在していた時間数は、LUN が削除された時点、または請求サイクルの終了時の、どちらか早い方の時点で計算されます。 使用期間が数日ないし 1 カ月未満のストレージには毎時請求が適しています。 毎時請求は、[ほとんどのデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC)で使用できます。

 * **月次請求**では、作成日から請求サイクルの終了までの料金が日割りで計算され、 即時に請求されます。 請求サイクルの終了前に LUN が削除された場合、返金はありません。 月次請求は、長期間 (1 カ月以上) の保管およびアクセスが必要なデータを利用する実動ワークロードに使用するストレージに適しています。

### エンデュランス
{: #pricing-comparison-endurance}

| 定義済み IOPS ティアの価格オプション | 0.25 IOPS | 2 IOPS/GB | 4 IOPS/GB | 10 IOPS/GB |
|-----|-----|-----|-----|-----|
| 月次価格 | $0.06/GB | $0.15/GB | $0.20/GB | $0.58/GB |
| 毎時価格 | $0.0001/GB | $0.0002/GB | $0.0003/GB | $0.0009/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="表による比較" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### パフォーマンス
{: #pricing-comparison-performance}

| カスタム IOPS ティアの価格オプション | 価格計算 |
|-----|-----|
| 月次価格 | $0.10/GB + $0.07/IOP |
| 毎時価格 | $0.0001/GB + $0.0002/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="表による比較" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
