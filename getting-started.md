---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-28"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 2h

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:ui-linked}

# Getting started with {{site.data.keyword.blockstorageshort}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="2h"}

{{site.data.keyword.blockstoragefull}} is persistent, high-performance iSCSI storage that is provisioned and managed independently of compute instances. iSCSI-based {{site.data.keyword.blockstorageshort}} LUNs are connected to authorized devices through redundant multi-path I/O (MPIO) connections.

{{site.data.keyword.blockstorageshort}} brings best-in-class levels of durability and availability with an unmatched feature set. It is built by using industry standards and best practices. {{site.data.keyword.blockstorageshort}} is designed to protect the integrity of the data and maintain availability through maintenance events and unplanned failures, and provide a consistent performance baseline.
{:shortdesc}

If you're looking for information about using {{site.data.keyword.blockstorageshort}} with the {{site.data.keyword.containerlong}}, see [Storing data on classic IBM Cloud Block Storage](/docs/containers?topic=containers-block_storage).

## Before you begin
{: #prereqs}
{: step}

{{site.data.keyword.blockstorageshort}} LUNs can be provisioned from 20 GB to 12 TB with two options: <br/>
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.blockstorageshort}} offering, see [What is {{site.data.keyword.blockstorageshort}}](https://www.ibm.com/cloud/block-storage){:external}.

## Provisioning considerations
{: #provconsiderBlock}
{: step}

### Block size

IOPS for both Endurance and Performance is based on a 16-KB block size with a 50/50 read/write 50/50 random/sequential workload. A 16-KB block is the equivalent of one write to the volume.
{:important}

The block size that is used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before the IOPS limit.

| Block Size (KB) | IOPS | Throughput (MB/s) |
|-----|-----|-----|
| 4 | 1,000 | 16 |
| 8 | 1,000 | 16 |
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 1 shows examples of how block size and IOPS affect the throughput.<br/>Average IO size x IOPS = Throughput in MB/s." caption-side="top"}

### Authorized hosts

Another factor to consider is the number of hosts that are using your volume. If there's a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

### Network connection

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage, and host side and application-specific tunings (IP stack or [queue depths](/docs/BlockStorage?topic=BlockStorage-hostqueuesettings), and other settings).

Storage traffic should be isolated from other traffic types, and not be directed through firewalls and routers. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#isolatedstoragetraffic).

Storage traffic is included in the total network usage of Public Virtual Servers. For more information about the limits that might be imposed by the service, see the [Virtual Server documentation](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers).
{:tip}

## Submitting your Order
{: #submitorder}
{: step}

When you're ready to submit your order, you can place it through the [Console](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage#orderingthroughConsole), the [SLCLI](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage#orderingthroughCLI), or the [IBMCLOUD CLI](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_order).

## Connecting and configuring your new storage
{: #mountingstorage}
{: step}

When your provisioning request is complete, authorize your hosts to access the new storage, and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Mounting LUNs on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Mounting LUNs on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Mapping LUNS on Microsoft&reg; Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuring {{site.data.keyword.blockstorageshort}} for backup with cPanel](/docs/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuring {{site.data.keyword.blockstorageshort}} for backup with Plesk](/docs/BlockStorage?topic=BlockStorage-PleskBackups)

## Managing your new Storage
{: step}

Through the console or the CLI, you can manage various aspects of your {{site.data.keyword.blockstorageshort}} such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage).
