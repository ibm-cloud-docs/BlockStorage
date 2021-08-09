---

copyright:
  years: 2014, 2020
lastupdated: "2020-09-10"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}


# Creating and managing dependent duplicate volumes
{: #dependentduplicate}

With the new dependent volume feature, {{site.data.keyword.cloud}} customers are able to create volume duplicates and refresh the data on the dependent volume by using a snapshot from the primary volume whenever they want to. Replica volumes cannot be used to create or update dependent duplicate volumes.
{: shortdesc}

If you would like to know about creating a duplicate volume that is independent from the original volume, see [Creating and managing independent duplicate volumes](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).
{: note}

The commands described in the article are part of the SLCLI. For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

## Ordering a dependent volume
{: #orderdependentvol}

Dependent duplicate volumes can be ordered through the SLCLI.
```
slcli block volume-duplicate --dependent-duplicate TRUE <primary-vol-id>
```

## Updating data on the dependent duplicate volume
{: #refreshdependentvol}

As time passes and the primary volume changes, the dependent volume can be updated with these changes to reflect the current state through the refresh action. The data on the dependent volume can be refreshed at any time. The refresh involves taking a snapshot of the primary volume and then, updating the dependent volume by using that snapshot. A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the dependent volume is unavailable and must be remounted after the refresh is completed.

Refreshes can be performed by using the SLCLI.
```
slcli block volume-refresh <dependent-vol-id> <primary-snapshot-id>
```
## Converting a dependent volume to a normal, independent volume
{: #convertdependentvol}

If you want to use the dependent volume as a stand-alone volume in the future, you can convert it to a normal, independent {{site.data.keyword.blockstoragefull}} volume through the SLCLI.

```
slcli block volume-convert <dependent-vol-id>
```

## Canceling a storage volume with a dependent duplicate
{: #cancelvolwithdependent}

Canceling an independent volume that has currently active dependent volumes requires canceling the dependent, duplicate volumes first.
