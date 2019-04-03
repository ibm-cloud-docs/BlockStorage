---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, accessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Disaster Recovery - Fail over with an accessible Primary volume
{: #dr-accessible}

If a catastrophic failure or disaster occurs on the primary site and the primary storage is still accessible, customers can perform the following actions to quickly access their data on the secondary site.

Before you start the failover, make sure that all host-authorization is in place.

Authorized hosts and volumes must be in the same data center. For example, you can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.
{:note}

1. Log in to [The {{site.data.keyword.cloud}} console ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/catalog/){:new_window} and click the **menu** icon on the upper left. Select **Classic Infrastructure**.

   Alternatively, you can log in to the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
2. Click your source or destination volume from the **{{site.data.keyword.blockstorageshort}}** page.
3. Click **Replica**.
4. Scroll down to the **Authorize Hosts** frame and click **Authorize Hosts** on the right.
5. Highlight the host that is to be authorized for replications. To select multiple hosts, use the CTRL-key and click the applicable hosts.
6. Click **Submit**. If you have no hosts that are available, you are prompted to purchase compute resources in the same data center.


## Starting a failover from a volume to its replica

If a failure event is imminent, you can start a **failover** to your destination, or target, volume. The target volume becomes active. The last successfully replicated snapshot is activated, and the volume is made available for mounting. Any data that was written to the source volume since the previous replication cycle is lost. When a failover is started, the replication relationship is flipped. Your target volume becomes your source volume, and your former source volume becomes your target as indicated by the **LUN Name** followed by **REP**.

Failovers are started under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [[{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

**Before you proceed with these steps, disconnect the volume. Failure to do so, results in corruption and data loss.**

1. Click your active LUN (“source”).
2. Click **Replica** and click **Actions**.
3. Select **Failover**.

   Expect a message across the page that states that the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the failover process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.<br/> When your target volume is live, you get another message. Your original source volume's LUN Name updates to end in "REP" and its Status becomes Inactive.
   {:note}
4. Click **View All ({{site.data.keyword.blockstorageshort}})**.
5. Click your active LUN (formerly your target volume).
6. Mount and attach your storage volume to the host. Click [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) for instructions.


## Starting a failback from a volume to its replica

When your original source volume is repaired, you can start a controlled Failback to your original source volume. In a controlled Failback,

- The acting source volume is taken offline,
- A snapshot is taken,
- The replication cycle is completed,
- The just-taken data snapshot is activated,
- And the source volume becomes active for mounting.

When a Failback is started, the replication relationship is flipped again. Your source volume is restored as your source volume, and your target volume is the target volume again as indicated by the **LUN Name** followed by **REP**.

Failbacks are started under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

1. Click your active LUN ("target").
2. In the upper right, click **Replica** and click **Actions**.
3. Select **Failback**.

   Expect a message across the page that shows the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   {:note}
4. In the upper right, click **View All {{site.data.keyword.blockstorageshort}}** link.
5. Click your active LUN ("source").
6. Mount and attach your storage volume to the host. Click [here](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) for instructions.
