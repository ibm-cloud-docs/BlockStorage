---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-29"

---
{:new_window: target="_blank"}
{:tip: .tip} 
{:note: .note} 
{:important: .important}

# Getting started with {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} is persistent, high-performance iSCSI storage that is provisioned and managed independently of compute instances. iSCSI-based {{site.data.keyword.blockstorageshort}} LUNs are connected to authorized devices through redundant multi-path I/O (MPIO) connections.

{{site.data.keyword.blockstorageshort}} brings best-in-class levels of durability and availability with an unmatched feature set. It is built by using industry standards and best practices. {{site.data.keyword.blockstorageshort}} is designed to protect the integrity of the data and maintain availability through maintenance events and unplanned failures, and provide a consistent performance baseline.

## Core Features

Take advantage of the following features of {{site.data.keyword.blockstorageshort}}:

- **Consistent performance baseline**
   - Provided through the allocation of protocol-level IOPS to individual volumes.
- **Highly durable and resilient**
   - Protects the integrity of the data and maintains availability through maintenance events and unplanned failures without the need to create and manage operating system-level redundant array of independent disk (RAID) arrays.
- **Data-At-Rest Encryption** ([Available in select data centers](new-ibm-block-and-file-storage-location-and-features.html))
   - Provider-managed encryption for data-at-rest at no additional cost.
- **All Flash Backed Storage** ([Available in select data centers](new-ibm-block-and-file-storage-location-and-features.html))
   - All flash storage for volumes that are provisioned with Endurance or Performance at 2 IOPS/GB or higher levels.
- **Snapshots** ([Available in select data centers](new-ibm-block-and-file-storage-location-and-features.html))
   - Captures point-in-time data snapshots non-disruptively.
- **Replication** ([Available in select data centers](new-ibm-block-and-file-storage-location-and-features.html))
   - Automatically copies snapshots to a partner {{site.data.keyword.BluSoftlayer_full}} data center.
- **Highly available connectivity**
   - Uses redundant networking connections to maximize availability 
   - iSCSI-based {{site.data.keyword.blockstorageshort}} uses Multipath I/O (MPIO).
- **Concurrent access**
   - Allows multiple hosts to simultaneously access block volumes (up to eight) for clustered configurations.
- **Clustered databases**
   - Supports advanced use cases, such as clustered databases.
     
## Billing

You can select hourly or monthly billing for a Block LUN. The type of billing that is selected for a LUN applies to its snapshot space and replicas. For example, if you provision a LUN with hourly billing, any snapshots or replica fees are billed hourly. If you provision a LUN with monthly billing, any snapshots or replica fees are billed monthly. 

With **hourly billing**, the number of hours the block LUN existed on the account is calculated at the time the LUN is deleted or at the end of the billing cycle, which ever comes first. Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is available for storage that is provisioned in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) only. 

With **monthly billing**, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. If a LUN is deleted before the end of the billing cycle, there's no refund. Monthly billing is a good choice for storage that is used in production workloads that use data that needs to be stored and accessed for long periods of time (one month or longer). 

**Performance**
<table>
  <caption>Table 1 is showing the prices for Performance Storage with monthly and hourly billing.</caption>
  <tr>
   <th>Monthly Price</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>Hourly Price</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>
 
**Endurance**
<table>
  <caption>Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options.</caption>
  <tr>
   <th>IOPS Tier</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monthly Price</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0.20/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>Hourly Price</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



## Provisioning

{{site.data.keyword.blockstorageshort}} LUNs can be provisioned from 20 GB to 12 TB with two options: <br/>
- Provision **Endurance** tiers that feature pre-defined performance levels and other features like snapshots and replication.
- Build a high-powered **Performance** environment with allocated input/output operations per second (IOPS). 

### Provisioning with Endurance Tiers

Endurance {{site.data.keyword.blockstorageshort}} is available in four IOPS performance tiers to support varying application needs. <br />

- **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at any time. Example applications include storing mailboxes or departmental level file shares.

- **2 IOPS per GB** is designed for most general-purpose usage. Example applications include hosting small databases that are backing web applications or VM disk images for a hypervisor.

- **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at any time. Example applications include transactional and other performance-sensitive databases.

