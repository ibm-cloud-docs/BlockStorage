---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 对主机队列深度设置的建议

{{site.data.keyword.BluSoftlayer_full}} 建议为每个性能层设置最大主机和应用程序输入/输出 (I/O) 队列深度。主机设置不会影响磁盘和控制器等待时间，仅影响主机和应用程序观察到的等待时间。

高于建议数字的队列深度可能会增加主机 I/O 等待时间；而低于建议数字的队列深度可能会降低主机 I/O 性能。由于每个应用程序各不相同，因此需要调整和观察以实现最高存储性能。

主机队列深度通常在主机总线适配器驱动程序或系统管理程序中进行调整，有时也在应用程序中进行调整。标准缺省值（如 32 或 64）可能会导致主机或应用程序等待时间太长。

如果一个主机或系统管理程序在使用多个性能层，请对速度最快的层使用队列深度，并观察速度最慢的性能层上的等待时间。如果无法接受速度最慢的层上的等待时间，请调整队列深度，直至观察到所有层的等待时间和性能均衡为止。

<table align="center">
	<tbody>
		<tr>
			<td><strong>性能层</strong></td>
			<td style="text-align: center; vertical-align: middle;">0.25 IOPS/GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS/GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS/GB</td>
		</tr>
		<tr>
			<td><strong>最大主机队列深度</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
