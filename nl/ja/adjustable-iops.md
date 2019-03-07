---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IOPS の調整
{: #adjustingIOPS}

この新機能により、{{site.data.keyword.blockstoragefull}} ストレージ・ユーザーは、既存の {{site.data.keyword.blockstorageshort}} の IOPS を即時に調整できます。 複製を作成したり、手動で新しいストレージにデータをコピーしたりする必要はありません。 調整が行われている間も、ストレージへのアクセスが停止したり、ストレージにアクセスできなくなったりといったことをユーザーが経験することはありません。

ストレージに対する請求は更新され、新しい金額との差は按分されて現在の請求サイクルに追加されます。 次の請求サイクルでは新しい金額全体が請求されます。


## 調整可能な IOPS の利点

- コスト管理 – 一部のお客様は、ピーク使用時にのみ高い IOPS を必要とする可能性があります。 例えば、大規模な小売店のピーク使用は年末のセール期間であり、その後、ストレージに高い IOPS レートが必要になる場合があります。 しかし、真夏に高い IOPS は必要ありません。 この機能により、そのような小売店はコストを管理し、必要なときに、高い IOPS のためにコストを払うことができます。

## 制限
{: #limitsofIOPSadjustment}

この機能は、[限定されたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)でのみ使用可能です。

お客様は、IOPS を調整するときに、エンデュランスとパフォーマンスを切り替えることはできません。 ただし、以下の基準と制限に基づいて、ストレージの新しい IOPS 層または IOPS レベルを指定できます。

- 元のボリュームがエンデュランス 0.25 層の場合、IOPS 層は更新できません。
- 元のボリュームが 0.30 IOPS/GB 以下の「パフォーマンス」である場合、使用可能なオプションには、0.30 IOPS/GB 以下になるサイズと IOPS の組み合わせのみが含まれます。
- 元のボリュームが 0.30 IOPS/GB を超える「パフォーマンス」である場合、使用可能なオプションには、0.30 IOPS/GB を超えるサイズと IOPS の組み合わせのみが含まれます。

## 複製に対する IOPS 調整の影響

ボリュームに複製が設定されている場合、1 次の IOPS の選択に一致するようにレプリカが自動的に更新されます。

## ストレージの IOPS の調整
{: #adjustingsteps}

1. {{site.data.keyword.blockstorageshort}}のリストに進みます。
   - {{site.data.keyword.slportal}}で、**「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックします。
   - {{site.data.keyword.BluSoftlayer_full}} コンソールから、**「インフラストラクチャー」** > **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. リストから LUN を選択し、**「アクション」** > **「LUN の変更 (Modify LUN)」**をクリックします。
3. **「ストレージ IOPS オプション (Storage IOPS Options)」**の下で、新しい選択を行います。
    - エンデュランス (層化 IOPS) の場合、0.25 IOPS/GB より大きい IOPS 層をストレージに選択します。 IOPS 層はいつでも増やすことができます。 ただし、下げることができるのは、月に 1 回のみです。
    - パフォーマンス (割り振り IOPS) の場合、100 から 48,000 IOPS までの値を入力して、ストレージの新しい IOPS オプションを指定します。

    注文フォームで、サイズ別に必要な特定の境界を必ず確認してください。
    {:tip}
4. 選択内容と新しい価格設定を確認します。
5. **「マスター・サービス契約を読み ... (I have read the Master Service Agreement ...)」**チェック・ボックスをクリックし、**「注文する (Place Order)」**をクリックします。
6. 新規ストレージ割り振りは数分後に使用可能になります。


あるいは、SLCLI を使用して IOPS を調整できます。
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
