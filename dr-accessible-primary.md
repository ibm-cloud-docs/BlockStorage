---

copyright:
  years: 2018, 2024
lastupdated: "2024-07-25"

keywords: Block Storage for Classic, accessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Disaster Recovery - Fail over from an accessible primary volume
{: #dr-accessible}

If a failure occurs on the primary site, and performance is degrading while the primary storage is still accessible, customers can reroute their operations to the secondary site by initiating a failover.
{: shortdesc}

Before you start the failover, make sure that the required host-authorization is in place.
{: important}

Authorized hosts and volumes must be in the same data center. For example, you can't have a replica volume in London and the host in Amsterdam. Both must be in London or both must be in Amsterdam.
{: note}

## Authorizing the host
{: #authreplicahost}

Before you begin, make sure that the host that is to access the {{site.data.keyword.blockstorageshort}} volume is authorized. For more information, see [Authorizing the host in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#authhostUI){: ui}[Authorizing the host from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#authhostCLI){: cli}[Authorizing the host with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#authhostTerraform){: terraform}.
{: requirement}

## Starting a failover from a volume to its replica
{: #failovertoreplica}

If a failure event is imminent, you can start an **Immediate failover** or a "Controlled Failover" to your destination, or target, volume.

When you choose an Immediate Failover, the last successfully replicated snapshot is activated, and the volume is made available for mounting. The target volume becomes active in less time compared to a Controlled Failover. However, any data that was written to the source volume since the previous replication cycle is lost.

A Controlled Failover is the best choice when you want to test the failover function. It's also the best option when it’s more important to continue operations at the replica location with the most recent data. In a Controlled Failover, a new snapshot is taken and copied over to the replica location. After the data is successfully copied over, the volume is made available for mounting.

When a failover is started, the replication relationship is flipped. Your target volume becomes your source volume, and your former source volume becomes your target as indicated by the **Volume Name** followed by **REP**.

Before you proceed with these steps, disconnect the volume. Failure to do so, results in corruption and data loss.
{: important}

## Fail over to replica in the console
{: #failovertoreplicaUI}
{: ui}

Failovers are started under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [[{{site.data.keyword.cloud}} console](/classic-gen1){: external}.

1. Click your active LUN (“source”).
2. Click **Replica**, and click **Actions**.
3. Select **Controlled Failover** or **Immediate Failover**.

   Expect a message across the page that states that the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** page that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the failover process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history. When your target volume is live, you get another message. Your original source volume's LUN Name updates to end in "REP" and its Status becomes Inactive.
   {: note}

4. Click **View All {{site.data.keyword.blockstorageshort}}**.
5. Click your active LUN (it was your previous target volume).
6. Mount and attach your storage volume to the host. For more information, see [Connecting your storage](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui#mountingnewLUN).

## Fail over to replica from the CLI
{: #failovertoreplicaCLI}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Initiating a failover from the IBMCLOUD CLI
{: #failovertoreplicaICCLI}

You can use the `ibmcloud sl block replica-failover` command to fail over operations from the source volume to the replica volume. The following example initiates a failover from the source share `560156918` to the replica share `560382016`.

```sh
$ ibmcloud sl block replica-failover 560156918 560382016
OK
Failover of volume 560156918 to replica 560382016 is now in progress.
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block replica-failover](/docs/cli?topic=cli-sl-block-storage#sl_block_replica_failover){: external}.

### Initiating a failover from the SLCLI
{: #failovertoreplicaSLCLI}

To fail over a block volume to a specific replicant volume, use the following command.

   ```sh
   $ slcli block replica-failover --help
   Usage: slcli block replica-failover [OPTIONS] VOLUME_ID

   Options:
   --replicant-id TEXT  ID of the replicant volume
   --immediate          Failover to replicant immediately.
   -h, --help           Show this message and exit.
   ```
   {: screen}

During the failover process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history. When your target volume is live, you get another message. Your original source volume's Status becomes Inactive.
{: note}

Mount and attach your storage volume to the host. For more information, see [Connecting your storage](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=cli#mountingnewLUN).

## Starting a failback from a volume to its replica
{: #failbackfromreplica}

When your original source volume is repaired, you can start a controlled Failback to your original source volume. In a controlled Failback,

- The acting source volume is taken offline.
- A snapshot is taken.
- The replication cycle is completed.
- The just-taken data snapshot is activated.
- The source volume becomes active for mounting.

When a Failback is started, the replication relationship is flipped again. Your source volume is restored as your source volume, and your target volume is the target volume again as indicated by the **LUN Name** followed by **REP**.

## Failback in the console
{: #failbackfromreplicaUI}
{: ui}

Failbacks are started under **Storage**, **{{site.data.keyword.blockstorageshort}}** in the [{{site.data.keyword.cloud}} console](/cloud-storage/block){: external}.

1. Click your active LUN ("target").
2. Next, click **Replica**, and click **Actions**.
3. Select **Failback**.

   Expect a message across the page that shows that the failover is in progress. Additionally, an icon appears next to your volume on the **{{site.data.keyword.blockstorageshort}}** that indicates that an active transaction is occurring. Hovering over the icon produces a window that shows the transaction. The icon disappears when the transaction is complete. During the Failback process, configuration-related actions are read-only. You can't edit any snapshot schedule or change snapshot space. The event is logged in replication history.
   {: note}

4. Next, click **View All {{site.data.keyword.blockstorageshort}}**.
5. Click your active LUN ("source").
6. Mount and attach your storage volume to the host. For more information, see [Connecting your storage](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=ui#mountingnewLUN).

## Failing back from the CLI
{: #failbackfromreplicaCLI}
{: cli}

### Initiating a failback from the IBMCLOUDCLI
{: #failbackfromreplicaICCLI}

You can use the `ibmcloud sl block replica-failback` command to fail back operations from the replica volume to the original source volume. The following example initiates a failback to the original source share `560156918`.

```sh
$ ibmcloud sl block replica-failback 560156918
OK
Failback of volume 560156918 is now in progress.
```
{: codeblock}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block replica-failback](/docs/cli?topic=cli-sl-block-storage#sl_block_replica_failback){: external}.

### Initiating a failback from the SL CLI
{: #failbackfromreplicaSLCLI}

To fail back a block volume from a specific replicant volume.
```sh
$ slcli block replica-failback --help
Usage: slcli block replica-failback [OPTIONS] VOLUME_ID

Options:
 --replicant-id TEXT  ID of the replicant volume
 -h, --help           Show this message and exit.
```
{: screen}

Mount and attach your storage volume to the host. For more information, see [Connecting your storage](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=cli#mountingnewLUN).
