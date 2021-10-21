---

copyright:
  years: 2014, 2021
lastupdated: "2021-04-28"

keywords: SLCLI, API, SLCLI usage, Block Storage, provisioning, ordering, managing

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}
{:shortdesc: .shortdesc}

# SLCLI commands for {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

You can use the SLCLI to take actions that are normally handled through the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}. For example, from the SLCLI you can place orders for volumes, snapshot space, replication, update authorizations, or cancel volumes.
{: shortdesc}

For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

## Access-related SLCLI commands
* [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage)  
   ```python
   slcli block access-authorize
   slcli block access-list
   slcli block access-password
   slcli block access-revoke
   slcli subnets-assign
   slcli subnets-list
   slcli subnets-remove
   ```

## Replication-related SLCLI commands

* [Replication-related SLCLI commands](/docs/BlockStorage?topic=BlockStorage-replication)
   ```python
   slcli block access-revoke
   slcli block replica-failback
   slcli block replica-failover
   slcli block replica-locations
   slcli block replica-order
   slcli block replica-partners
   ```

## Snapshots-related SLCLI commands

* [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots#ordersnapshotSLCLI)
    ```python
   slcli block snapshot-order
    ```

* [Managing Snapshots](/docs/BlockStorage?topic=BlockStorage-managingSnapshots)
   ```python
   slcli block snapshot-create
   slcli block snapshot-list
   slcli block snapshot-restore
   slcli block snapshot-cancel
   slcli block snapshot-delete
   slcli block snapshot-disable
   slcli block snapshot-enable
   ```

## Volume-related SLCLI commands

* [Ordering a {{site.data.keyword.blockstorageshort}} volume](/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage#orderingthroughCLI)
   ```python
   slcli block volume-order
   ```

* [Creating an independent duplicate volume](/docs/BlockStorage?topic=BlockStorage-duplicatevolume)
    ```python
   slcli block volume-duplicate
   ```

* [Creating and managing a dependent duplicate volume](/docs/BlockStorage?topic=BlockStorage-dependentduplicate)
   ```python
   slcli block volume-duplicate --dependent-duplicate TRUE <independent-vol-id>
   slcli block volume-refresh <dependent-vol-id> <independent-snapshot-id>
   slcli block volume-convert <dependent-vol-id>
   ```

* [Adjusting the IOPS](/docs/BlockStorage?topic=BlockStorage-adjustingIOPS)
   ```python
   slcli block volume-modify
   ```

* [Expanding the capacity](/docs/BlockStorage?topic=BlockStorage-expandingcapacity)
   ```python
   slcli block volume-modify
   ```
  
* [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage)  
   ```python
   slcli block volume-cancel
   slcli block volume-count
   slcli block volume-detail
   slcli block volume-list
   slcli block volume-set-lun-id
   ```

* [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits)  
   ```python
   slcli block volume-limit
   slcli block volume-count
   ```
