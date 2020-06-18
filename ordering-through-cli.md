---

copyright:
  years: 2014, 2019
lastupdated: "2019-11-14"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}

# Ordering {{site.data.keyword.blockstorageshort}} through the SLCLI
{: #orderingthroughCLI}

You can use the SLCLI to place orders for products that are normally ordered through the [{{site.data.keyword.cloud_notm}} console](https://{DomainName}/){: external}. In the SL API, an order can consist of multiple order containers. The order CLI works with one order container only.
{:shortdesc}

For more information about how to install and use the SLCLI, see [Python CLI Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Searching for available {{site.data.keyword.blockstorageshort}} offers

The first component to look for when you place an order is a package. Packages are divided among the different top-level products that are available for ordering in the {{site.data.keyword.cloud}}. Some example packages are CLOUD_SERVER for VSIs, BARE_METAL_SERVER for bare metal servers, and STORAGE_AS_A_SERVICE_STAAS for {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}}.

Within a package, some items are subdivided into categories. Some packages have presets for your convenience and others require items to be specified individually. If a package's category is required, an item from that category must be chosen to order the package. Depending on the category, some items within the category can be mutually exclusive.

Each order must have an associated location (data center). When you order {{site.data.keyword.blockstorageshort}}, make sure that it is provisioned in the same location as your compute instances.
{:important}

You can use the `slcli order package-list` command to find the package that you want to order. A `–keyword` option is provided to perform simple searching and filtering. This option makes it easier to find the package you need. Look for **Storage-as-a-Service Package 759**.

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

You can also use the `slcli block volume-order` command.

```
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Order a block storage volume.

Options:
 --storage-type [performance|endurance]
                                 Type of block storage volume  [required]
 --size INTEGER                  Size of block storage volume in GB.
                                 Permitted Sizes:
                                 20, 40, 80, 100, 250, 500,
                                 1000, 2000, 4000, 8000, 12000  [required]
 --iops INTEGER                  Performance Storage IOPs, between 100 and
                                 6000 in multiples of 100  [required for
                                 storage-type performance]
 --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                 [required for storage-type endurance]
 --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                 Operating System  [required]
 --location TEXT                 Datacenter short name (e.g.: dal09)
                                 [required]
 --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                 space along with endurance block storage;
                                 specifies the size (in GB) of snapshot space
                                 to order
 --service-offering [storage_as_a_service|enterprise|performance]
                                 The default is 'storage_as_a_service'
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

For more information about ordering {{site.data.keyword.blockstorageshort}} through the API, see [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
To be able to access all the new features, order `Storage-as-a-Service Package 759`.
{:tip}

For more information about Window OS types, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).


## Placing the order

The following example shows how to order an 80-GB {{site.data.keyword.blockstorageshort}} volume with 20-GB Snapshot space and 0.25 IOPS per GB.

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

By default, you can provision a combined total of 250 {{site.data.keyword.blockstorageshort}} and {{site.data.keyword.filestorage_short}} volumes. To increase the number of your volumes, contact your sales representative. For more information about increasing limits, see [Managing Storage limits](/docs/BlockStorage?topic=BlockStorage-managingstoragelimits).
{:important}

## Authorizing the hosts to access the new storage

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account. Also, a host cannot be authorized to access multiple LUNs of differing OS types at the same time. A host can be authorized to access LUNs of a **single** OS type. If you attempt to authorize a host to access multiple LUNs with different OS types, the operation results in an error.
{:note}
{:important}

For more information about authorizing hosts to access the {{site.data.keyword.blockstorageshort}} through the API, see [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}.
{:tip}

For the limit on simultaneous authorizations, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs).
{:important}


## Connecting your new storage
{: #mountingCLI}

Depending on your host's operating system, follow the appropriate link.
- [Connecting to LUNs on Linux](/docs/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connecting to LUNs on CloudLinux](/docs/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connecting to LUNS on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuring {{site.data.keyword.blockstorageshort}} for backup with cPanel](/docs/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuring {{site.data.keyword.blockstorageshort}} for backup with Plesk](/docs/BlockStorage?topic=BlockStorage-PleskBackups)
