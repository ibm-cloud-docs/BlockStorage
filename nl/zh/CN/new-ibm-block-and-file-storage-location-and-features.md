---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 的新位置和功能

{{site.data.keyword.BluSoftlayer_full}} 将推出新版本的 {{site.data.keyword.blockstoragefull}}！ 

新存储器在精选数据中心内可用，是更高 IOPS 级别的闪存支持的存储器，具有针对静态数据的磁盘级别加密。在精选数据中心内供应的所有存储器都将自动通过新版本的 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_full}} 供应。

**注：**新卷的 NFS 安装点已更改。有关详细信息，请参阅下面的**加密 {{site.data.keyword.filestorage_short}} 卷的新安装点**。

新的 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 目前在以下区域/数据中心内可用，很快会增加更多数据中心！
<table style="width:100%;">
	<caption>适用的数据中心</caption>
	<tbody>
		<tr>
			<td><strong>美国 2</strong></td>
			<td><strong>欧盟</strong></td>
			<td><strong>澳大利亚</strong></td>
			<td><strong>加拿大</strong></td>
			<td><strong>拉丁美洲</strong></td>
			<td><strong>亚太地区</strong></td>
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


新存储器具有以下特性和功能：

- [对静态数据进行提供者管理的加密](block-file-storage-encryption-rest.html)。所有 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 都将自动以加密形式免费供应。
- 10 IOPS/GB 层选项。向耐久性类型的 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 添加了一个新层，用于支持要求最苛刻的工作负载。
- 所有存储器均通过闪存支持。在 2 IOPS/GB 或更高级别供应有“耐久性”或“性能”的 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}}，通过全闪存存储器支持。
- 供应有“耐久性”或“性能”的 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 支持快照和复制。
- 为存储器增加了按小时计费选项，此选项计划用于使用时间不到一整月的情况。 
- 供应有“性能”的 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}}，最高 48,000 IOPS。
- 对于季节性负载变化，可以调整 IOPS 速率来提高性能。请在[此处](adjustable-iops.html)阅读有关此功能的更多信息。
- 使用 [{{site.data.keyword.blockstorageshort}} 卷复制功能](how-to-create-duplicate-volume.html)创建数据的新克隆。
- 存储器可动态扩展（以 GB 为增量）到最大 12 TB，无需创建复制项或将数据手动迁移到更大的卷。请在[此处](expandable_block_storage.html)阅读有关此功能的更多信息。

## 加密存储卷的新安装点

这些数据中心内供应的所有加密存储卷的安装点与非加密卷不同。要确保对加密和非加密存储卷使用正确的安装点，可以在 UI 中的“卷详细信息”页面中查看安装点信息，还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

重新检查此处可确定何时已升级其他数据中心以及为 {{site.data.keyword.blockstorageshort}} 添加的新可用特性和功能。
