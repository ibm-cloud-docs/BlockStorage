---

copyright:
  years: 2014, 2022
lastupdated: "2022-07-01"

keywords: Block Storage, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:faq: data-hd-content-type='faq'}
{:support: data-reuse='support'}

# Best Practices for {{site.data.keyword.blockstorageshort}}
{: #best-practices-classic}

Follow our best practices to maximize the performance of your storage, and avoid application downtime.
{: shortdesc}

## Best Practice 1: Clear the path
{: #bestpractice1}

To achieve mximum IOPS, adequate network resources need to be in place. 

* **Avoid routing your storage traffic to a gateway device** whenever possible. When storage traffic is routed to a gateway device, this can add latency to storage traffic or it can cause storage traffic disruption if the firewall in the gateway device is misconfigured. The storage disruption is especially true when a maintenance such as a reboot is required on a single (non-clustered) gateway device. If a storage traffic must be routed through a gateway device, ensure that  the gateway device has an at least 10-Gbps interface or the gateway device might becomes a network bottleneck.

* **Route storage traffic on aa dedicate VLAN.** Running storage traffic through software firewalls also increases latency and adversely affects storage performance. It's best to run storage traffic on a VLAN, which bypasses the firewall. For more information, see [routing storage traffic to its own VLAN interface](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs&interface=ui#howtoisolatedstorage).

* **Use a faster NIC.** There are limits set at the LUN level and a faster interface doesn't increase that limit. However, with a slower Ethernet connection, your bandwidth can be a potential hinderance to achieving best performance levels.

* **Choose a higher bandwidth.** The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth.
     For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. However, when you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

* **[Enable Jumbo Frames](/docs/BlockStorage?topic=FileStorage-jumboframes&interface=ui) and configure them to be the same on the entire network path** from source device <-> switch <-> router <-> switch <-> target device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.cloud}} has network devices set to 9,000 currently. For best performance, all customer devices need to be set to the same 9,000 value.

Best Practice 2: Create multiple paths for redundancy
{: #bestpractice2}

{{site.data.keyword.blockstorageshort}} is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. The data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure and controller failure because the node can still access its partner's disks for continued productivity. Redundant network ports and paths protect against network failures across the cloud connections.

**Do not run iSCSI traffic over 802.3ad LACP port channel.** Link Aggregation Control Protocol (LACP) is not a recommended configuration with iSCSI. 

**Use Multi-Path Input/Output (MPIO) framework for I/O balancing and redundancy.** With an MPIO configuration, a server with multiple NICs can transmit and receive I/O across all available interfaces to a corresponding MPIO-enabled storage device. This provides redundancy that can ensure that the storage traffic remains steady even if one of the paths becomes unavailable. If a server has two 1-Gb NICs and the storage server has two 1-Gb NICs, the theoretical maximum throughput is about 200 MB/s.

While it is possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, it is important to establish connections on both paths to ensure no disruption of service. If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance.

Best Practice 3: Optimize the host and applications
{: #bestpractice3}











