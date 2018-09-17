---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-17"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} 的新位置和功能

{{site.data.keyword.BluSoftlayer_full}} 将推出新版本的 {{site.data.keyword.blockstoragefull}}！

新的存储器在精选数据中心内提供，是更高 IOPS 级别的闪存支持的存储器，具有针对静态数据的磁盘级别加密。在已升级数据中心内供应的所有存储器都将自动通过新版本创建。

**注：**新卷的 NFS 安装点不同于非加密卷的安装点。有关详细信息，请参阅**加密 {{site.data.keyword.filestorage_short}} 卷的新安装点**。

新的 {{site.data.keyword.blockstorageshort}} 在以下区域/数据中心提供。
<table role="presentation">
	 <tr>
	   <td><strong>美国 2</strong></td>
	   <td><strong>欧盟</strong></td>
	   <td><strong>澳大利亚</strong></td>
	   <td><strong>加拿大</strong></td>
	   <td><strong>拉丁美洲</strong></td>
	   <td><strong>亚太地区</strong></td>
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

*表 1 显示了数据中心可用性。每个区域单独一列。某些城市（例如，达拉斯、圣何塞、华盛顿特区、阿姆斯特丹、法兰克福、伦敦和悉尼）有多个数据中心。*

新存储器具有以下特性和功能：

- **[对静态数据进行提供者管理的加密](block-file-storage-encryption-rest.html)**。
  所有 {{site.data.keyword.blockstorageshort}} 都将自动以加密形式免费供应。
- **10 IOPS/GB 层选项**。
  “耐久性”类型的 {{site.data.keyword.blockstorageshort}} 有一个新层可用，用于支持要求最苛刻的工作负载。
- **所有存储器均通过闪存支持**。
  在 2 IOPS/GB 或更高级别供应的类型为“耐久性”或“性能”的所有 {{site.data.keyword.blockstorageshort}}，通过全闪存存储器予以支持。
- {{site.data.keyword.blockstorageshort}} 支持**快照和复制**
- 为存储器提供了**按小时计费**选项，此选项计划用于使用时间不到一整月的情况。
- 供应的类型为“性能”的 {{site.data.keyword.blockstorageshort}}，**最高 48,000 IOPS**。
- 对于季节性负载变化，**可以调整 IOPS 速率**来提高性能。请在[此处](adjustable-iops.html)阅读有关此功能的更多信息。
- 使用 **[{{site.data.keyword.blockstorageshort}} 卷复制功能](how-to-create-duplicate-volume.html)**创建数据的克隆。
- **存储器可扩展**（以 GB 为增量）到最大 12 TB，无需创建复制项或将数据手动移至更大的卷。请在[此处](expandable_block_storage.html)阅读有关此功能的更多信息。

## 加密存储卷的新安装点

这些数据中心内供应的所有加密存储卷的安装点与非加密卷不同。要确保对存储卷使用正确的安装点，可以在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中的**卷详细信息**页面中查看安装点信息。还可以通过 API 调用来访问正确的安装点：`SoftLayer_Network_Storage::getNetworkMountAddress()`。

升级了更多数据中心时，可重新检查此处来查看这些数据中心，并可了解为 {{site.data.keyword.blockstorageshort}} 添加的新特性和功能。
