---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-30"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}
{:shortdesc: .shortdesc}
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}

# Creating and managing duplicate volumes
{: #duplicatevolume}

You can create a duplicate of an existing {{site.data.keyword.blockstoragefull}}. The duplicate volume inherits the capacity and performance options of the original volume by default and has a copy of the data up to the point-in-time of a snapshot. The duplicate volume can be dependent or independent from the original volume.
{: shortdesc}

Because the duplicate is based on the data in a point-in-time snapshot, snapshot space is required on the original volume before you can create a duplicate. For more information about snapshots and how to order snapshot space, see the [Snapshot documentation](/docs/BlockStorage?topic=BlockStorage-snapshots).
{: important}

**Independent duplicates** can be created from both **primary** and **replica** volumes. The new duplicate is created in the same data center as the original volume. If you create a duplicate from a replica volume, the duplicate volume is created in the same data center as the replica volume.

**Dependent duplicate** volumes are created by using a snapshot from the primary volume. Replica volumes cannot be used to create or update dependent duplicate volumes.

All duplicate volumes can be accessed by a host for read and write operations as soon as the volume is provisioned. 

Dependent duplicate can be refreshed from new snapshots of the parent volume manually immediately after their creation. The dependent duplicate volume keeps the original snapshot locked so the snapshot cannot be deleted while the dependent duplicate exists.

However, snapshots and replication of independent duplicate volumes aren't allowed until the data copy from the original to the duplicate is complete and the duplicate volume is fully independent from the parent volume. Depending on the size of the data, the separation process can take several hours. When it's complete, the duplicate can be managed and used as an independent volume.

This feature is available in most locations. For more information, see [the list of available data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

If you are a Dedicated account user of {{site.data.keyword.containerlong}}, see your options for duplicating a volume in the [{{site.data.keyword.containerlong_notm}} documentation](/docs/containers?topic=containers-block_storage#block_backup_restore).
{: tip}

Some common uses for a duplicate volume:
- **Disaster Recovery Testing**. Create a duplicate of your replica volume to verify that the data is intact and can be used if a disaster occurs, without interrupting the replication.
- **Golden Copy**. Use a storage volume as golden copy that you can create multiple instances from for various uses.
- **Data refreshes**. Create a copy of your production data to mount to your non-production environment for testing.
- **Restore from Snapshot**. Restore data on the original volume with specific files and date from a snapshot without overwriting the entire original volume with the snapshot restore function.
- **Development and Testing (dev/test)**. Create up to four simultaneous duplicates of a volume at one time to create duplicate data for development and testing.
- **Storage Resize**. Create a volume with new size, IOPS rate or both without needing to move your data.


## Creating a duplicate volume in the UI
{: #cloneLUNinUI}
{: ui}

You can create an independent duplicate volume through the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external} in a couple of ways. However, you can provision dependent duplicate volumes only from the CLI.

### Creating a duplicate from the Storage List in the UI
{: #cloneLUN1UI}
{: ui}

1. Go to your list of {{site.data.keyword.blockstorageshort}} in the {{site.data.keyword.cloud_notm}} console by clicking **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select a volume from the list and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") > **Duplicate Volume**.
3. Select the snapshot option to be used to create the duplicate. You can choose an existing Snapshot or take a new one.
4. The location entries remain the same as the original volume.
5. Hourly or Monthly Billing – you can choose to provision the duplicate LUN with hourly or monthly billing. The billing type for the original volume is automatically selected. If you want to choose a different billing type for your duplicate storage, you can make that selection here.
6. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.blockstorageshort}} can be resized to 10 times the original size of the volume.
   {: tip}

7. You can update the snapshot space for the new volume to add more, less, or no snapshot space.
8. You can select the OS Type to be different than the original volume or to stay the same.
9. You can specify IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
   - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
   - If your original volume is 2, 4, or 10 IOPR Endurance tier, you can move anywhere between those tiers for the new volume.

10. Check the box if you read and agreed to the terms and conditions.
11. Click **Create** to continue.

### Creating a duplicate from the Volume detail page the UI
{: #cloneLUN2UI}
{: ui}

1. Go to your list of {{site.data.keyword.blockstorageshort}}.
2. Click a LUN from the list to view the details page. (It can either be a replica or non-replica volume.)
3. Click **Actions**  ![Actions icon](../icons/action-menu-icon.svg "Actions")> **Duplicate Volume**.   
4. Select the snapshot option to be used to create the duplicate. You can choose an existing Snapshot or take a new one.
5. The location entries remain the same as the original volume.
6. Hourly or Monthly Billing – you can choose to provision the duplicate LUN with hourly or monthly billing. The billing type for the original volume is automatically selected. If you want to choose a different billing type for your duplicate storage, you can make that selection here.
7. You can update the size of the new volume so that it's larger than the original. The size of the original volume is set by default.

   {{site.data.keyword.blockstorageshort}} can be resized to 10 times the original size of the volume.
   {: tip}

8. You can update the snapshot space for the new volume to add more, less, or no snapshot space.
9. You can select the OS Type to be different than the original volume or to stay the same.
10. You can specify IOPS or IOPS Tier for the new volume if you want to. The IOPS designation of the original volume is set by default. Available Performance and size combinations are displayed.
    - If your original volume is 0.25 IOPS Endurance tier, you can't make a new selection.
    - If your original volume is 2, 4, or 10 IOPR Endurance tier, you can move anywhere between those tiers for the new volume.

11. Check the box if you read and agreed to the terms and conditions.
12. Click **Create** to continue.


## Creating a duplicate LUN from the SLCLI
{: #cloneinCLI}
{: cli}

The commands that are described in the article are part of the SLCLI. For more information about how to install and use the SLCLI, see [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{: tip}

To create an **independent duplicate** {{site.data.keyword.blockstorageshort}} volume, you can use the following command.

```python
# slcli block volume-duplicate --help
Usage: slcli block volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate block volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Potential Sizes:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] Minimum: [the size of the
                                  origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin block volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```
{: codeblock}

**Dependent duplicate** volumes can be ordered from the SLCLI, too, with the option `--dependent-duplicate TRUE`.

```python
slcli block volume-duplicate --dependent-duplicate TRUE <primary-vol-id>
```

## Managing your duplicate volume
{: #manageduplicatevol}

While data is being copied from the original volume to the independent duplicate, you can see a status on the details page that shows the duplication is in progress. During this time, you can attach to a host, and read and write to the volume, but you can't create snapshot schedules or perform a refresh. When the separation process is complete, the new volume is independent from the original and can be managed with snapshots and replication as normal, and can be manually refreshed by using a snapshot from the parent volume.

Dependent duplicates do not go through the separation process and can be refreshed manually at any time. The refresh process is initiated from the CLI. Later, if you want to convert the dependent duplicate into an independent volume, you can initiate that process from the CLI, too.

## Updating data on the duplicate from the parent volume from the CLI
{: #refreshindependentvol}
{: cli}

As time passes and the primary volume changes, the duplicate volume can be updated with these changes to reflect the current state through the refresh action. The refresh involves taking a snapshot of the primary volume and then, updating the duplicate volume by using that snapshot. 

Refreshes can be performed by using the following command.
```python
slcli block volume-refresh <duplicate-vol-id> <primary-snapshot-id>
```

A refresh incurs no downtime on the primary volume. However, during the refresh transaction, the duplicate volume is unavailable and must be remounted after the refresh is completed.
{: important}

## Converting a dependent volume to an independent duplicate
{: #convertdependentvol}
{: cli}

If you want to use the dependent volume as a stand-alone volume in the future, you can convert it to a normal, independent {{site.data.keyword.blockstoragefull}} volume through the SLCLI by using the following command.

```python
slcli block volume-convert <dependent-vol-id>
```

The conversion process can take some time to complete. The bigger the volume, the longer it takes to convert it. You can check on the status of the process by using the following command.

```python
slcli block duplicate-convert-status <dependent-vol-id>
```

Example output:
```python
slcli block duplicate-convert-status 370597202
Username            Active Conversion Start Timestamp   Completed Percentage
SL02SEVC307608_74   2022-06-13 14:59:17                 90
```

## Canceling a storage volume with a dependent duplicate
{: #cancelvolwithdependent}

Canceling a parent volume that has active dependent volumes requires canceling the dependent duplicate volumes first.