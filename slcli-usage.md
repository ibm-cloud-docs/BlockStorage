---

copyright:
  years: 2014, 2025
lastupdated: "2025-11-03"

keywords: SLCLI, API, SLCLI usage, Block Storage, provisioning, ordering, managing

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# SLCLI commands for {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

You can use the SLCLI to accomplish tasks that are normally handled through the [{{site.data.keyword.cloud_notm}} console](/login){: external}. For example, from the SLCLI you can place orders for volumes, snapshot space, replication, update authorizations, or cancel volumes. For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: shortdesc}

## Access-related SLCLI commands
{: #slcliaccess}

* [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage)
   ```sh
   slcli block access-authorize
   slcli block access-list
   slcli block access-password
   slcli block access-revoke
   slcli subnets-assign
   slcli subnets-list
   slcli subnets-remove
   ```
   {: codeblock}

## Replication-related SLCLI commands
{: #slclireplica}

* [Replication-related SLCLI commands](/docs/BlockStorage?topic=BlockStorage-replication)
   ```sh
   slcli block access-revoke
   slcli block replica-failback
   slcli block replica-failover
   slcli block replica-locations
   slcli block replica-order
   slcli block replica-partners
   ```
   {: codeblock}

## Snapshots-related SLCLI commands
{: #slclisnaps}

* [Ordering Snapshots](/docs/BlockStorage?topic=BlockStorage-orderingsnapshots#orderingsnapthroughSLCLI)
   ```sh
   slcli block snapshot-order
   ```
   {: codeblock}

* [Managing Snapshots](/docs/BlockStorage?topic=BlockStorage-managingSnapshots)
   ```sh
   slcli block snapshot-create
   slcli block snapshot-list
   slcli block snapshot-restore
   slcli block snapshot-cancel
   slcli block snapshot-delete
   slcli block snapshot-disable
   slcli block snapshot-enable
   ```
   {: codeblock}

## Volume-related SLCLI commands
{: #sliclivolume}

* [Ordering a {{site.data.keyword.blockstorageshort}} volume](https://cloud.ibm.com/docs/BlockStorage?topic=BlockStorage-orderingBlockStorage&interface=cli#orderingthroughCLI)
   ```sh
   slcli block volume-order
   ```
   {: codeblock}

* [Adjusting the IOPS](/docs/BlockStorage?topic=BlockStorage-adjustingIOPS)
   ```sh
   slcli block volume-modify VOLUME_ID
   ```
   {: codeblock}

* [Expanding the capacity](/docs/BlockStorage?topic=BlockStorage-expandingcapacity)
   ```sh
   slcli block volume-modify VOLUME_ID
   ```
   {: codeblock}

* [Managing {{site.data.keyword.blockstorageshort}}](/docs/BlockStorage?topic=BlockStorage-managingstorage)
   ```sh
   slcli block volume-cancel VOLUME_ID
   slcli block volume-count
   slcli block volume-detail VOLUME_ID
   slcli block volume-list
   slcli block volume-set-lun-id VOLUME_ID
   ```
   {: codeblock}

* [Creating and managing duplicate volumes](/docs/BlockStorage?topic=BlockStorage-duplicatevolume)
   ```sh
   slcli block volume-duplicate ORIGIN_VOLUME_ID
   slcli block volume-duplicate ORIGIN_VOLUME_ID --dependent-duplicate TRUE
   slcli block volume-refresh DUPLICATE_VOLUME_ID ORIGIN_VOLUME_ID
   slcli block volume-convert DUPLICATE_VOLUME_ID
   slcli block duplicate-convert-status DUPLICATE_VOLUME_ID
   ```
   {: codeblock}

* [Managing storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits)
   ```sh
   slcli block volume-limit
   slcli block volume-count
   ```
   {: codeblock}
