---

copyright:
  years: 2014, 2023
lastupdated: "2023-03-20"

keywords: Block Storage, block storage, ISCSI, durability, availability, HA, high-availability, data loss, data integrity, uptime, five 9's, eleven 9's, data health, data corruption, data decay, encryption, security, integrity

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Availability and Durability of {{site.data.keyword.blockstorageshort}}
{: #storageavailability}

In today's fast-paced economy, companies rely on data in their decision-making. They need secure and immediate access to their data on a moment's notice. Data integrity is high priority because compromised or incomplete data is of no use. Not to mention the dangers that are presented if sensitive data goes missing. When you store your data in {{site.data.keyword.blockstoragefull}}, it's durable, highly available, and encrypted.
{: shortdesc}

| Storage type | Purpose | Durability | Availability | Encryption |
|--------------|----------|------------|--------------|------------|
| Classic Endurance - \n 0.25 IOPS per GB tier  |  It is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental level file shares. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption at rest.|
| Classic Endurance - \n 2 IOPS per GB tier | It is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.| 99.999999999%  \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| Classic Endurance - \n 4 IOPS per GB tier| It is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases. | 99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| Classic Endurance - \n 10 IOPS per GB tier| It is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. | 99.999999999%  \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| Classic Performance - \n A high-powered environment with custom IOPS | It is designed to manage rapid data changes with well-defined performance requirements. | 99.999999999%  \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| VPC Storage - \n 3 IOPS per GB tier| It is designed for general-purpose workloads such as workloads that host small databases for web applications or store virtual machine disk images for a hypervisor. |  99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption + Customer-managed encryption |
| VPC Storage - \n 5 IOPS per GB tier| It is designed for high I/O intensity workloads that are characterized by a large percentage of active data, such as transactional and other performance-sensitive databases. |  99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption + Customer-managed encryption |
| VPC Storage - \n 10 IOPS per GB tier| It is designed for demanding storage workloads such as data-intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics. |  99.999999999% \n (11 9's) | 99.999%  \n (5 9's) | Provider-managed AES-256 encryption + Customer-managed encryption |
{: caption="Table 1. Storage durability and availability chart." caption-side="top"}

## Durability
{: #stordurability}

Think of durability as a measurement of how healthy and resilient your data is. Durability in {{site.data.keyword.blockstorageshort}} means that your data is stored consistent and intact without any signs of data decay, influence of drive failures, or any other form of corruption. 99.999999999% (11 nines) durability means that if you store 10 million files, then you expect to lose one file every 10000 years.

When people hear the word durability, most of them think of hardware failures of storage, Compute, and network components that might cause data loss. In {{site.data.keyword.blockstorageshort}}, your data is protected against drive failures and numerous type of disk errors that otherwise might negatively impact data durability and data integrity. The data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

Other than physical failure, a common source of data loss is accidental deletion or modifications of files by users. {{site.data.keyword.blockstorageshort}} is only accessible to authorized hosts within your network. You control who can access it. Another measure to protect against accidental deletion and modification of files by users is a snapshot. If a user accidentally modifies or deletes crucial data from a volume, the data can be easily and quickly restored from a snapshot copy. For more information about this feature, see [Snapshots](/docs/BlockStorage?topic=BlockStorage-snapshots).

The 11 nines durability target applies to a single Availability Zone. To protect against natural or human-made disasters that might destroy an entire Availability Zone, consider storing your most important data in multiple locations. For more information, see [Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication).

## High Availability
{: #storavailability}

{{site.data.keyword.blockstorageshort}} is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. The data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure and controller failure because the node can still access its partner's disks for continued productivity. Redundant network ports and paths protect against network failures across the cloud connections.

## Encryption
{: #storencryption}

{{site.data.keyword.cloud}} provides full-disk encryption without compromising storage application performance. For more information about encryption of {{site.data.keyword.blockstorageshort}}, see [Securing your data in {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-mng-data). For more information about provider- and customer-managed encryption in a VPC, see [Data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption).