- **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL databases, and data processing for Analytics. This tier is available for storage that is provisioned up to 4 TB in [select data centers](new-ibm-block-and-file-storage-location-and-features.html) only.

Up to 48,000 IOPS are available with a 12 TB Endurance volume.
 
Choosing the right Endurance tier for your workload is key. It's equally important to use the right block size, Ethernet connection speed, and the number of hosts necessary to achieve maximum performance. If any of these parts don't align with the other, it can have a significant impact on the resulting throughput.

 
### Provisioning with Performance

Performance is a class of {{site.data.keyword.blockstorageshort}} that is designed to support high I/O applications with understood performance requirements that don't fit well within an Endurance tier. Predictable performance is achieved through the allocation of protocol-level IOPS to individual volumes. Various IOPS rates (100 - 48,000) can be provisioned with storage sizes that range from 20 GB to 12 TB. 

Performance for {{site.data.keyword.blockstorageshort}} is accessed and mounted through a Multipath I/O (MPIO) internet Small Computer System Interface (iSCSI) connection. {{site.data.keyword.blockstorageshort}} is typically used when the volume is accessed by a single server. Multiple volumes can be mounted to a host and striped together to achieve larger volumes and higher IOPS counts. Performance volumes can be ordered according to the sizes and IOPS rates in Table 3 for Linux, XEN, and Windows operating systems.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>Table 3 is showing size and IOPS combinations for Performance storage.<br/><sup><img src="/images/numberone.png" alt="Footnote" /></sup> IOPS limit greater than 6,000 is available in select data centers.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Size (GB)</th>
            <th>Min IOPS</th>
            <th>Max IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 or 10,000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 or 20,000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 or 40,000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 or 48,000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 or 48,000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 or 48,000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
</table>


Performance volumes are designed to operate consistently close to the provisioned IOPS level. Consistency makes it easier to size and scale application environments with a specific level of performance. Additionally, it's possible to optimize an environment by building a volume with the ideal price-to-performance ratio.

### Provisioning considerations

**Block size**

IOPS for both Endurance and Performance is based on a 16 KB block size with a 50/50 read/write 50 percent random workload. A 16 KB block is the equivalent of one write to the volume.
{:important}

The block size that is used by your application directly impacts the storage performance. If the block size that is used by your application is smaller than 16 KB, the IOPS limit is realized before the throughput limit. Conversely, if the block size that is used by your application is larger than 16 KB, the throughput limit is realized before to the IOPS limit.

<table>
  <caption>Table 4 shows examples of how block size and IOPS affect the throughput.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Block Size (KB)</th>
            <th>IOPS</th>
            <th>Throughput (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (typical for Linux)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (typical for Oracle)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (typical for SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**Authorized hosts**

Another factor to consider is the number of hosts that are using your volume. If there's a single host that is accessing the volume, it can be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput, it would be best to configure at least a couple servers to access your volume to avoid a single-server bottleneck.

**Network connection**

The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16 KB block size, the volume can handle approximately 94 MBps throughput. If you have a 1 Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1 Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.

To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage, and host side and application-specific tunings (IP stack or [queue depths](set-host-queue-depth-settings-performance-and-endurance-storage.html), and other settings).

Storage traffic is included in the total network usage of Public Virtual Servers. Please see the [Virtual Server documentation](https://console.bluemix.net/docs/vsi/vsi_public.html#public-virtual-servers) to understand limits that may be imposed by the service.
{:tip}

## Submitting your Order

When you're ready to submit your order, follow the instructions [here](provisioning-block_storage.html).

## Connecting your new storage

When your provisioning request is complete, authorize your hosts to access the new storage and configure your connection. Depending on your host's operating system, follow the appropriate link.
- [Connecting to MPIO iSCSI LUNs on Linux](accessing_block_storage_linux.html)
- [Connecting to MPIO iSCSI LUNs on CloudLinux](configure-iscsi-cloudlinux.html)
- [Connecting to MPIO iSCSI LUNS on Microsoft Windows](accessing-block-storage-windows.html)
- [Configuring Block Storage for Backup with cPanel](configure-backup-cpanel.html)
- [Configuring Block Storage for Backup with Plesk](configure-backup-plesk.html)

