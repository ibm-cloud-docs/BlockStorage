---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# データの保護 - プロバイダー管理の保存データの暗号化

## {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_full_notm}} の保存データの暗号化 

{{site.data.keyword.BluSoftlayer_full}} は、セキュリティーに対するニーズを深刻に受け止めるとともに、データを安全に維持するためにデータを暗号化できることの重要性を理解しています。プロバイダー管理の暗号化では、エンデュランスまたはパフォーマンスのいずれかとともにプロビジョンされた {{site.data.keyword.blockstoragefull}} および {{site.data.keyword.filestorage_full}} はデフォルトで暗号化され、そのための追加のコストもパフォーマンスへの影響も発生しません。

プロバイダー管理の保存データの暗号化機能は、以下の業界標準プロトコルを使用します。

* 業界標準 AES-256 暗号化
* 鍵は、業界標準の Key Management Improbability Protocol (KMIP) を使用して社内で管理されます。
* ストレージは、Federal Information Security Management Act (FISMA)、Health Insurance Portability and Accountability Act (HIPAA)、Payment Card Industry (PCI)、Basel II, California Security Breach Information Act (SB 1386)、および EU Data Protection Directive 95/46/EC コンプライアンスに関して連邦情報処理標準 (FIPS) Publication 140-2 認定です。

## スナップショットまたは複製されたストレージの保存データの暗号化  

すべてのスナップショットおよび暗号化された {{site.data.keyword.blockstorageshort}} のレプリカも、デフォルトで暗号化されます。この機能は、ボリューム単位でオフにすることはできません。

## 暗号化機能を備えたストレージのプロビジョン

プロバイダー管理の保存データの暗号化機能は、限定されたデータ・センターでプロビジョンされる {{site.data.keyword.blockstorageshort}} でのみ使用可能です。この機能を使用できる新しいデータ・センターは定期的に追加されています。これらのデータ・センターでプロビジョンされるストレージはすべて、Data at Rest (保存されたデータ) の暗号化機能とともに自動的にプロビジョンされます。{{site.data.keyword.blockstorageshort}} の Data at Rest の暗号化を使用可能なデータ・センターの現行リストを確認するには、[ここ](new-ibm-block-and-file-storage-location-and-features.html)をクリックしてください。

{{site.data.keyword.blockstorageshort}} を注文するとき、「*」および、暗号化が使用可能であることを示すメッセージが記されたデータ・センターを選択します。LUN/ボリューム名フィールドの右側に、暗号化されていることを示すロック・アイコンが表示されます。図 1 を参照してください。

![LUN が暗号化されていることを示すロック・アイコン](/images/encryptedstorage.png)
<caption>図 1. LUN が暗号化されていることを示すロック・アイコンの例。</caption>



**注**: データ・センターのアップグレード前にプロビジョンされた非暗号化ストレージは、自動的には**暗号化されません**。アップグレードされたデータ・センター内に非暗号化ストレージがある場合は、新しい LUN またはボリュームを作成し、データ・マイグレーションを実行する必要があります。以下の記事にそのガイダンスがあります。

* [アップグレードされたデータ・センターでの {{site.data.keyword.blockstorageshort}} のマイグレーション](migrate-block-storage-encrypted-block-storage.html)
