---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: performance tuning, host performance improvement,

subcollection: BlockStorage

---
{:new_window: target="_blank"}

# Adjusting host queue depth settings
{: #hostqueuesettings}

{{site.data.keyword.BluSoftlayer_full}} suggests a maximum host and application input/output (I/O) queue depth for each performance tier.

<table align="center">
  <caption>Recommended queue depth for each IOPS tier</caption>
        <thead>
	    <tr>
		<th>Performance tier</th>
		<th>Maximum host queue depth</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0.25 IOPS per GB</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2 IOPS per GB</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4 IOPS per GB</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

The host setting doesn’t affect disk and controller latency. It affects only the latency that is observed by the host and application.

Queue depth that exceeds the listed numbers can increase host I/O latency, while queue depth less than the listed number can reduce host I/O performance. Because each application is different, adjustment and observation are required to achieve maximum storage performance.

Host queue depth is typically adjusted in the host bus adapter driver or the hypervisor, and sometimes in the application. Standard defaults such as 32 or 64 can cause excessive host or application latency.

If one host or hypervisor is using multiple performance tiers, use the queue depth for the fastest tier and observe latency on the slowest performance tier.

If latency on the lowest tier is unacceptable, adjust the queue depth until the balance of latency and performance for all tiers is achieved.
