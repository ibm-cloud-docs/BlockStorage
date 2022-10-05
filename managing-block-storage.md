---

copyright:
  years: 2014, 2022
lastupdated: "2022-03-08"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

You can manage your {{site.data.keyword.blockstoragefull}} volumes through the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), select **Classic Infrastructure** ![Classic icon](../icons/classic.svg "Classic") to interact with classic services.
{: shortdesc}

## Viewing {{site.data.keyword.blockstorageshort}} LUN details in the UI
{: #viewLUNdeetsUI}
{: help}
{: support}
{: ui}

You can view a summary of the key information for the selected storage LUN including extra snapshot and replication capabilities that were added to the storage.

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Click the appropriate Volume name from the list.

## Viewing {{site.data.keyword.blockstorageshort}} LUN details from the SLCLI
{: #viewLUNdeetsCLI}
{: help}
{: support}
{: cli}

To view information about a Storage LUN, you can use the following command from the SLCLI.
```python
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## Authorizing hosts to access {{site.data.keyword.blockstorageshort}} in the UI
{: #authhostUI}
{: help}
{: support}
{: ui}

"Authorized" hosts are hosts that were given access to a particular LUN. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your LUN generates the user name, password, and iSCSI qualified name (IQN), which are needed to mount the multipath I/O (MPIO) iSCSI connection.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Locate the volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
3. Click **Authorize Host**.
4. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device type or subnets.
   - If you choose Devices, you can select from Bare Metal Server or Virtual server instances.
   - If you choose IP address, select the subnet where your host resides.
5. From the filtered list, select one or more hosts that can access the volume and click **Save**.

The default limit for the number of authorizations per block volume is eight. This means that up to 8 hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} LUN. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware deployment can request the authorization limit to be increased to 64. To request a limit increase, contact your sales representative or raise a [Support case](https://{DomainName}/unifiedsupport/cases/add){: external}.
{: note}

## Authorizing hosts to access {{site.data.keyword.blockstorageshort}} from the SLCLI
{: #authhostCLI}
{: help}
{: support}
{: cli}

"Authorized" hosts are hosts that were given access to a particular LUN. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your LUN generates the user name, password, and iSCSI qualified name (IQN), which are needed to mount the multipath I/O (MPIO) iSCSI connection.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

To authorize a host, you can use the following commands in SLCLI.
```python
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

```python
# slcli block subnets-list -h
Usage: slcli block subnets-list [OPTIONS] ACCESS_ID
  List block storage assigned subnets for the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
    -h, --help  Show this message and exit.
```

```python
# slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```

```python
# slcli block subnets-remove -h
Usage: slcli block subnets-remove [OPTIONS] ACCESS_ID
  Remove block storage subnets for the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to remove; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```

The default limit for the number of authorizations per block volume is eight. This means that up to 8 hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} LUN. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware deployment can request the authorization limit to be increased to 64. To request a limit increase, contact your sales representative or raise a [Support case](https://{DomainName}/unifiedsupport/cases/add){: external}.
{: note}

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} LUN in the UI
{: #viewauthhostUI}
{: help}
{: support}
{: ui}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and click your Volume name.
2. Click **Authorized Hosts** to display the list of compute instances that have access to the volume.

There you can see the list of hosts, which are currently authorized to access the LUN. You can also see the authentication information that is needed to make a connection – user name, password, and IQN Host. The Target address is listed on the **Storage Detail** page. For NFS, the Target address is described as a DNS name, and for iSCSI, it's the IP address of the Discover Target Portal.

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} LUN from the SLCLI
{: #viewauthhostCLI}
{: help}
{: support}
{: cli}

To see the list of hosts, which are currently authorized to access the LUN, you can use the following command in SLCLI.

```python
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## Viewing the {{site.data.keyword.blockstorageshort}} to which a host is authorized in the UI
{: #viewLUNauthUI}
{: help}
{: support}
{: ui}

You can view the LUNs to which a host has an access to, including information that is needed to make a connection – LUN Name, Storage Type, Target Address, capacity and location:

1. Click **Devices** > **Device List** in the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external} and click the appropriate device.
2. Select the **Storage** tab.

You're presented with a list of storage LUNs that this particular host has access to. The list is grouped by storage type (block, file, other). You can authorize more storage or remove access by clicking **Actions**.

A host cannot be authorized to access LUNs of differing OS types at the same time. A host can be authorized to access LUNs of a **single** OS type. If you attempt to authorize a host to access multiple LUNs with different OS types, the operation results in an error.
{: note}

## Revoking a host's access to {{site.data.keyword.blockstorageshort}} in the UI
{: #revokeauthinUI}
{: ui}

If you want to stop the access from a host to a particular storage LUN, you can revoke the access. Upon revoking access, the host connection is dropped from the LUN. The operating system and applications on that host can't communicate with the LUN anymore.

To avoid host side issues, unmount the storage LUN from your operating system before you revoke the access to avoid missing drives or data corruption.
{: important}

You can revoke access from the **Device List** or the **Storage view**.

### Revoking access from the Device List
{: #revokeDeviceUI}
{: help}
{: support}
{: ui}

1. In the [{{site.data.keyword.cloud}} console](https://{DomainName}/classic){: external}, click the Classic Infrastructure icon. Then, click **Devices** > **Device List** and double-click the appropriate device.
2. Select the **Storage** tab.
3. You are presented with a list of storage LUNs that this particular host has an access to. The list is grouped by storage type (block, file, other). Next to the Volume name, click **Actions**, and click **Revoke Access**.
4. Confirm that you want to revoke the access for a LUN because the action can't be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

If you want to disconnect multiple LUNs from a specific host, you need to repeat the Revoke Access action for each LUN.
{: tip}


### Revoking access from the Storage View
{: #revokeStorageUI}
{: help}
{: support}
{: ui}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and select the LUN from which you want to revoke access.
2. Click **Authorized Hosts**.
3. Click **Actions**  ![Actions icon](../icons/action-menu-icon.svg "Actions")next to the host whose access is to be revoked, and select **Revoke Access**.
4. Confirm that you want to revoke the access for a LUN because the action can't be undone. Click **Yes** to revoke LUN access or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific LUN, you need to repeat the Revoke Access action for each host.
{: tip}

## Revoking access from the SLCLI.
{: #revokeSLCLI}
{: help}
{: support}
{: cli}

If you want to stop the access from a host to a particular storage LUN, you can revoke the access. Upon revoking access, the host connection is dropped from the LUN. The operating system and applications on that host can't communicate with the LUN anymore.

To avoid host side issues, unmount the storage LUN from your operating system before you revoke the access to avoid missing drives or data corruption.
{: important}

Then, you can use the following command in SLCLI.

```python
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of a virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of an IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  --help                    Show this message and exit.
```

## Deleting a storage LUN in the UI
{: #cancelLUNUI}
{: help}
{: support}
{: ui}

If you no longer need a specific LUN, you can delete it at any time.

To cancel a storage LUN, it's necessary to revoke access from any hosts first.
{: important}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the volume to be canceled, click **Actions**, and select **Delete {{site.data.keyword.blockstorageshort}}**.
3. Confirm if want to delete the volume immediately or on the anniversary date of when the LUN was provisioned.

   If you select the option to delete the LUN on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

4. Click the **Acknowledgment** check box and click **Delete**

When the volume is canceled, there's a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours (immediate cancellation) or until the anniversary date. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs).

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.


## Deleting a storage LUN from the SLCLI
{: #cancelLUNCLI}
{: help}
{: support}
{: cli}

If you no longer need a specific LUN, you can cancel it at any time.

To cancel a storage LUN, it's necessary to revoke access from any hosts first.
{: important}

Then, you can use the following command in SLCLI to cancel the storage.
```python
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```

When the volume is canceled, there's a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours (immediate cancellation) or until the anniversary date. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs).

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.
