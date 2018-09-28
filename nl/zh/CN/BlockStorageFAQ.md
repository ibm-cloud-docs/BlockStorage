---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-18"

---
{:new_window: target="_blank"}

# 常见问题

## 一个 {{site.data.keyword.blockstorageshort}} 卷可以由多少个实例共享使用？
每个块卷的缺省授权数限制为 8。这意味着，最多可授权 8 个主机来访问块存储器 LUN。要请求增大此限制，请联系销售代表。

## 可以订购多少个卷？
缺省情况下，总共可以供应 250 个 {{site.data.keyword.blockstorageshort}} 卷。要增大卷限制，请联系销售代表。有关更多信息，请参阅[管理存储限制](managing-storage-limits.html)。

## 在一个主机上可以安装多少个 {{site.data.keyword.blockstorageshort}} 卷？
这取决于主机操作系统的处理能力，而不受 {{site.data.keyword.BluSoftlayer_full}} 的限制。有关可安装的卷数的限制，请参阅操作系统文档。

## 是按实例还是按卷强制执行分配的 IOPS 限制？
IOPS 会在卷级别强制执行。换句话说，连接到一个具有 6000 IOPS 的卷的两个主机会共享 6000 IOPS。

## 度量 IOPS
IOPS 根据 16 KB 块的负载概要文件来度量，其中随机 50% 读操作和 50% 写操作。不同于此概要文件的工作负载可能会遇到性能降低问题。

## 使用较小的块大小来度量性能时会发生什么情况？
使用更小的块大小时，仍然可以获得最大 IOPS。但是吞吐量会下降。例如，下面是具有 6000 IOPS 的卷针对各种不同块大小的吞吐量：

- 16 KB * 6000 IOPS == 约 93.75 MB/秒 
- 8 KB * 6000 IOPS == 约 46.88 MB/秒
- 4 KB * 6000 IOPS == 约 23.44 MB/秒

## 卷需要预热才能达到所需吞吐量吗？
不需要预热。在供应卷之后，您可以立即观察到指定的吞吐量。

## 使用更快的以太网连接可以实现更大的吞吐量吗？
吞吐量限制是在逐个卷/LUN 级别设置的，因此使用更快的以太网连接并不会增加该设定限制。但是，使用较慢的以太网连接时，带宽可能是潜在瓶颈。

## 防火墙/安全组会影响性能吗？
最好是在绕过防火墙的 VLAN 上运行存储流量。通过软件防火墙运行存储流量会延长等待时间，并对存储器性能产生负面影响。

## 预计 {{site.data.keyword.blockstorageshort}} 中的等待时间是多少？   
存储器中的目标等待时间小于 1 毫秒。存储器会连接到共享网络上的计算实例，所以确切的性能等待时间将取决于运行期间的网络流量。

## 为什么可以在一些数据中心内订购耐久性 10 IOPS/GB 层的 {{site.data.keyword.blockstorageshort}}，而在其他数据中心内不行？
“耐久性”类型的 {{site.data.keyword.blockstorageshort}} 的 10 IOPS/GB 层仅在精选数据中心内提供，会逐渐增加新的数据中心。您可以在[此处](new-ibm-block-and-file-storage-location-and-features.html)找到已升级的数据中心和可用功能的完整列表。

## 如何判断哪些 {{site.data.keyword.blockstorageshort}} LUN/卷已加密？
在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中查看 {{site.data.keyword.blockstorageshort}} 的列表时，加密 LUN/卷的名称右侧会显示“锁定”图标。

## 怎样知道是在已升级的数据中心内供应 {{site.data.keyword.blockstorageshort}}？
订购 {{site.data.keyword.blockstorageshort}} 时，在订购表单中会用星号 (`*`) 表示所有已升级的数据中心，并指示即将供应使用加密的存储器。供应存储器后，在存储器列表中会看到相应图标，指示该存储器已加密。所有加密卷和 LUN 仅在已升级的数据中心内供应。您可以在[此处](new-ibm-block-and-file-storage-location-and-features.html)找到已升级的数据中心和可用功能的完整列表。

## 如果在最近升级的数据中心内拥有非加密 {{site.data.keyword.blockstorageshort}}，可以加密该 {{site.data.keyword.blockstorageshort}} 吗？
无法对在数据中心升级之前供应的 {{site.data.keyword.blockstorageshort}} 加密。
在已升级的数据中心内供应的新 {{site.data.keyword.blockstorageshort}} 会自动加密。没有加密设置可供选择，这是自动执行的操作。通过创建新的块 LUN，然后使用基于主机的迁移将数据复制到新的已加密 LUN，可以对已升级数据中心内非加密存储器上的数据进行加密。单击[此处](migrate-block-storage-encrypted-block-storage.html)以获取指示信息。

## {{site.data.keyword.blockstorageshort}} 支持 SCSI-3 持久性预留量以对 Db2 pureScale 实施 I/O 电子篱笆吗？
是的，{{site.data.keyword.blockstorageshort}} 支持 SCSI-2 和 SCSI-3 持久性预留量。

## 删除 {{site.data.keyword.blockstorageshort}} LUN 时，数据会发生什么情况？
{{site.data.keyword.blockstoragefull}} 会在物理存储器上为客户提供块卷，并且会在重复使用物理存储器之前擦除其上的数据。如果客户对合规性有特殊要求（如 NIST 800-88《存储介质清理指南》），那么在删除存储器之前，必须执行数据清理过程。

## 使驱动器从云数据中心退役时会发生什么情况？
使驱动器退役后，IBM 会在处置前先销毁驱动器。驱动器将变为无法使用。写入该驱动器的任何数据都将变得无法访问。
