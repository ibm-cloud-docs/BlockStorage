---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-17"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} の新しいロケーションと機能

{{site.data.keyword.BluSoftlayer_full}} では、{{site.data.keyword.blockstoragefull}} の新規バージョンが導入されています。

この新しいストレージは限られたデータ・センターで使用可能で、Data at Rest (保存されたデータ) に対してディスク・レベルで暗号化でき IOPS レベルがより高いフラッシュ・ストレージに基づいています。 アップグレードされたデータ・センターにプロビジョンされるストレージはすべて、新規バージョンで自動的に作成されます。

**注:** 新規ボリュームの NFS マウント・ポイントは、非暗号化ボリュームのマウント・ポイントとは異なります。詳しくは、**暗号化 {{site.data.keyword.filestorage_short}} ボリュームの新規マウント・ポイント**セクションを参照してください。

新しい {{site.data.keyword.blockstorageshort}} は、以下の地域/データ・センターにあります。
<table role="presentation">
	 <tr>
	   <td><strong>米国 2</strong></td>
	   <td><strong>EU</strong></td>
	   <td><strong>オーストラリア</strong></td>
	   <td><strong>カナダ</strong></td>
	   <td><strong>中南米</strong></td>
	   <td><strong>アジア太平洋</strong></td>
	</tr>
	<tr>
	   <td><p>SJC03<br />
		SJC04<br />
		WDC04<br />
		WDC06<br />
		WDC07<br />
		DAL09<br />
		DAL10<br />
		DAL12<br />
		DAL13<br /><br /><br /></p>
	   </td>
	   <td><p>LON02<br />
		LON04<br />
		LON06<br />
		FRA02<br />
		FRA04<br />
		FRA05<br />
		AMS01<br />
		AMS03<br />
		OSLO1<br />
		PAR01<br />
		MIL01</p>
            </td>
	    <td><p>SYD01<br />
		SYD04<br />
		MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>TOR01<br />
		MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>TOK02<br />
        TOK04<br />
        TOK05<br/>
	    	HKG02<br />
	    	SEO01<br />
	    	SNG01<br />
    		CHE01<br /><br /><br /><br /><br /></p>
	   </td>
	</tr>
</table>

*表 1 は、IBM のデータ・センターの可用性を示しています。 地域ごとに独自の列があります。 一部の都市 (ダラス、サンノゼ、ワシントン DC、アムステルダム、フランクフルト、ロンドン、シドニーなど) には複数のデータ・センターがあります。*

新しいストレージは以下の機能と性能を備えています。

- **[Data at Rest (保存されたデータ) に対するプロバイダー管理の暗号化](block-file-storage-encryption-rest.html)**。
  すべての {{site.data.keyword.blockstorageshort}} が、追加料金なしに、自動的に暗号化されてプロビジョンされます。
- **10 IOPS/GB ティア・オプション**。
  最も厳しいワークロードに対応するために、エンデュランス・タイプの {{site.data.keyword.blockstorageshort}} で新しいティアが使用可能になりました。
- **オール・フラッシュ・バック・ストレージ。**
  2 IOPS/GB 以上のエンデュランス・タイプまたはパフォーマンス・タイプのいずれかでプロビジョンされる {{site.data.keyword.blockstorageshort}} は、すべてオール・フラッシュ・ストレージによって支えられています。
- {{site.data.keyword.blockstorageshort}} の**スナップショットとレプリケーション**のサポート
- 予定使用期間が 1 カ月未満のストレージには**時間単位の請求**オプションを使用可能。
- パフォーマンスがプロビジョンされた {{site.data.keyword.blockstorageshort}} には、**最大 48,000 IOPS**。
- 季節的な負荷の変動時にパフォーマンスを改善するために **IOPS レートを調整可能**。 この機能について詳しくは、[ここ](adjustable-iops.html)を参照してください。
- **[{{site.data.keyword.blockstorageshort}} ボリューム複写機能](how-to-create-duplicate-volume.html)**を使用して、データのクローンを作成します。
- 複製を作成したり、より大きなボリュームに手動でデータを移動したりする必要なしに、GB 単位で最大 12 TB まで**ストレージを拡張可能**。 この機能について詳しくは、[ここ](expandable_block_storage.html)を参照してください。

## 暗号化ストレージ・ボリュームの新規マウント・ポイント

これらのデータ・センターにプロビジョンされる拡張ストレージ・ボリュームはすべて、マウント・ポイントが非暗号化ボリュームとは異なります。 ストレージ・ボリュームに対して正しいマウント・ポイントを使用していることを確認するには、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}の**「ボリュームの詳細」**ページでマウント・ポイント情報を表示します。また、API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して、正しいマウント・ポイントにアクセスすることもできます。

追加のデータ・センターがアップグレードされていないか確認したり、新しいフィーチャーや機能が {{site.data.keyword.blockstorageshort}} に追加されていないか確認したりするには、このページをもう一度参照してください。
