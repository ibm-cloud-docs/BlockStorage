---

copyright:
  years: 2014, 2025
lastupdated: "2025-03-19"

keywords: Block Storage for Classic, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, iSCSI, MPIO, redundant

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

You can manage your {{site.data.keyword.blockstoragefull}} volumes in the [{{site.data.keyword.cloud}} console](/classic-gen1){: external}. From the **menu** ![Menu icon](../icons/icon_hamburger.svg "Menu"), select **Infrastructure** ![VPC icon](../icons/vpc.svg) > **Classic Infrastructure** to interact with classic services. You can also manage your volumes from the CLI, with the API or Terraform.
{: shortdesc}

## Viewing {{site.data.keyword.blockstorageshort}} volume details in the console
{: #viewLUNdeetsUI}
{: help}
{: support}
{: ui}

You can view a summary of the key information for the selected storage volume that includes the snapshot and replication capabilities that were added to the storage.

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Click the appropriate Volume name from the list.

## Viewing {{site.data.keyword.blockstorageshort}} volume details from the CLI
{: #viewLUNdeetsCLI}
{: help}
{: support}
{: cli}

Before you begin, decide on the CLI client that you want to use.

* You can either install the [IBM Cloud CLI](/docs/cli){: external} and install the SL plug-in with `ibmcloud plugin install sl`. For more information, see [Extending IBM Cloud CLI with plug-ins](/docs/cli?topic=cli-plug-ins).
* Or, you can install the [SLCLI](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.

### Viewing {{site.data.keyword.blockstorageshort}} volume details from the IBMCLOUD CLI
{: #viewLUNdeetsICCLI}

1. Use the `ibmcloud sl block volume-list` command to view the list of the available storage volumes. Locate the volume in the output. You can sort the list by `id`, `username`, `datacenter`, `storage_type`, `capacity_gb`, `bytes_used`, `ip_addr`,`lunId`, `active_transactions`, and `created_by` values.

   - The following example retrieves the list of volumes and the output sorts the volumes by name with the most recently created volume first.
     ```sh
     ibmcloud sl block volume-list -sortby username
     ```
    {: pre}

   - The following example retrieves the list of volumes that were created by a specific order.
     ```sh
     $ ibmcloud sl block volume-list --order 110758744
     id        username            datacenter   storage_type             capacity_gb   bytes_used   IOPs   ip_addr     lunId  active_transactions   rep_partner_count   notes
     562193766 SL02SEL1414935-675  dal09        endurance_block_storage  80            -            -      10.2.125.62 0      0                     0                   - 
     ```
     {: screen}

1. Use the `ibmcloud sl block volume-detail` command to view the details of a specific block volume from the CLI.

   ```sh
   $ ibmcloud sl block volume-detail  562193766
   Name                       Value
   ID                         562193766
   User name                  SL02SEL1414935-675
   Type                       endurance_block_storage
   Capacity (GB)              80
   LUN Id                     0
   Endurance Tier             LOW_INTENSITY_TIER
   Endurance Tier Per IOPS    0.25
   Datacenter                 dal09
   Target IP                  10.2.125.62
   Snapshot Size (GB)         20
   Snapshot Used (Bytes)      -
   # of Active Transactions   0
   Replicant Count            0
   Notes                      -
   ```
   {: screen}

For more information about all of the parameters that are available for these commands, see [ibmcloud sl block volume-detail](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_detail){: external} and [ibmcloud sl block volume-list](/docs/BlockStorage?topic=BlockStorage-sl-block-storage#sl_block_volume_list){: external}.

### Viewing {{site.data.keyword.blockstorageshort}} volume details from the SLCLI
{: #viewLUNdeetsSLCLI}

To view information about a Storage volume, you can use the following commands from the CLI.

1. List the available storage volumes with the `slcli block volume-list` command, and use one of the available filters to help identify the volume that you are interested in. The following example command lists the volumes by their order IDs.
   ```sh
   slcli block volume-list --order ORDER_ID
   ```
    {: pre}

1. Use the volume ID from the output of the first command to run the `slcli block volume-detail` command. 

   ```sh
   $ slcli block volume-detail --help
   Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

   Options:
     -h, --help  Show this message and exit.
   ```
   {: screen}

For more information about all of the parameters that are available for these commands, see [block volume-detail](https://softlayer-python.readthedocs.io/en/latest/cli/block/#block-volume-detail){: external} and [block volume-list](https://softlayer-python.readthedocs.io/en/latest/cli/block/#block-volume-list){: external}. 

## Authorizing hosts to access {{site.data.keyword.blockstorageshort}} in the console
{: #authhostUI}
{: help}
{: support}
{: ui}

"Authorized" hosts are hosts that are granted access to a particular volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the username, password, and iSCSI qualified name (IQN), which are needed to mount the multipath I/O (MPIO) iSCSI connection.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Locate the volume and click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions").
3. Click **Authorize Host**.
4. To see the list of available devices or IP addresses, first, select whether you want to authorize access based on device type or subnets.
   - If you choose the Devices option, you can select from Bare Metal Server or virtual server instances.
   - If you choose the IP address option, select the subnet where your host resides.
5. From the filtered list, select one or more hosts that can access the volume and click **Save**.

The default limit for the number of authorizations per block volume is eight. That means that up to eight hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} volume. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware deployment can request the authorization limit to be increased to 64. To request a limit increase, raise a [Support case](/unifiedsupport/cases/add){: external}.
{: note}

## Authorizing hosts to access {{site.data.keyword.blockstorageshort}} from the CLI
{: #authhostCLI}
{: help}
{: support}
{: cli}

“Authorized” hosts are hosts that are granted access to a particular volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the username and password.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

### Authorizing hosts from the IBMCLOUD CLI
{: #authhostICCLI}

Use the `ibmcloud sl block access-authorize` command to authorize a host to access the volume. The following example authorizes the virtual server instance `87654321` to mount the volume `12345678`.

```sh
ibmcloud sl block access-authorize 12345678 --virtual-id 87654321
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block access-authorize](/docs/cli?topic=cli-sl-block-storage#sl_block_access_authorize){: external}.

You can also specify a subnet of the Compute instances that are allowed to access the storage by using the following command.

```sh
ibmcloud sl block subnets-assign --subnet-id 1234 87654321
```
{: screen}

### Authorizing hosts from the SLCLI
{: #authhostSLCLI}

To authorize a host to access the volume, you can use the following command.
```sh
$ slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```
{: screen}

```sh
$ slcli block subnets-list -h
Usage: slcli block subnets-list [OPTIONS] ACCESS_ID
  List block storage assigned subnets for the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
    -h, --help  Show this message and exit.
```
{: screen}

```sh
$ slcli block subnets-assign -h
Usage: slcli block subnets-assign [OPTIONS] ACCESS_ID
  Assign block storage subnets to the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to assign; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```
{: screen}

```sh
$ slcli block subnets-remove -h
Usage: slcli block subnets-remove [OPTIONS] ACCESS_ID
  Remove block storage subnets for the given host id.
  access_id is the host_id obtained by: slcli block access-list <volume_id>

Options:
  --subnet-id INTEGER  ID of the subnets to remove; e.g.: --subnet-id 1234
  -h, --help           Show this message and exit.
```
{: screen}

The default limit for the number of authorizations per block volume is eight. That means that up to eight hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} volume. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware deployment can request the authorization limit to be increased to 64. To request a limit increase, raise a [Support case](/unifiedsupport/cases/add){: external}.
{: note}

## Authorizing hosts to access {{site.data.keyword.blockstorageshort}} with Terraform
{: #authhostTerraform}
{: help}
{: support}
{: terraform}

"Authorized" hosts are hosts that are granted access to a particular volume. Without host authorization, you can't access or use the storage from your system. Authorizing a host to access your volume generates the username, password, and iSCSI qualified name (IQN), which are needed to mount the multipath I/O (MPIO) iSCSI connection.

You can authorize and connect hosts that are located in the same data center as your storage. You can have multiple accounts, but you can't authorize a host from one account to access your storage on another account.
{: important}

To authorize a Compute host to access the volume, use the `ibm_storage_block` resource and specify the `allowed_virtual_guest_ids` for virtual servers, or `allowed_hardware_ids` for bare metal servers. Specify `allowed_ip_addresses` to define which IP addresses have access to the storage. 

The following example defines that the virtual server with the ID `27699397` can access the volume from the `10.40.98.193`, `10.40.98.200` addresses.

```terraform
resource "ibm_storage_block" "test1" {
        type = "Endurance"
        datacenter = "dal09"
        capacity = 40
        iops = 4
        os_format_type = "Linux"

        # Optional fields
        allowed_virtual_guest_ids = [ 27699397 ]
        allowed_ip_addresses = ["10.40.98.193", "10.40.98.200"]
        snapshot_capacity = 10
        hourly_billing = true
}
```
{: codeblock}

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

The default limit for the number of authorizations per block volume is eight. That means that up to eight hosts can be authorized to access the {{site.data.keyword.blockstorageshort}} volume. Customers who use {{site.data.keyword.blockstorageshort}} in their VMware deployment can request the authorization limit to be increased to 64. To request a limit increase, raise a [Support case](/unifiedsupport/cases/add){: external}.
{: note}

To remove authorization from a host, remove its details from the `ibm_storage_block` resource and apply your changes.

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} volume in the console
{: #viewauthhostUI}
{: help}
{: support}
{: ui}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and click your Volume name.
1. Click **Authorized Hosts** to display the list of Compute instances that have access to the volume.
1. Click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **View host details**. A side panel is displayed with details like device name, IP address, username and password, Host IQN, and device type. When ISCSI Isolation is enabled, the Access Control List section is also displayed. You can add or remove subnets in the Access Control List section.
 
The Target address is listed on the **Storage Detail** page. For NFS, the Target address is described as a DNS name, and for iSCSI, it's the IP address of the Discover Target Portal.
{: tip}

## Updating host authorization in the console
{: #updateauthhostUI}
{: help}
{: support}
{: ui}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and click your Volume name.
1. Click **Authorized Hosts** to display the list of Compute instances that have access to the volume.
1. Click the ellipsis ![Actions icon](../icons/action-menu-icon.svg "Actions") and select **Add a subnet**. This option is only available when ISCSI Isolation is enabled.
1. In the new dialog box, select the subnet that you want to add from the list.
1. Click **Submit**.

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} volume from the CLI
{: #viewauthhostCLI}
{: help}
{: support}
{: cli}

### Viewing the list of authorized hosts from the IBMCLOUD CLI
{: #viewauthhostICCLI}

To confirm that the authorization worked, run the `ibmcloud sl block access-list` command.

```sh
ibmcloud sl block access-list 12345678 --sortby id 
```
{: screen}

### Viewing the list of authorized hosts from the SLCLI
{: #viewauthhostSLCLI}

To see the list of hosts, which are currently authorized to access the volume, you can use the following command.

```sh
$ slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```
{: screen}

## Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} volume with Terraform
{: #viewauthhostTerraform}
{: help}
{: support}
{: terraform}

After your storage resource is created, you can view the `allowed_host_info` attribute, which contains the username, password, and the IQN of the Compute host that is authorized to access the volume.

For more information about the arguments and attributes, see [ibm_storage_block](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/storage_block){: external}.

## Viewing the {{site.data.keyword.blockstorageshort}} to which a host is authorized in the console
{: #viewLUNauthUI}
{: help}
{: support}
{: ui}

You can view the volumes, which a host has access to, including information that is needed to make a connection – LUN Name, Storage type, Target address, capacity, and location:

1. Click **Devices** > **Device List** in the [{{site.data.keyword.cloud}} console](/classic-gen1){: external} and click the appropriate device.
2. Select the **Storage** tab.

You're presented with a list of storage volumes that this particular host has access to. The list is grouped by storage type (block, file, other). You can authorize more storage or remove access by clicking **Actions**.

A host cannot be authorized to access volumes of differing OS types at the same time. A host can be authorized to access volumes of a **single** OS type. If you attempt to authorize a host to access multiple volumes with different OS types, the operation results in an error.
{: note}

## Revoking a host's access to {{site.data.keyword.blockstorageshort}} in the console
{: #revokeauthinUI}
{: ui}

If you want to stop the access from a host to a particular storage volume, you can revoke the access. Upon revoking access, the host connection is dropped from the volume. The operating system and applications on that host can't communicate with the volume anymore.

To avoid host side issues, unmount the storage volume from your operating system before you revoke the access to avoid missing drives or data corruption.
{: important}

You can revoke access from the **Device List** or the **Storage view**.

### Revoking access from the Device List
{: #revokeDeviceUI}
{: help}
{: support}
{: ui}

1. In the [{{site.data.keyword.cloud}} console](/classic-gen1){: external}, click the Classic Infrastructure icon. Then, click **Devices** > **Device List** and double-click the appropriate device.
2. Select the **Storage** tab.
3. You are presented with a list of storage volumes that this particular host has access to. The list is grouped by storage type (block, file, other). Next to the Volume name, click **Actions**, and click **Revoke Access**.
4. Confirm that you want to revoke the access for a volume because the action can't be undone. Click **Yes** to revoke volume access or **No** to cancel the action.

If you want to disconnect multiple volumes from a specific host, you need to repeat the Revoke Access action for each volume.
{: tip}

### Revoking access from the Storage View
{: #revokeStorageUI}
{: help}
{: support}
{: ui}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**, and select the volume from which you want to revoke access.
2. Click **Authorized Hosts**.
3. Click **Actions** ![Actions icon](../icons/action-menu-icon.svg "Actions") next to the host whose access is to be revoked, and select **Revoke Access**.
4. Confirm that you want to revoke the access for a volume because the action can't be undone. Click **Yes** to revoke volume access or **No** to cancel the action.

If you want to disconnect multiple hosts from a specific volume, you need to repeat the Revoke Access action for each host.
{: tip}

## Revoking access from the CLI.
{: #revokeCLI}
{: help}
{: support}
{: cli}

If you want to stop the access from a host to a particular storage volume, you can revoke the access. Upon revoking access, the host connection is dropped from the volume. The operating system and applications on that host can't communicate with the volume anymore.

To avoid host side issues, unmount the storage volume from your operating system before you revoke the access to avoid missing drives or data corruption.
{: important}

### Revoking access from the IBMCLOUD CLI
{: #revokeICCLI}

Use the following command to revoke access from a Compute host. In the following example, access to the volume 12345678 is revoked from the virtual server instance 87654321.

```sh
ibmcloud sl block access-revoke 12345678 --virtual-id 87654321
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block access-revoke](/docs/cli?topic=cli-sl-block-storage#sl_block_access_revoke){: external}.

### Revoking access from the SLCLI
{: #revokeSLCLI}

Use the following command to revoke access from a Compute host.

```sh
$ slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of a virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of an IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  --help                    Show this message and exit.
```
{: screen}

## Deleting a storage volume in the console
{: #cancelLUNUI}
{: help}
{: support}
{: ui}

If you no longer need a specific volume, you can delete it at any time.

To cancel a storage volume, it's necessary to revoke access from any hosts first.
{: important}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the volume to be canceled, click **Actions**, and select **Delete {{site.data.keyword.blockstorageshort}}**.
3. Confirm if you want to delete the volume immediately or on the anniversary date of when the volume was provisioned.

   If you select the option to delete the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

4. Click the **Acknowledgment** checkbox and click **Delete**.

When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours (immediate cancellation) or until the anniversary date. The waiting period gives you a chance to void the request to cancel if needed. If you want to cancel the deletion of the volume, raise a [Support case](/unifiedsupport/cases/add){: external}. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs).

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.

## Deleting a storage volume from the CLI
{: #cancelLUNCLI}
{: help}
{: support}
{: cli}

If you no longer need a specific volume, you can cancel it at any time.

To cancel a storage volume, it's necessary to revoke access from any hosts first.
{: important}

When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours (immediate cancellation) or until the anniversary date. The waiting period gives you a chance to void the request to cancel if needed. If you want to cancel the deletion of the volume, raise a [Support case](/unifiedsupport/cases/add){: external}. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs).

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.

### Deleting a storage volume from the IBMCLOUD CLI
{: #cancelLUNICCLI}

Use the following command to cancel the storage. The following example command cancels the volume 12345678 immediately, instead of on the anniversary date.

```sh
ibmcloud sl volume-cancel --immediate 12345678
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block volume-cancel](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_cancel){: external}.

### Deleting a storage volume from the SLCLI
{: #cancelLUNSLCLI}

Use the following command in SLCLI to cancel the storage.
```sh
$ slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```
{: screen}

## Deleting a storage volume from Terraform
{: #cancelLUNTerraform}
{: help}
{: support}
{: terraform}

Use the `terraform destroy` command to conveniently remove a remote object such as a single volume. The following example shows the syntax of the command.

```terraform
terraform destroy --target ibm_storage_block.volumeID
```
{: codeblock}

For more information, see [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.

When the volume is canceled, the request is followed by a 24-hour reclaim wait period. You can still see the volume in the console during those 24 hours (immediate cancellation) or until the anniversary date. The waiting period gives you a chance to void the request to cancel if needed. If you want to cancel the deletion of the volume, raise a [Support case](/unifiedsupport/cases/add){: external}. Billing for the volume stops immediately. When the reclaim-period expires, the data is destroyed and the volume is removed from the console, too. For more information, see the [FAQ](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs).

Active replicas and dependent duplicates can block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.
