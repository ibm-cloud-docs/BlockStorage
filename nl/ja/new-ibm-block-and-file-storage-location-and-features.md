---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-25"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} の新しいロケーションと機能

{{site.data.keyword.BluSoftlayer_full}} では、{{site.data.keyword.blockstoragefull}} の新規バージョンが導入されています。 

この新しいストレージは限られたデータ・センターで使用可能で、Data at Rest (保存されたデータ) に対してディスク・レベルで暗号化でき IOPS レベルがより高いフラッシュ・ストレージに基づいています。  この限られたデータ・センターにプロビジョンされているすべてのストレージには、自動的に {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_full}} の新規バージョンがプロビジョンされます。

**注:** 新規ボリュームの NFS マウント・ポイントが変更されています。 詳しくは、以下の**『暗号化 {{site.data.keyword.filestorage_short}} ボリュームの新規マウント・ポイント』**を参照してください。

新しい {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} が現時点で使用可能なのは、以下の地域/データ・センターですが、他のデータ・センターでもまもなく使用可能になります。
<table style="width:100%;">
	<caption>使用可能なデータ・センター</caption>
	<tbody>
		<tr>
			<td><strong>米国 2</strong></td>
			<td><strong>EU</strong></td>
			<td><strong>オーストラリア</strong></td>
			<td><strong>カナダ</strong></td>
			<td><strong>中南米</strong></td>
			<td><strong>アジア太平洋</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br /></p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				FRA04<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01</p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
			        SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


新しいストレージは以下の機能と性能を備えています。

- [Data at Rest (保存されたデータ) に対するプロバイダー管理の暗号化](block-file-storage-encryption-rest.html)。 {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} はすべて、自動的に追加料金なしで暗号化されてプロビジョンされます。
- 10 IOPS/GB ティア・オプション。 最も厳しいワークロードを支援するため、エンデュランス・タイプの {{site.data.keyword.blockstorageshort}} と {{site.data.keyword.filestorage_short}} に新しいティアが追加されました。
- オール・フラッシュ・バック・ストレージ。 エンデュランスまたはパフォーマンスのいずれかでプロビジョンされた {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} は、2 IOPS/GB 以上で、オール・フラッシュ・ストレージに基づいています。
- エンデュランスまたはパフォーマンスのいずれかでプロビジョンされた {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} での、スナップショットと複製のサポート。
- 予定使用期間が 1 カ月未満のストレージ用に毎時請求オプションを追加。 
- パフォーマンスがプロビジョンされた {{site.data.keyword.blockstorageshort}} および {{site.data.keyword.filestorage_short}} には、最大 48,000 IOPS。
- IOPS 速度は、季節による負荷変動に備えて性能を改善するため調整可能です。 この機能について詳しくは、[ここ](adjustable-iops.html)を参照してください。
- [{{site.data.keyword.blockstorageshort}} ボリューム複写機能](how-to-create-duplicate-volume.html)を使用して、データの新規クローンを作成します。
- ストレージは運用中も拡張可能で、最大 12 TB まで GB 単位で増やせます。データの複製もより大きなボリュームへの手動によるマイグレーションも不要です。 この機能について詳しくは、[ここ](expandable_block_storage.html)を参照してください。

## 暗号化ストレージ・ボリュームの新規マウント・ポイント

このようなデータ・センターにプロビジョンされている暗号化ストレージ・ボリュームはすべて、マウント・ポイントが非暗号化ボリュームとは異なります。 暗号化と非暗号化のいずれのストレージ・ボリュームでも正しいマウント・ポイントを使用していることを保証するため、UI の「ボリュームの詳細 (Volume Details)」ページでマウント・ポイント情報を調べることができます。また、次の API を呼び出して、正しいマウント・ポイントにアクセスすることもできます。`SoftLayer_Network_Storage::getNetworkMountAddress()`

追加のデータ・センターがアップグレードされた時期および {{site.data.keyword.blockstorageshort}} 用に追加される新しく使用可能な機能と性能について確認するには、再度ここで調べてください。
