---

copyright:
  years: 2014, 2024
lastupdated: "2024-05-28"

keywords: Block Storage for Classic, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, iSCSI, MPIO, redundant

subcollection: BlockStorage

content-type: tutorial
services:
account-plan: paid
completion-time: 2h

---
{{site.data.keyword.attribute-definition-list}}
{: ui-linked}

# Getting started with {{site.data.keyword.blockstorageshort}}
{: #getting-started}
{: toc-content-type="tutorial"}
{: toc-services=""}
{: toc-completion-time="2h"}

{{site.data.keyword.blockstoragefull}} is persistent, high-performance iSCSI storage that is provisioned and managed independently of Compute instances. iSCSI-based {{site.data.keyword.blockstorageshort}} LUNs are connected to authorized devices through redundant multi-path I/O (MPIO) connections.

{{site.data.keyword.blockstorageshort}} brings best-in-class levels of durability and availability with an unmatched feature set. It is built by using industry standards and best practices. {{site.data.keyword.blockstorageshort}} is designed to protect the integrity of the data and maintain availability through maintenance events and unplanned failures, and provide a consistent performance baseline.
{: shortdesc}

For more information about using {{site.data.keyword.blockstorageshort}} with the {{site.data.keyword.containerlong}}, see [Storing data on classic IBM Cloud Block Storage](/docs/containers?topic=containers-block_storage).

## Before you begin
{: #prereqs}
{: step}

{{site.data.keyword.blockstorageshort}} LUNs can be provisioned from 20 GB to 12 TB with two options:
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS).

For more information about the {{site.data.keyword.blockstorageshort}} offering, see [What is {{site.data.keyword.blockstorageshort}}](https://www.ibm.com/products/block-storage){: external}.

## Provisioning considerations
{: #provconsiderBlock}
{: step}

### Block size
{: #blocksizeBlock}

IOPS for both Endurance and Performance is based on a 16-KB IO size with a 50/50 read and write, 50/50 random and sequential workload. A 16-KB block is the equivalent of one write operation to the volume.
{: important}

The IO size that is used by your application directly impacts the storage performance. If the IO size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the IO size that is used by your application is larger than 16 KB, the throughput limit is realized before the IOPS limit.

| IO Size (KB) | IOPS | Throughput (MB/s) |
|-----|-----|-----|
| 4 | 1,000 | 4 |
| 8 | 1,000 | 8 |
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 1 shows examples of how block size and IOPS affect the throughput. Average IO size x IOPS = Throughput in MB/s." caption-side="top"}

### Authorized hosts
{: #numberofhosts}

Another factor to consider is the number of hosts that are using your volume. IOPS limits are enforced at the volume level. In other words, two hosts connected to a volume with 6000 IOPS share that 6000 IOPS. At high IOPS counts, you might several hosts to access the volume simultaneously to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s).

The maximum IOPS for a block storage volume is 48,000 IOPS. If your workload requires high throughput, it's best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

The default limit for the number of authorizations per block volume is eight. That means that up to eight hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} LUN. For more information about authorization and increasing the limit of 8, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#authlimit).

### Network connection
{: #networkconnectivity}

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16-KB IO size, the volume can handle approximately 94-MBps throughput. If you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage, and host-side and application-specific tunings (IP stack or [queue depths](/docs/BlockStorage?topic=BlockStorage-hostqueuesettings), and other settings).

Storage traffic ought to be isolated from other traffic types, and not be directed through firewalls and routers. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#isolatedstoragetraffic).

Storage traffic is included in the total network usage of Public virtual servers. For more information about the limits that might be imposed by the service, see the [virtual server Documentation](/docs/virtual-servers?topic=virtual-servers-about-virtual-servers).
{: tip}

## Submitting your order
{: #submitorder}
{: step}

When you're ready to submit your order, you can place it in the [console](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui#orderingthroughConsole), from the [CLI](https://cloud.ibm.com/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=cli#orderingthroughCLI), with the [API](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=api#orderingthroughAPI), or [Terraform](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=terraform#orderingthroughTerraform).

By default, you can provision a combined total of 700 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes globally. If you require more storage, see [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).
{: note}

## Connecting and configuring your new storage
{: #mountingstorage}
{: step}

When your provisioning request is complete, authorize your hosts to access the new storage, and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [RHEL 6]{: tag-linux} [Mount iSCSI LUNs on Linux&reg; - RHEL6 and CentOS6](/docs/BlockStorage?topic=BlockStorage-mountingLinux).
- [RHEL 8]{: tag-linux} [Mount iSCSI LUN on Red Hat Enterprise Linux&reg; 8](/docs/BlockStorage?topic=BlockStorage-mountingRHEL8).
- [Cloudlinux 6]{: tag-linux} [Mount iSCSI LUNs on CloudLinux 6.10](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux).
- [CloudLinux 8]{: tag-linux} [Mount iSCSI LUN on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8).
- [Ubuntu 20]{: tag-linux} [Mount iSCSI LUN on Ubuntu 20](/docs/BlockStorage?topic=BlockStorage-mountingUbu20).
- [Debian 10]{: tag-linux} [Mount iSCSI LUN on Debian 10](/docs/BlockStorage?topic=BlockStorage-mountingdebian10).
- [Windows]{: tag-windows}[Mapping LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows).
- [cPanel]{: tag-app}[Configuring {{site.data.keyword.blockstorageshort}} for backup with cPanel](/docs/BlockStorage?topic=BlockStorage-cPanelBackups).
- [Plesk]{: tag-app} [Configuring {{site.data.keyword.blockstorageshort}} for backup with Plesk](/docs/BlockStorage?topic=BlockStorage-PleskBackups).

## Managing your new storage
{: #managingnewstorage}
{: step}

In the console, from the CLI, with the API, or Terraform, you can manage various aspects of your {{site.data.keyword.blockstorageshort}} such as host authorizations and cancellations. For more information, see [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage).

You can keep your data in sync in two different locations by using replication. Replication uses one of your snapshot schedules to automatically copy snapshots to a destination volume in a remote data center. The copies can be recovered in the remote site if a catastrophic event occurs or your data becomes corrupted. For more information, see [Replication and Disaster Recovery – Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication&interface=ui).
