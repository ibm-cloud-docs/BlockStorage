---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Recommendation for Host Queue Depth Settings

{{site.data.keyword.BluSoftlayer_full}} recommends a maximum host and application input/output (I/O) queue depth for each performance tiers. The host setting doesn’t affect disk and controller latency, only latency observed by the host and application.

Queue depth above the recommended numbers can increase host I/O latency; while queue depth below the recommended number may reduce host I/O performance. Because each application is different, adjustment and observation is required to achieve maximum storage performance.

Host queue depth is typically adjusted in the Host Bus Adaptor driver or the hypervisor, and sometimes in the application. Standard defaults such as 32 or 64 may cause excessive host or application latency.

If one host or hypervisor is using multiple performance tiers, use the queue depth for the fastest tier and observe latency on the slowest performance tier. If latency on the lowest tier is unacceptable, adjust the queue depth until the balance of latency and performance for all tiers is observed.

<table align="center">
	<tbody>
		<tr>
			<td><strong>Performance tier</strong></td>
			<td style="text-align: center; vertical-align: middle;">0.25 IOPS per GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS per GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS per GB</td>
		</tr>
		<tr>
			<td><strong>Maximum host queue depth</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
