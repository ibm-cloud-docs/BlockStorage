---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: Block Storage, new features, new locations, Block Storage, mount point changes, select data centers, ISCSI,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 新しい場所および新機能
{: #news}

{{site.data.keyword.cloud}} では、{{site.data.keyword.blockstoragefull}} の新規バージョンが導入されています。

この新しいストレージは限られたデータ・センターで使用可能で、Data at Rest (保存されたデータ) に対してディスク・レベルで暗号化でき IOPS レベルがより高いフラッシュ・ストレージに基づいています。 アップグレードされたデータ・センターにプロビジョンされるストレージはすべて、新規バージョンで自動的に作成されます。

新規ボリュームの NFS マウント・ポイントは、非暗号化ボリュームのマウント・ポイントとは異なります。 詳しくは、[暗号化 {{site.data.keyword.blockstorageshort}} ボリュームの新規マウント・ポイント](#mountpoints)セクションを参照してください。
{:important}

## 新しい場所
{: #new-locations}

新しい {{site.data.keyword.blockstorageshort}} は、以下の地域およびデータ・センターにあります。

|米国 2|ラテンアメリカ|カナダ|EU|アジア太平洋|オーストラリア|
|-----|-----|-----|-----|-----|------|
| DAL09<br >DAL10<br />DAL12<br />DAL13<br />SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07 | MEX01<br />SAO01 | MON01<br />TOR01  | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />MIL01<br />OSLO1<br />PAR01 | CHE01<br />HKG02<br />SEO01<br />SNG01<br />TOK02<br />TOK04<br />TOK05 | MEL01<br />SYD01<br />SYD04<br />SYD05 |
{: caption="表 1 は、データ・センターの可用性を示しています。 地域ごとに独自の列があります。 一部の都市 (ダラス、サンノゼ、ワシントン DC、アムステルダム、フランクフルト、ロンドン、シドニーなど) には複数のデータ・センターがあります。" caption-side="top"}


## 新機能および能力
{: #features}

- **[Data at Rest (保存されたデータ) に対するプロバイダー管理の暗号化](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)**。
  すべての {{site.data.keyword.blockstorageshort}} が、追加料金なしに、自動的に暗号化されてプロビジョンされます。
- **10 IOPS/GB ティア・オプション**。
  最も厳しいワークロードに対応するために、エンデュランス・タイプの {{site.data.keyword.blockstorageshort}} で新しいティアが使用可能になりました。
- **オール・フラッシュ・バック・ストレージ。**
  2 IOPS/GB 以上のエンデュランス・タイプまたはパフォーマンス・タイプのいずれかでプロビジョンされる {{site.data.keyword.blockstorageshort}} は、すべてオール・フラッシュ・ストレージによって支えられています。
- {{site.data.keyword.blockstorageshort}} の**スナップショットとレプリケーション**のサポート
- 予定使用期間が 1 カ月未満のストレージには**時間単位の請求**オプションを使用可能。
- パフォーマンスがプロビジョンされた {{site.data.keyword.blockstorageshort}} には、**最大 48,000 IOPS**。
- 季節的な負荷の変動時にパフォーマンスを改善するために **IOPS レートを調整可能**。 この機能について詳しくは、[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS)を参照してください。
- **[{{site.data.keyword.blockstorageshort}} ボリューム複写機能](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)**を使用して、データのクローンを作成します。
- 複製を作成したり、より大きなボリュームに手動でデータを移動したりする必要なしに、GB 単位で最大 12 TB まで**ストレージを拡張可能**。 この機能について詳しくは、[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity)を参照してください。

## 暗号化ストレージ・ボリュームの新規マウント・ポイント
{: #mountpoints}

これらのデータ・センターにプロビジョンされる拡張ストレージ・ボリュームはすべて、マウント・ポイントが非暗号化ボリュームとは異なります。 [{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic/storage/block){: external}の**「ボリュームの詳細」**ページでマウント・ポイント情報を確認して、正しいマウント・ポイントを使用していることを確認してください。API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して正しいマウント・ポイント情報を取得することもできます。

すべての新規機能を利用できるようにするには、API を使用して発注する場合に「`Storage-as-a-Service Package 759`」を選択してください。 API を使用した {{site.data.keyword.blockstorageshort}} の注文について詳しくは、[order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external} を参照してください。
{:important}

追加のデータ・センターがアップグレードされていないか確認したり、新しいフィーチャーや機能が {{site.data.keyword.blockstorageshort}} に追加されていないか確認したりするには、このページをもう一度参照してください。
{:tip}
