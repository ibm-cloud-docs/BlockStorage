---

copyright:
  years: 2014, 2019
lastupdated: "2020-01-07"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Managing {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

You can manage your {{site.data.keyword.blockstoragefull}} volumes through the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}. From the **menu**, select **Classic Infrastructure** to interact with classic services.
{:shortdesc}

## Viewing {{site.data.keyword.blockstorageshort}} LUN details
{: #viewLUNdeetsUI}
{: help}
{: support}

You can view a summary of the key information for the selected storage LUN including extra snapshot and replication capabilities that were added to the storage.

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Click the appropriate Volume name from the list.

Alternatively, you can use the following command in SLCLI.
```
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## Authorizing hosts to access to {{site.data.keyword.blockstorageshort}}
{: #authhostUI}
{: help}
{: support}

"Authorized" hosts are hosts that were given access to a particular LUN. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your LUN generates the user name, password, and iSCSI qualified name (IQN), which are needed to mount the multipath I/O (MPIO) iSCSI connection.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{:important}

2. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Locate the volume and click the ellipsis (**...**).
4. Click **Authorize Host**.
5. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device type or subnets.
   - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
   - If you choose IP address, first, select the subnet where your host resides.
6. From the filtered list, select one or more hosts that can access the volume and click **Save**.

Alternatively, you can use the following command in SLCLI.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} LUN
{: #viewauthhostUI}
{: help}
{: support}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and click your Volume name.
2. Scroll down to the **Authorized Hosts** section.

There you can see the list of hosts, which are currently authorized to access the LUN. You can also see the authentication information that is needed to make a connection – user name, password, and IQN Host. The Target address is listed on the **Storage Detail** page. For NFS, the Target address is described as a DNS name, and for iSCSI, it's the IP address of the Discover Target Portal.

Alternatively, you can use the following command in SLCLI.
```
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## Viewing the {{site.data.keyword.blockstorageshort}} to which a host is authorized
{: #viewLUNauthUI}
{: help}
{: support}

You can view the LUNs to which a host has access to, including information that is needed to make a connection – LUN Name, Storage Type, Target Address, capacity and location:

1. Click **Devices** -> **Device List** in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external} and click the appropriate device.
2. Select the **Storage** tab.

You're presented with a list of storage LUNs that this particular host has access to. The list is grouped by storage type (block, file, other). You can authorize more storage or remove access by clicking **Actions**.

A host cannot be authorized to access LUNs of differing OS types at the same time. A host can be authorized to access LUNs of a **single** OS type. If you attempt to authorize a host to access multiple LUNs with different OS types, the operation results in an error.
{:note}

## Mounting and unmounting {{site.data.keyword.blockstorageshort}}
{: #mountLUNUI}
{: help}
{: support}

Based on the Operating System of your host, follow the appropriate instructions.

- [Connecting to LUNs on Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connecting to LUNs on CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connecting to LUNS on Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuring Block Storage for backup with cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuring Block Storage for backup with Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## Revoking a host's access to {{site.data.keyword.blockstorageshort}}

If you want to stop the access from a host to a particular storage LUN, you can revoke the access. Upon revoking access, the host connection is dropped from the LUN. The operating system and applications on that host can't communicate with the LUN anymore.

To avoid host side issues, unmount the storage LUN from your operating system before you revoke the access to avoid missing drives or data corruption.
{:important}

You can revoke access from the **Device List** or the **Storage view**.

### Revoking access from the Device List
{: #revokeDeviceUI}
{: help}
{: support}

1. In the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}, click the Classic Infrastructure icon. Then, click **Devices**, **Device List** and double-click the appropriate device.
2. Select the **Storage** tab.
3. You are presented with a list of storage LUNs that this particular host has access to. The list is grouped by storage type (block, file, other). Next to the Volume name, click **Actions**, and click **Revoke Access**.
4. Confirm that you want to revoke the access for a LUN because the action can't be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

If you want to disconnect multiple LUNs from a specific host, you need to repeat the Revoke Access action for each LUN.
{:tip}


### Revoking access from the Storage View
{: #revokeDStorageUI}
{: help}
{: support}


1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**, and select the LUN from which you want to revoke access.
2. Scroll to **Authorized Hosts**.
3. Click **Actions** next to the host whose access is to be revoked, and select **Revoke Access**.
4. Confirm that you want to revoke the access for a LUN because the action can't be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific LUN, you need to repeat the Revoke Access action for each host.
{:tip}

### Revoking access through the SLCLI.
{: #revokeSLCLI}
{: help}
{: support}


Alternatively, you can use the following command in SLCLI.
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of a virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of an IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  --help                    Show this message and exit.
```

## Canceling a storage LUN
{: #rcancelLUNUI}
{: help}
{: support}


If you no longer need a specific LUN, you can cancel it at any time.

To cancel a storage LUN, it's necessary to revoke access from any hosts first.
{:important}

1. Click **Storage**, **{{site.data.keyword.blockstorageshort}}**.
2. Select the volume to be canceled, click **Actions**, and select **Cancel {{site.data.keyword.blockstorageshort}}**.
3. Confirm if want to cancel the LUN immediately or on the anniversary date of when the LUN was provisioned.

   If you select the option to cancel the LUN on its anniversary date, you can void the cancellation request before its anniversary date.
   {:tip}
4. Click **Continue** or **Close**.
5. Click the **Acknowledgment** check box and click **Confirm**.

Alternatively, you can use the following command in SLCLI.
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```

You can expect the LUN to remain visible in your Storage list for at least 24 hours (immediate cancellation) or until the anniversary date. Certain features aren't going to be available any longer, but the volume remains visible until it's reclaimed. However, billing is stopped immediately after you click Delete/Cancel.

Active replicas can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, and replication is canceled before you attempt to cancel the original volume.
