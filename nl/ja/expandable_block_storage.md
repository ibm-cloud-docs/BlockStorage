---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# ブロック・ストレージ容量の拡張
{: #expandingcapacity}

この新機能を使用すると、{{site.data.keyword.blockstoragefull}} の現行ユーザーは、既存の {{site.data.keyword.blockstorageshort}} のサイズを GB 単位で増やして最大 12 TB まで即時に拡張できます。 複製ボリュームを作成する必要も、より大きなボリュームにデータを手動でマイグレーションする必要もありません。 サイズ変更の実行中に、ストレージへのアクセスが停止することも、ストレージへアクセスできなくなることもありません。

ボリュームに対する請求は自動更新されて、新価格の差額が日割り計算で現在の請求サイクルに追加されます。 その後、次の請求サイクルでは新しい金額全体が請求されます。

この機能は、[限定されたデータ・センター ](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)で使用可能です。

## 拡張可能なストレージの利点

- **コスト管理**: データが増加する可能性があることはわかっているが、開始時点で必要なのは小さいストレージでよい場合があります。 拡張する機能があれば、お客様はストレージのコストを節約し、必要に合わせて拡張することができます。  

- **ストレージ必要量の増大** - データが急速に増加しているお客様には、それに対応するために、ストレージのサイズを素早く簡単に増やす方法が必要です。

## 複製に対するストレージ容量拡張の影響

1 次ストレージに対する拡張アクションによって、レプリカのサイズ変更が自動的に行われます。

## 制限
{: #limitsofexpandingstorage}

この機能は、[限定されたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)にプロビジョンされたストレージに対して使用可能です。

この機能がリリースされる前の **2017 年 4 月から 2017 年 12 月 14 日**の間に、これらのデータ・センターでプロビジョンされたストレージは、元のサイズの 10 倍までしか増やすことができません。 **2017 年 12 月 14 日**より後にプロビジョンされたストレージは、最大 12 TB まで増やすことができます。

エンデュランスを指定してプロビジョンされた{{site.data.keyword.blockstorageshort}}に関する既存のサイズ制限 (10 IOPS ティアでは最大 4 TB、他のすべてのティアでは最大 12 TB) は、引き続き適用されます。

## ストレージのサイズ変更
{: #resizingsteps}

1. {{site.data.keyword.slportal}}で、**「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックするか、{{site.data.keyword.cloud}} コンソールから、**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. リストから LUN を選択し、**「アクション」** > **「LUN の変更 (Modify LUN)」**をクリックします。
3. 新しいストレージ・サイズを GB 単位で入力します。
4. 選択内容と新しい価格設定を確認します。
5. **「マスター・サービス契約を読み ... (I have read the Master Service Agreement ...)」**チェック・ボックスをクリックし、**「注文する (Place Order)」**をクリックします。
6. 新規ストレージ割り振りは数分後に使用可能になります。

あるいは、SLCLI を使用してボリュームをサイズ変更できます。

```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
{:codeblock}

ボリューム上のファイル・システム (さらに、ある場合はパーティション) を拡張して新しいスペースを使用する方法に関して詳しくは、ご使用の OS の資料を参照してください。
{:tip}
