---

copyright:
  years: 2014, 2025
lastupdated: "2025-03-19"

keywords: Block Storage for Classic, performance tuning, host performance improvement,

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Adjusting host queue depth settings
{: #hostqueuesettings}

{{site.data.keyword.cloud}} suggests a maximum host and application input/output (I/O) queue depth for each performance tier.
{: shortdesc}

| Performance tier | Maximum host queue depth |
|:------:|:------:|
| 0.25 IOPS per GB | 8 |
| 2 IOPS per GB | 24 |
| 4 IOPS per GB | 56 |
| 10 IOPS per GB | 56+ |
{: caption="Recommended queue depth for each IOPS tier" caption-side="bottom"}

The host setting doesn’t affect disk and controller latency. It affects only the latency that is observed by the host and application.

Queue depth that exceeds the listed numbers can increase host I/O latency, while queue depth less than the listed number can reduce host I/O performance. Because each application is different, adjustment and observation are required to achieve maximum storage performance.

Performance tuning is typically a trial-and-error process. For example, for the 10 IOPS per GB performance tier, the recommendation is to start at 56. Then, you can tune up in increments until you see diminishing gains. Then, you back off until you find a good balance of performance and latency.

Host queue depth is typically adjusted in the host bus adapter driver or the hypervisor, and sometimes in the application. Standard defaults such as 32 or 64 can cause excessive host or application latency.

If one host or hypervisor is using multiple performance tiers, use the queue depth for the fastest tier and observe latency on the slowest performance tier.

If latency on the lowest tier is unacceptable, adjust the queue depth until a balance of latency and performance is achieved for all tiers.
