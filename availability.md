---

copyright:
  years: 2014, 2026
lastupdated: "2026-06-17"

keywords: Block Storage for Classic, block storage, iSCSI, durability, availability, HA, high-availability, data loss, data integrity, uptime, five 9's, eleven 9's, data health, data corruption, data decay, encryption, security, integrity

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# High availability and durability for {{site.data.keyword.blockstorageshort}}
{: #storageavailability}

{{site.data.keyword.blockstoragefull}} delivers 99.999% availability and 99.999999999% durability with Advanced Encryption Standard (AES)-256 encryption and redundant storage.
{: shortdesc}

Enterprise data storage requires both immediate accessibility and long-term data integrity. According to {{site.data.keyword.cloud}} infrastructure standards, Block Storage is designed to provide continuous data access while it protects against hardware failures, data corruption, and unauthorized access.

The following table provides an overview of the durability, availability, and encryption features of Block Storage.

| Storage type | Purpose | Durability | Availability | Encryption |
|--------------|----------|------------|--------------|------------|
| Classic Endurance - \n 0.25 IOPS per GB tier  |  It is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a time. Example applications include storing mailboxes or departmental-level file shares. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption at rest.|
| Classic Endurance - \n 2 IOPS per GB tier | It is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or virtual machine disk images for a hypervisor.| 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| Classic Endurance - \n 4 IOPS per GB tier| It is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a time. Example applications include transactional and other performance-sensitive databases. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| Classic Endurance - \n 10 IOPS per GB tier| It is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption at rest. |
| Classic Performance - \n A high-powered environment with custom IOPS | It is designed to manage rapid data changes with well-defined performance requirements. | 99.999999999% \n (11 9's) | 99.999% \n (5 9's) | Provider-managed AES-256 encryption at rest. |
{: caption="Storage durability and availability chart." caption-side="bottom"}

The values in the table represent service level objectives (SLOs), design targets that indicate the performance and availability levels the storage service is engineered to meet. These SLOs are not contractual service level agreements (SLAs) and do not constitute a guarantee. For more information, see the [Service Level Agreements for IBM Cloud](https://www.ibm.com/support/customer/csol/terms/?id=i126-9268&lc=en){: external}.

## Durability
{: #stordurability}

Durability measures data integrity and resilience over time. {{site.data.keyword.blockstorageshort}} is designed to achieve 99.999999999% (eleven 9's) durability, which means storing 10 million files results in an expected loss of only one file every 10,000 years. This durability level protects data against decay, drive failures, and corruption.

{{site.data.keyword.blockstorageshort}} protects data through redundant storage across multiple physical disks within an Availability Zone. This architecture prevents data loss from single component failures, including drive failures, disk errors, and hardware malfunctions that typically compromise data durability and integrity.

Beyond hardware failures, accidental deletion and user modifications represent common data loss scenarios. {{site.data.keyword.blockstorageshort}} restricts access to authorized hosts within your network, providing administrator-controlled access management. Snapshot functionality enables rapid data restoration when users accidentally modify or delete critical data. For more information about this feature, see [Snapshots](/docs/BlockStorage?topic=BlockStorage-snapshots).

The eleven 9's durability target applies within a single Availability Zone. To protect against zone-wide disasters (natural or human-made), {{site.data.keyword.cloud}} recommends storing critical data across multiple geographic locations. For more information, see [Replicating Data](/docs/BlockStorage?topic=BlockStorage-replication).

## High Availability
{: #storavailability}

{{site.data.keyword.blockstorageshort}} is designed to achieve high availability through enterprise-grade hardware and redundant architecture. Data is stored across multiple physical disks on high-availability (HA) paired nodes. Each storage node maintains multiple paths to both its own Solid-State Drives (SSDs) and its partner node's SSDs. This multi-path configuration helps ensure continued operation during path failures or controller failures, as nodes can access partner disks for uninterrupted service. Redundant network ports and paths provide extra protection against network failures.

## Encryption
{: #storencryption}

{{site.data.keyword.cloud}} implements provider-managed Advanced Encryption Standard (AES)-256 encryption at rest for all Block Storage volumes. This full-disk encryption maintains data security without impacting storage performance. For more information about encryption of {{site.data.keyword.blockstorageshort}}, see [Securing your data in {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-mng-data). For more information about provider- and customer-managed encryption in a VPC, see [Data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption).
