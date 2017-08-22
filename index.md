---

copyright:
  years: 2014, 2017
lastupdated: "2017-08-22"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Getting Started with Block Storage

Block storage is persistent, high performance iSCSI storage that is provisioned and managed independent of compute instances. iSCSI-based block storage LUNs are connected to authorized devices via redundant multi- path I/O (MPIO) connections.

Block Storage brings best-in-class levels of durability and availability with an unmatched feature set and is built using industry standards and best practices, and designed to protect the integrity of the data and maintain availability through maintenance events and unplanned failures while providing a consistent performance baseline.

Take advantage of the following core features of Block Storage:

- **Consistent performance baseline**
   - Provided through the allocation of protocol-level IOPS to individual volumes
- **Highly durable and resilient**
   - Protects the integrity of the data and maintains availability through maintenance events and unplanned failures without the need to create and manage operating system-level redundant array of independent disk (RAID) arrays
- **Data-At-Rest Encryption** ([Available in select data centers](/docs/infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).)
   - Provider managed encryption for data-at-rest at no additional cost
- **All Flash Backed Storage** ([Available in select data centers](/docs/infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).)
   - All flash storage for volumes provisioned with Endurance or Performance at 2IOPS/GB or higher
- **Snapshots** (when provisioned with Endurance or Performance in [select data centers](/docs/infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).
   - Captures point-in-time data snapshots non-disruptively
- **Replication** (when provisioned with Endurance or Performance in [select data centers](/docs/infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).
   - Automatically copies snapshots to a partner IBM Cloud data center
- **Highly available connectivity**
   - Uses redundant networking connections to maximize availability - iSCSI-based Block Storage uses Multipath I/O (MPIO)
- **Concurrent access**
   - Allows multiple hosts to simultaneously access block volumes (up to 8) for clustered configurations.
- **Clustered databases**
   - Supports advanced use cases, such as clustered databases
     
## Provisioning

Block Storage LUNs can be provisioned from 20GB to 12TB with two options for provisioning:

Provision **Endurance** tiers featuring pre-defined performance levels and features like snapshots and replication or build a high-powered **Performance** environment with allocated input/output operations per second (IOPS). 

### Endurance Tiers

Endurance is available in three IOPS performance tiers to support varying application needs. 
Note: Once provisioned, you cannot migrate between tiers.

- **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a given time. Example applications include storing mailboxes or departmental level file shares.
- **2 IOPS per GB** is designed for most general purpose usage. Example applications include hosting small databases backing web applications or virtual machine disk images for a hypervisor.
- **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a given time. Example applications include transactional and other performance-sensitive databases.
- **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL Databases, and data processing for Analytics.  This tier is available for storage provisioned up to 4TB in size in [select data centers](/docs/infrastructure/BlockStorage/new-ibm-block-and-file-storage-location-and-features.html).

Up to 48,000 IOPS are available with a 12TB Endurance volume.
 

While choosing the right tier of Endurance block storage for your workload it is key, it is equally important to use the block size, Ethernet connection speed, and number of hosts necessary to achieve maximum performance. If any of these parts do not align with the other, it can have a significant impact on the resulting throughput.

 
### Performance

Performance is a class of block storage that is designed to support high I/O applications with well understood performance requirements that don’t fit well within an Endurance tier. Predictable performance is achieved through the allocation of protocol-level IOPS to individual volumes. IOPS ranging from 100 through 48,000 can be provisioned with storage sizes that range from 20 GB to 12 TB. 

Performance for block storage is accessed and mounted through a Multipath I/O (MPIO) Internet Small Computer System Interface (iSCSI) connection. Block storage is typically used when the volume will be accessed by a single machine. Multiple volumes can be mounted to a host and striped together to achieve larger volumes and higher IOPS counts. Performance volumes can be ordered according to the sizes and IOPS in Table 1 for Linux, XEN, VMware, and Windows operating systems.
<table cellpadding="1" cellspacing="1">
	<caption>Table 1. -  Sizes and IOPS ranges</caption>
	<tbody>
		<tr>
			<td><strong>Size (GB)</strong></td>
			<td><strong>20</strong></td>
			<td><strong>40</strong></td>
			<td><strong>80</strong></td>
			<td><strong>100</strong></td>
			<td><strong>250</strong></td>
			<td><strong>500</strong></td>
			<td><strong>1,000</strong></td>
			<td><strong>2,000 - 3,000</strong></td>
			<td><strong>4,000 - 7,000</strong></td>
			<td><strong>8,000 - 9,000</strong></td>
			<td><strong>10,000 - 12,000</strong></td>
		</tr>
		<tr>
			<td><strong>Min IOPS</strong></td>
			<td>100</td>
			<td>100</td>
			<td>100</td>
			<td>100</td>
			<td>100</td>
			<td>100</td>
			<td>100</td>
			<td>200</td>
			<td>300</td>
			<td>500</td>
			<td>1,000</td>
		</tr>
		<tr>
			<td><strong>Max IOPS</strong></td>
			<td>1,000</td>
			<td>2,000</td>
			<td>4,000</td>
			<td>6,000</td>
			<td>6,000</td>
			<td>6,000 or 10,000<sup><img src="./images/numberone.png" alt="1"/></sup></td>
			<td>6,000 or 20,000<sup><img src="./images/numberone.png" alt="1"/></sup></td>
			<td>6,000 or 40,000<sup><img src="./images/numberone.png" alt="1"/></sup></td>
			<td>,000 or 48,000<sup><img src="./images/numberone.png" alt="1"/></sup></td>
			<td>6,000 or 48,000<sup><img src="./images/numberone.png" alt="1"/></sup></td>
			<td>6,000 or 48,000<sup><img src="./images/numberone.png" alt="1"/></sup></td>
		</tr>
	</tbody>
</table>

<sup>![footnote](/images/numberone.png)</sup> IOPs limit above 6,000 available in select data centers.


Performance volumes are designed to perform consistently close to the provisioned IOPS level. Consistency makes it easier to size and scale application environments with a given level of performance. Additionally, given the range of volume sizes and IOPS counts, it becomes possible to optimize an environment by building a volume with the ideal price-to-performance ratio.

 

## Tips for Provisioning IOPS for Block Storage


Choosing the block storage that is right for your workload is important, and equally important is how to avoid bottlenecks.  IOPS for both Endurance and Performance is measured based on a 16KB block size with a 50/50 read/write 50% random workload. This is important because if you choose a block size larger than 16KB, the throughput is affected. The reason is that a 16KB block is the equivalent of one write to the volume. Each multiple adds more writes decreasing the response time to the server. For example, a 64KB block size is the equivalent to four writes to the volume. Or, four IOPS per GB at 16KB block size is equivalent to one IOPS per GB at 64KB block size.


Knowing how many IOPS you are getting from your volume can help you determine what your throughput will be. A way to calculate expected throughput is to multiply block size by IOPS (block size * IOPS = throughput). However, throughput can also be constrained by other factors. The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. For example, if you have 6,000 IOPS and are using a 16KB block size, the volume is capable of approximately 94MB per second. If you have a 1Gbps Ethernet connection to your LUN, it will become a bottleneck when your servers attempt to utilize the maximum available throughput.


As a general rule you should not expect to saturate your Ethernet connection beyond 70% of the available bandwidth. If expected workload will require the maximum throughput of your volume, it is recommended to assure that your Ethernet connection speed can accommodate the necessary throughput. In the example above, 70% of the theoretical limit of a 1Gbps Ethernet connection (125MB per second) would allow for 88MB per second. You would encounter a bottleneck if you were attempting to utilize the maximum throughput of 94MB per second of your volume.


Another factor to consider is the number of hosts that are utilizing your volume. If there is a single host that is accessing the volume it may be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput it would best to configure at least two or three servers to access your volume to avoid a single server bottleneck.


To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage and host side and application specific tunings (IP stack, queue depths, and so on).
