---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} 與 {{site.data.keyword.filestorage_short}} 的新位置及特性

{{site.data.keyword.BluSoftlayer_full}} 將引進新版的 {{site.data.keyword.blockstoragefull}}！ 

精選資料中心內會提供新的儲存空間，並且由更高 IOPS 層次的快閃記憶體儲存空間支援，且具有靜態資料的磁碟層次加密。精選資料中心內佈建的所有儲存空間都會自動佈建新版本的 {{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_full}}。

**附註：**新磁區的 NFS 裝載點已變更。如需詳細資料，請參閱下面的**加密 {{site.data.keyword.filestorage_short}} 磁區的新裝載點**。

下列地區/資料中心目前已提供新的 {{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_short}}，其他資料中心即將會推出！
<table style="width:100%;">
	<caption>資料中心可用性</caption>
	<tbody>
		<tr>
			<td><strong>美國 2</strong></td>
			<td><strong>歐盟</strong></td>
			<td><strong>澳大利亞</strong></td>
			<td><strong>加拿大</strong></td>
			<td><strong>拉丁美洲</strong></td>
			<td><strong>亞太</strong></td>
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
					DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
					MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
			        SEO01<br />
				SNG01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


新的儲存空間具有下列特性及功能：

- [靜態資料的提供者管理加密](block-file-storage-encryption-rest.html)。所有 {{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_short}} 都會自動佈建為已加密，不需額外付費。
- 每 GB 10 IOPS 層級選項。「耐久性」類型 {{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_short}} 已新增層級，可支援最嚴苛的工作負載。
- 全快閃記憶體支援的儲存空間。{{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_short}} 已佈建每 GB 2 IOPS 或以上的「耐久性」或「效能」，並由全快閃記憶體儲存空間支援。
- 已佈建「耐久性」或「效能」的 {{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_short}} 的 Snapshot 和抄寫支援。
- 針對計劃使用期間不到一整個月的儲存空間，新增了「每小時計費」選項。 
- 已佈建「效能」的 {{site.data.keyword.blockstorageshort}} 及 {{site.data.keyword.filestorage_short}} 最多有 48,000 IOPS。
- 可以調整 IOPS 速率，以改善季節性負載變更的效能。請在[這裡](adjustable-iops.html)深入閱讀此特性。
- 使用 [{{site.data.keyword.blockstorageshort}} 磁區複製特性](how-to-create-duplicate-volume.html)，以建立您資料的新複製品。
- 可以即時擴充儲存空間（以 GB 為增量單位，最多可到 12 TB），而不需要建立重複項目，或將資料手動移轉至較大的磁區。請在[這裡](expandable_block_storage.html)深入閱讀此特性。

## 加密儲存空間磁區的新裝載點

這些資料中心內佈建的所有加密儲存空間磁區都具有與未加密磁區不同的裝載點。為了確保加密及未加密儲存空間磁區都使用正確的裝載點，您可以在使用者介面的「磁區詳細資料」頁面中檢視裝載點資訊，並且透過 API 呼叫 `SoftLayer_Network_Storage::getNetworkMountAddress()` 來存取正確的裝載點。

請在這裡再次確認，以查看何時已升級其他資料中心以及要針對 {{site.data.keyword.blockstorageshort}} 新增的新可用特性及功能。
