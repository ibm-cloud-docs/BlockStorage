---

copyright:
  years: 2014, 2022
lastupdated: "2022-07-06"

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

To achieve maximum IOPS, adequate network resources need to be in place. 

* **Run storage traffic on a dedicate VLAN.** Running storage traffic through software firewalls increases latency and adversely affects storage performance. It's best to run storage traffic on a VLAN, which bypasses the firewall. For more information, see [routing storage traffic to its own VLAN interface](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs&interface=ui#howtoisolatedstorage).

* **Avoid routing your storage traffic to a gateway device** whenever possible. When storage traffic is routed to a gateway device, this can add latency to storage traffic or it can cause storage traffic disruption if the firewall in the gateway device is misconfigured. The storage disruption is especially true when a maintenance such as a reboot is required on a single (non-clustered) gateway device. If a storage traffic must be routed through a gateway device, ensure that  the gateway device has an at least 10-Gbps interface or the gateway device might becomes a network bottleneck.

* **Use a faster NIC.** There are limits set at the LUN level and a faster interface doesn't increase that limit. However, with a slower Ethernet connection, your bandwidth can be a potential hinderance to achieving best performance levels.

* **Choose a higher bandwidth.** The speed of your Ethernet connection must be faster than the expected maximum throughput from your volume. Generally, don't expect to saturate your Ethernet connection beyond 70% of the available bandwidth.

     For example, if you have 6,000 IOPS and are using a 16-KB block size, the volume can handle approximately 94-MBps throughput. However, when you have a 1-Gbps Ethernet connection to your LUN, it becomes a bottleneck when your servers attempt to use the maximum available throughput. It's because 70 percent of the theoretical limit of a 1-Gbps Ethernet connection (125 MB per second) would allow for 88 MB per second only.
     {: note}

