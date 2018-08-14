---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} 的新位置及特性

{{site.data.keyword.BluSoftlayer_full}} 將引進新版的 {{site.data.keyword.blockstoragefull}}！

精選資料中心內會提供新的儲存空間，並且由更高 IOPS 層次的快閃記憶體儲存空間支援，且具有靜態資料的磁碟層次加密。已升級資料中心內佈建的所有儲存空間，都會自動使用新版本建立。

**附註：**新磁區的 NFS 裝載點與未加密磁區的裝載點不同。如需詳細資料，請參閱**加密 {{site.data.keyword.filestorage_short}} 磁區的新裝載點**一節。

新的 {{site.data.keyword.blockstorageshort}} 提供於下列地區/資料中心。
<table role="presentation">
	 <tr>
	   <td><strong>美國 2</strong></td>
	   <td><strong>歐盟</strong></td>
	   <td><strong>澳大利亞</strong></td>
	   <td><strong>加拿大</strong></td>
	   <td><strong>拉丁美洲</strong></td>
	   <td><strong>亞太</strong></td>
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
		HKG02<br />
		SEO01<br />
		SNG01<br />
		CHE01<br /><br /><br /><br /><br /><br /><br /></p>
	   </td>
	</tr>
</table>

*表 1 顯示資料中心可用性。每一個地區都有自己的直欄。有些城市（例如「達拉斯」、「聖荷西」、「華盛頓特區」、「阿姆斯特丹」、「法蘭克福」、「倫敦」及「雪梨」）會有多個資料中心。*

新的儲存空間具有下列特性及功能：

- **[靜態資料的提供者管理加密](block-file-storage-encryption-rest.html)**。
  所有 {{site.data.keyword.blockstorageshort}} 都會自動佈建為已加密，不需額外付費。
- **每 GB 10 IOPS 層級選項**。
  「耐久性」類型 {{site.data.keyword.blockstorageshort}} 提供新的層級，可支援最嚴苛的工作負載。
- **全快閃記憶體支援的儲存空間。**
  所有 {{site.data.keyword.blockstorageshort}} 都已佈建每 GB 2 IOPS 或以上的「耐久性」或「效能」類型，並由全快閃記憶體儲存空間支援。
- {{site.data.keyword.blockstorageshort}} 的 **Snapshot 及抄寫**支援
- **按小時計費**選項適用於計劃使用期間不到一整個月的儲存空間。
- 已佈建「效能」的 {{site.data.keyword.blockstorageshort}} **最多有 48,000 IOPS**。
- **可以調整 IOPS 速率**，以改善季節性負載變更期間的效能。請在[這裡](adjustable-iops.html)深入閱讀此特性。
- 使用 **[{{site.data.keyword.blockstorageshort}} 磁區複製特性](how-to-create-duplicate-volume.html)**，以建立您資料的複製品。
- **可以擴充儲存空間**（以 GB 為增量單位，最多可到 12 TB），而不需要建立重複項目，或將資料手動移動至較大的磁區。請在[這裡](expandable_block_storage.html)深入閱讀此特性。

## 已加密儲存空間磁區的新裝載點

這些資料中心內佈建的所有加強型儲存空間磁區都具有與未加密磁區不同的裝載點。為了確保儲存空間磁區使用正確的裝載點，您可以在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 的**磁區詳細資料**頁面上檢視裝載點資訊。您也可以透過 API 呼叫存取正確的裝載點：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

請在這裡再次確認，以查看何時升級其他資料中心以及要針對 {{site.data.keyword.blockstorageshort}} 新增的新特性及功能。
