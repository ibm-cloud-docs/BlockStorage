---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Getting Started with {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} is persistent, high performance iSCSI storage that is provisioned and managed independent of compute instances. iSCSI-based {{site.data.keyword.blockstorageshort}} LUNs are connected to authorized devices via redundant multi- path I/O (MPIO) connections.

{{site.data.keyword.blockstorageshort}} brings best-in-class levels of durability and availability with an unmatched feature set and is built using industry standards and best practices, and designed to protect the integrity of the data and maintain availability through maintenance events and unplanned failures while providing a consistent performance baseline.

Take advantage of the following core features of {{site.data.keyword.blockstorageshort}}:

- **Consistent performance baseline**
   - Provided through the allocation of protocol-level IOPS to individual volumes
- **Highly durable and resilient**
   - Protects the integrity of the data and maintains availability through maintenance events and unplanned failures without the need to create and manage operating system-level redundant array of independent disk (RAID) arrays
- **Data-At-Rest Encryption** ([Available in select data centers](new-ibm-block-and-file-storage-location-and-features.html).)
   - Provider managed encryption for data-at-rest at no additional cost
- **All Flash Backed Storage** ([Available in select data centers](new-ibm-block-and-file-storage-location-and-features.html).)
   - All flash storage for volumes provisioned with Endurance or Performance at 2 IOPS/GB or higher
- **Snapshots** (when provisioned with Endurance or Performance in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).
   - Captures point-in-time data snapshots non-disruptively
- **Replication** (when provisioned with Endurance or Performance in [select data centers](/new-ibm-block-and-file-storage-location-and-features.html).
   - Automatically copies snapshots to a partner {{site.data.keyword.BluSoftlayer_full}} data center
- **Highly available connectivity**
   - Uses redundant networking connections to maximize availability - iSCSI-based {{site.data.keyword.blockstorageshort}} uses Multipath I/O (MPIO)
- **Concurrent access**
   - Allows multiple hosts to simultaneously access block volumes (up to 8) for clustered configurations.
- **Clustered databases**
   - Supports advanced use cases, such as clustered databases
     
## Hourly/Monthly Billing

You can select hourly or monthly billing for a Block LUN. The type of billing selected for a LUN will apply to its snapshot space and replicas. For example, if you provision a LUN with hourly billing, any snapshots or replica fees will be billed hourly. If you provision a LUN with monthly billing, any snapshots or replica fees will be billed monthly. 

With **hourly billing**, the calculation of the number of hours the block LUN existed on the account is performed at the time the LUN is deleted or at the end of the billing cycle, which ever comes first.  Hourly billing is a good choice for storage that is used for a few days or less than a full month. Hourly billing is only available for storage provisioned in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). 

With **monthly billing**, the calculation for the price is pro-rated from the date of creation to the end of the billing cycle and billed immediately. There is no refund If a LUN is deleted before the end of the billing cycle.  Monthly billing is a good choice for storage used in production workloads that use data that needs to be stored and accessed for long periods of time (one month or longer). 

### Performance:
<table>
<table>
 <tbody>
  <tr>
   <th>Monthly Price</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>Hourly Price</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Endurance:
<table>
<table>
 <tbody>
  <tr>
   <th>IOPS Tier</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monthly Price</th>
   <td>$0.10/GB</td>
   <td>$0.20/GB</td>
   <td>$0.35/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>Hourly Price</th>
   <td>0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0005/GB</td>
   <td>$0.0009/GB</td>
  </tr>
  </tbody>
</table>



## Provisioning

{{site.data.keyword.blockstorageshort}} LUNs can be provisioned from 20 GB to 12 TB with two options for provisioning:

Provision **Endurance** tiers featuring pre-defined performance levels and features like snapshots and replication or build a high-powered **Performance** environment with allocated input/output operations per second (IOPS). 

### Endurance Tiers

Endurance is available in three IOPS performance tiers to support varying application needs. <br />
Note: Once provisioned, you cannot migrate between tiers.

