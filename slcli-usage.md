---

copyright:
  years: 2014, 2020
lastupdated: "2020-01-27"

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

You can use the SLCLI to take actions that are normally handled through the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}. For example, with SLCLI you can place orders for volumes, snapshot space, replication, update authorizations, or cancel volumes.
{:shortdesc}

For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Access-related SLCLI commands
* [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  slcli subnets-assign
  slcli subnets-list
  slcli subnets-remove
  ```

## Replication-related SLCLI commands

* [Replication-related SLCLI commands](/docs/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## Snapshots-related SLCLI commands

* [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots#ordersnapshotSLCLI)
  ```
  slcli block snapshot-order
  ```

* [Managing Snapshots](/docs/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## Volume-related SLCLI commands

* [Ordering a {{site.data.keyword.blockstorageshort}} volume](/docs/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [Creating a duplicate volume](/docs/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [Adjusting the IOPS](/docs/BlockStorage?topic=BlockStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli block volume-modify
  ```
* [Expanding the capacity](/docs/BlockStorage?topic=BlockStorage-expandingcapacity#resizingsteps)
  ```
  slcli block volume-modify
  ```
* [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-limit
  slcli block volume-count
  ```
