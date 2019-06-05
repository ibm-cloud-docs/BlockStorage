---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage Encryption, industry standard protocols, IBM Block Storage, LUN, provider-managed encryption

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# プロバイダー管理の保存データの暗号化
{: #encryption}

## {{site.data.keyword.blockstorageshort}} の保存データの暗号化

{{site.data.keyword.cloud}} は、セキュリティーに対するニーズを深刻に受け止めるとともに、データを安全に維持するためにデータを暗号化できることの重要性を理解しています。 プロバイダー管理の暗号化では、エンデュランスまたはパフォーマンスのいずれかのオプションを指定してプロビジョンされた {{site.data.keyword.blockstoragefull}} はデフォルトで暗号化され、そのための追加のコストもパフォーマンスへの影響も発生しません。

プロバイダー管理の保存データの暗号化機能は、以下の業界標準プロトコルを使用します。

* 業界標準 AES-256 暗号化
* 鍵は、業界標準の Key Management Interoperability Protocol (KMIP) を使用して社内で管理されます。
* ストレージは、連邦情報処理標準 (FIPS) Publication 140-2、Federal Information Security Management Act (FISMA)、医療保険の積算と責任に関する法律 (HIPAA) に対応しています。 ストレージは、Payment Card Industry (PCI)、Basel II, California Security Breach Information Act (SB 1386)、および EU Data Protection Directive 95/46/EC コンプライアンスにも対応しています。

## スナップショットまたは複製されたストレージの保存データの暗号化の提供  

すべてのスナップショットおよび暗号化された {{site.data.keyword.blockstorageshort}} のレプリカも、デフォルトで暗号化されます。 この機能は、ボリューム単位でオフにすることはできません。

## 暗号化機能を備えたストレージのプロビジョン

プロバイダー管理の保存中の暗号化機能は、[限定されたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)でプロビジョンされる {{site.data.keyword.blockstorageshort}} で使用可能です。 それらのデータ・センターで注文されたすべてのストレージは、自動的に暗号化されてプロビジョンされます。

{{site.data.keyword.blockstorageshort}} を注文するときに、アスタリスク (`*`) で示されたデータ・センターを選択します。 「LUN/ボリューム名」フィールドの右に、ボリュームが暗号化されていることを示すロック・アイコンが表示されます。

![LUN が暗号化されていることを示すロック・アイコン](/images/encryptedstorage.png)
<caption>図 1. ボリュームが暗号化されていることを示すロック・アイコンの例。</caption>



データ・センターがアップグレードされる前にプロビジョンされた非暗号化ストレージは、自動的には**暗号化されません**。 アップグレードされたデータ・センターに暗号化されていないストレージを所有していて、暗号化ストレージが必要な場合は、新しいボリュームを作成し、データをマイグレーションする必要があります。 詳しくは、[{{site.data.keyword.blockstorageshort}} アップグレード済みのデータ・センターでのマイグレーション](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage)を参照してください。
{:important}