- **0.25 IOPS per GB** is designed for workloads with low I/O intensity. These workloads are typically characterized by having a large percentage of data inactive at a given time. Example applications include storing mailboxes or departmental level file shares.
- **2 IOPS per GB** is designed for most general purpose usage. Example applications include hosting small databases backing web applications or virtual machine disk images for a hypervisor.
- **4 IOPS per GB** is designed for higher-intensity workloads. These workloads are typically characterized by having a high percentage of data active at a given time. Example applications include transactional and other performance-sensitive databases.
- **10 IOPS per GB** is designed for the most demanding workloads such as those created by NoSQL Databases, and data processing for Analytics.  This tier is available for storage provisioned up to 4 TB in size in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).

Up to 48,000 IOPS are available with a 12 TB Endurance volume.
 

While choosing the right tier of Endurance {{site.data.keyword.blockstorageshort}} for your workload it is key, it is equally important to use the block size, Ethernet connection speed, and number of hosts necessary to achieve maximum performance. If any of these parts do not align with the other, it can have a significant impact on the resulting throughput.

 
### Performance

Performance is a class of {{site.data.keyword.blockstorageshort}} that is designed to support high I/O applications with well understood performance requirements that don’t fit well within an Endurance tier. Predictable performance is achieved through the allocation of protocol-level IOPS to individual volumes. IOPS ranging from 100 through 48,000 can be provisioned with storage sizes that range from 20 GB to 12 TB. 

Performance for {{site.data.keyword.blockstorageshort}} is accessed and mounted through a Multipath I/O (MPIO) Internet Small Computer System Interface (iSCSI) connection. {{site.data.keyword.blockstorageshort}} is typically used when the volume will be accessed by a single machine. Multiple volumes can be mounted to a host and striped together to achieve larger volumes and higher IOPS counts. Performance volumes can be ordered according to the sizes and IOPS in Table 1 for Linux, XEN, VMware, and Windows operating systems.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
            <td>6,000 or 20,000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>2,000-3,000</td>
            <td>200</td>
            <td>6,000 or 40,000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>4,000-7,000</td>
            <td>300</td>
            <td>6,000 or 48,000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>8,000-9,000</td>
            <td>500</td>
            <td>6,000 or 48,000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>10,000-12,000</td>
            <td>1,000</td>
            <td>6,000 or 48,000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![footnote](/images/numberone.png)</sup> IOPS limit above 6,000 is available in [select data centers](new-ibm-block-and-file-storage-location-and-features.html).


Performance volumes are designed to perform consistently close to the provisioned IOPS level. Consistency makes it easier to size and scale application environments with a given level of performance. Additionally, given the range of volume sizes and IOPS counts, it becomes possible to optimize an environment by building a volume with the ideal price-to-performance ratio.

 

## Tips for Provisioning IOPS for {{site.data.keyword.blockstorageshort}}

IOPS for both Endurance and Performance is based on a 16 KB block size with a 50/50 read/write 50% random workload. A 16 KB block is the equivalent of one write to the volume.

The block size used by your application will directly impact storage performance.  If the block size used by your application is smaller than 16 KB the IOP limit will be realized prior to the throughput limit.  Conversely, if the block size used by your application is larger than 16KB the throughput limit will be realized prior to the IOP limit.

Changing the block size will affect the performance as follows:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Block Size (KB)</th>
            <th>IOPS</th>
            <th>Throughput (MB/s)</th>
          </tr>
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
            <td>32 (typical for SQLServer)</td>
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

Choosing the {{site.data.keyword.blockstorageshort}} that is right for your workload is important, and equally important is how to avoid bottlenecks. The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. As a general rule you should not expect to saturate your Ethernet connection beyond 70% of the available bandwidth. For example, if you have 6,000 IOPS and are using a 16 KB block size, the volume is capable of approximately 94 MB per second. If you have a 1 Gbps Ethernet connection to your LUN, it will become a bottleneck when your servers attempt to utilize the maximum available throughput because 70% of the theoretical limit of a 1 Gbps Ethernet connection (125 MB per second) would only allow for 88 MB per second.


Another factor to consider is the number of hosts that are utilizing your volume. If there is a single host that is accessing the volume it may be difficult to realize the maximum IOPS available, especially at extreme IOPS counts (10,000s). If your workload requires high throughput it would be best to configure at least two or three servers to access your volume to avoid a single server bottleneck.


To achieve maximum IOPS, adequate network resources need to be in place. Other considerations include private network usage outside of storage and host side and application specific tunings (IP stack, queue depths, and so on).