## Best Practice 2: Set up multiple paths for redundancy
{: #bestpractice2}

{{site.data.keyword.blockstorageshort}} is built upon best-in-class, proven, enterprise-grade hardware and software to ensure high availability and uptime. The data is stored redundantly across multiple physical disks on HA paired nodes. Each storage node has multiple paths to its own Solid-State Drives and its partner node's SSDs as well. This configuration protects against path failure and controller failure because the node can still access its partner's disks for continued productivity. Redundant network ports and paths protect against network failures across the cloud connections.

* **Do not run iSCSI traffic over 802.3ad LACP port channel.** Link Aggregation Control Protocol (LACP) is not a recommended configuration with iSCSI. 

* **Use Multi-Path Input/Output (MPIO) framework for I/O balancing and redundancy.** With an MPIO configuration, a server with multiple NICs can transmit and receive I/O across all available interfaces to a corresponding MPIO-enabled storage device. This provides redundancy that can ensure that the storage traffic remains steady even if one of the paths becomes unavailable. If a server has two 1-Gb NICs and the storage server has two 1-Gb NICs, the theoretical maximum throughput is about 200 MB/s.

   While it is possible to attach {{site.data.keyword.blockstorageshort}} with only a single path, it is important to establish connections on both paths to ensure no disruption of service. If MPIO isn't configured correctly, your storage device might disconnect and appear offline when a network outage occurs or when {{site.data.keyword.cloud}} teams perform maintenance.
   {: important}

* **Add iSCSI multi-sessions when necessary**. Having multiple sessions per target (MS/T) is a storage performance tuning strategy documented by [Oracle](https://docs.oracle.com/cd/E37838_01/html/E61018/gqgbw.html){: external}. Using MS/T and creating multiple TCP connections ensures better usage of the networking stack. It also ensures better performance by using multiple send and receive threads.  
 
   Add persistent iscsi multi-sessions through the iscsiadm CLI.
     1. List existing sessions. 
        ```zsh
        iscsiadm -m session
        ```

     1. Modify the number of sessions by  using  the following command. This configuration change is persistent when the host is rebooted.
        ```zsh
        iscsiadm -m node -T <IQN> -p <IP> --op update -n node.session.nr_sessions -v <TOTAL_SESSION>
        ```

        EXAMPLE of adding 3 additional sessions (4 total) to target portal 161.26.115.77:3260.
        ```zsh
        iscsiadm -m node -T iqn.1992-08.com.netapp:stfdal1306 -p 161.26.115.77:3260 --op update -n node.session.nr_sessions -v 4
        ```

     1. Log into the portal to establish the extra sessions.
        ```zsh
        iscsiadm -m node -T iqn.1992-08.com.netapp:stfdal1306 -p 161.26.115.77:3260 -l
        ```

     1. List sessions to see the added sessions against the single portal IP.
        ```zsh
        iscsiadm -m session
        ``` 

     1. Log out of the iSCSI session by using the session ID in place of the X in the following command.
        ```zsh
        iscsiadm -m session -r X -u
        ```

## Best Practice 3: Optimize the host and applications
{: #bestpractice3}

* **Use the right i/o scheduler**. I/O schedulers help to optimize disk access requests. They traditionally do this by merging I/O requests. By grouping requests located at similar sections of disk, the drive doesn't need to "seek" as often, improving the overall response time for disk operations. On modern Linux implementations, there are several I/O scheduler options available. Each of these have their own unique method of scheduling disk access requests. 

    - **Deadline** is the default I/O scheduler on Red Hat 7.9, and usually it does not need to be changed to a different I/O scheduler. It's latency-oriented scheduler and it works by creating a separate read queue and sepate a write queue. Each I/O request has a time stamp that is associated with it to be used by the kernel for an expiration time. While this scheduler also attempts to service the queues based on the most efficient ordering possible, the expiration time acts as a "deadline" for each I/O request. When an I/O request reaches its deadline, it is pushed to the highest priority. 

    - **No Operation (NOOP)** is a basic scheduler that simply passes down the I/O that comes to it. This scheduler places all I/O requests into a FIFO (First in, First Out) queue. It's a useful tool for checking whether complex I/O scheduling decisions of other schedulers are causing I/O performance regressions. This scheduler is recommended for setups with devices that do I/O scheduling themselves, such as intelligent storage or in multipathing environments. If you choose a more complicated scheduler on the host, the scheduler of the host and the scheduler of the storage device can compete with each other and decrease performance. The storage device can usually determine best how to schedule I/O. For more information about how to check and configure the I/O scheduler, see Red Hat's [How to use the Noop or None IO Schedulers](https://access.redhat.com/solutions/109223){: external}. 

    - **Completely Fair Queuing (CFQ)** uses both request merging and elevators and is a bit more complex than the NOOP or deadline schedulers. It's the standard scheduler for many Linux distributions. It groups simultaneous requests that are made by operations into a series of per-process pools before allocating timeslices to use the disc for every queue.

   If your work load is dominated by interactive applications, the users might complain of sluggish performance of databases with many I/O operations. In such environments, read operations happen significantly more often than write operations, and applications are more likely to be waiting to read data. You can check the default IO scheduler settings and try different schedulers to ensure optimization for your specific workload. 

* **Tune the I/O queue depth**. Change `/etc/iscsi/iscsid.conf node.session.queue_depth` from the default 32 to 64. Most host bus adapters (HBA) have a default queue depth of around 32, which is usually enough to generate up to the target maximum IOPS. If you have only one path to the LUN then that's the maximum possible number of IOPS, however the same LUN with 2 or more sessions would be able to push more I/O's per second of storage throughput to the target LUN. The flip-side of increasing i/o depth is it adds more latency. To counteract the latency, enable Jumbo Frames. For more information about host queue depth recommendations, see [Adjusting Host Queue settings](/docs/BlockStorage?topic=BlockStorage-hostqueuesettings).

* **[Enable Jumbo Frames](/docs/BlockStorage?topic=FileStorage-jumboframes) and configure them to be the same on the entire network path** from source device <-> switch <-> router <-> switch <-> target device. If the entire chain isn't set the same, it defaults to the lowest setting along the chain. {{site.data.keyword.cloud}} has network devices set to 9,000 currently. For best performance, all customer devices need to be set to the same 9,000 value. 

   Setting MTU to 9000 on your hosts has the following benefits:
    - Data can be transmitted in fewer frames.
    - Per-packet overhead is reduced.
    - Throughput is increased by reducing the number of CPU cycles and instructions for packet processing.
    - Jumbo frames provide less opportunity for packets to arrive out of order or to be lost, resulting in fewer retransmissions. Fewer retransmissions mean less time spent in TCP recovery. The end result is greater throughput.

* **VMware specific best practice for teaming**:  If you plan to use teaming to increase the availability of your network access to the storage array, you must turn off port security on the switch for the two ports on which the virtual IP address is shared. The purpose of this port security setting is to prevent spoofing of IP addresses. Thus, many network administrators enable this setting. However, if you do not change it, the port security setting prevents failover of the virtual IP from one switch port to another and teaming cannot fail over from one path to another. For most LAN switches, the port security is enabled on a port level and thus can be set on or off for each port.

