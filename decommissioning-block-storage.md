---

copyright:
  years: 2025, 2026
lastupdated: "2026-10-08"

keywords: Block Storage for Classic, delete volume, cancel LUN, decommission, authorized hosts, device permissions, reclaim

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Decommissioning and deleting {{site.data.keyword.blockstorageshort}} volumes
{: #decommissioning-block-storage}

Before you delete a {{site.data.keyword.blockstorageshort}} volume, complete the pre-deletion checklist to confirm that the volume is no longer in use. Deleting a volume is permanent: after the reclaim period expires, the data cannot be recovered.
{: shortdesc}

## Pre-deletion checklist
{: #block-storage-deletion-checklist}

Work through each item before you submit a cancellation request. Skipping steps is the most common cause of accidental data loss.

### 1. Confirm ownership and purpose
{: #checklist-ownership}

Check the **Notes** field on the volume details page and confirm with the team that originally ordered the volume. The volume name alone does not identify the workload or the data it contains. For more information, see [Viewing {{site.data.keyword.blockstorageshort}} volume details in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#viewLUNdeetsUI){: ui}[Viewing {{site.data.keyword.blockstorageshort}} volume details from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#viewLUNdeetsCLI){: cli}.

### 2. Understand what "authorized hosts" means
{: #checklist-authorized-hosts}

An authorized host can mount the volume. An empty authorized-host list is not sufficient evidence that a volume is unused or safe to delete. If authorized hosts are present, confirm with the host owners that the volume is no longer required before you proceed with deletion.

### 3. Verify the authorized-host list under the correct user
{: #checklist-verify-host-list}

The portal, the CLI (`ibmcloud sl block access-list`), and the SoftLayer API (`SoftLayer_Network_Storage::getObject` with `allowedVirtualGuests`, `allowedHardware`, `allowedSubnets`, or `allowedIpAddresses`) return only the hosts the calling user has Classic infrastructure device permission to see. A user without permission to view a server receives HTTP 200 with an empty list, not an error. An empty list does not confirm that the volume has no authorized hosts.

Always run the authorization check as the account owner, or as a user with **access to all devices**. To grant all-device access, go to **Manage > Access (IAM) > Users**, open the user, then under **Classic infrastructure > Devices**, select all device types and enable **Automatically grant access when new devices are added**. For more information, see [Managing classic infrastructure access](/docs/BlockStorage?topic=BlockStorage-mngclassicinfra).
{: important}

If you are scripting the authorization check, treat an empty host list as **unverified**, not **unused**. Log the HTTP response body so that a permission-filtered result is distinguishable from a genuinely empty result.
{: tip}

For more information, see [Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} volume in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#viewauthhostUI){: ui}[Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} volume from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#viewauthhostCLI){: cli}[Viewing the list of hosts that are authorized to access a {{site.data.keyword.blockstorageshort}} volume with Terraform](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=terraform#viewauthhostTerraform){: terraform}.

### 4. Check actual data usage on the host
{: #checklist-data-usage}

{{site.data.keyword.blockstorageshort}} volumes do not report bytes used. The `bytes_used` column in `ibmcloud sl block volume-list` is not a reliable usage indicator for block volumes. To determine whether data is present on a block volume, inspect the volume from the authorized host:

- Run `multipath -ll` and `lsblk` to confirm the device is visible.
- Check mount points and file system usage with `df -h` or equivalent commands.
- Confirm with the application or database team that no live workload depends on the volume.

### 5. Unmount and disconnect the volume from every host
{: #checklist-unmount}

Before you revoke authorization or cancel the volume, unmount the volume from all operating systems and log out of the iSCSI target. Canceling a mounted volume can cause data corruption or stale sessions on the host.

Follow the instructions for your operating system to safely unmount and disconnect:

- [Unmounting {{site.data.keyword.blockstorageshort}} volumes on Red Hat Enterprise Linux 9](/docs/BlockStorage?topic=BlockStorage-mountingRHEL#unmountingLin)
- [Unmounting {{site.data.keyword.blockstorageshort}} volumes on Debian 12](/docs/BlockStorage?topic=BlockStorage-mountingdebian#unmountingLindebian)
- [Unmounting {{site.data.keyword.blockstorageshort}} volumes on Ubuntu](/docs/BlockStorage?topic=BlockStorage-mountingUbuntu#unmountingUbu)
- [Unmounting {{site.data.keyword.blockstorageshort}} volumes on CloudLinux 8](/docs/BlockStorage?topic=BlockStorage-mountingCloudLin8#unmountingcloudlin)
- [Unmounting {{site.data.keyword.blockstorageshort}} volumes on Microsoft Windows](/docs/BlockStorage?topic=BlockStorage-mountingWindows#unmountingWin)

### 6. Revoke all host authorizations
{: #checklist-revoke}

After the volume is unmounted from every host, revoke access for all authorized hosts. For more information, see [Revoking a host's access to {{site.data.keyword.blockstorageshort}} in the console](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=ui#revokeauthinUI){: ui}[Revoking access from the CLI](/docs/BlockStorage?topic=BlockStorage-managingstorage&interface=cli#revokeCLI){: cli}.

### 7. Cancel active replication and remove dependent duplicates
{: #checklist-replication}

Active replicas and dependent duplicate volumes block reclamation of the original volume. Cancel any replication partnerships and remove any dependent duplicates before you request deletion. For more information, see [Replication](/docs/BlockStorage?topic=BlockStorage-replication) and [Creating a duplicate volume](/docs/BlockStorage?topic=BlockStorage-duplicatevolume).

### 8. Understand the reclaim timeline
{: #checklist-reclaim}

After a cancellation request, the volume is reclaimed approximately 24 hours later (for immediate cancellation) or on the next billing anniversary date. After the volume is reclaimed, the volume and all of its data are permanently destroyed and cannot be recovered. To stop a pending cancellation, open a [Support case](/unifiedsupport/cases/add){: external} before the reclaim runs.


## Delete a storage volume in the console
{: #cancelLUNUI}
{: help}
{: support}
{: ui}

If you no longer need a specific volume, you can delete it at any time.

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the volume to be canceled, click **Actions**, and select **Delete {{site.data.keyword.blockstorageshort}}**.
3. Confirm whether you want to delete the volume immediately or on the anniversary date of when the volume was provisioned.

   If you select the option to delete the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

4. Click the **Acknowledgment** checkbox and click **Delete**.

## Delete a storage volume from the CLI
{: #cancelLUNCLI}
{: help}
{: support}
{: cli}

If you no longer need a specific volume, you can delete it at any time.

### Delete a storage volume from the IBM Cloud CLI
{: #cancelLUNICCLI}

Use the following command to cancel the storage. The following example command cancels the volume 12345678 immediately, instead of on the anniversary date.

```sh
ibmcloud sl volume-cancel --immediate 12345678
```
{: screen}

For more information about all of the parameters that are available for this command, see [ibmcloud sl block volume-cancel](/docs/cli?topic=cli-sl-block-storage#sl_block_volume_cancel){: external}.

### Delete a storage volume from the SLCLI
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

## Delete a storage volume with the API
{: #cancelLUNAPI}
{: help}
{: support}
{: api}

Use the [`cancel_volume` method](https://softlayer-python.readthedocs.io/en/latest/api/managers/SoftLayer.managers.BlockStorageManager/#SoftLayer.managers.BlockStorageManager.cancel_volume){: external} in the SoftLayer Python API client. Specify the `volume_id` and whether to cancel immediately or on the billing anniversary date (`immediate=True` or `immediate=False`). Optionally, provide a reason for the cancellation.

## Delete a storage volume from Terraform
{: #cancelLUNTerraform}
{: help}
{: support}
{: terraform}

The preferred way to delete a block volume that is managed by Terraform is to remove its `ibm_storage_block` resource block from your configuration and run `terraform apply`. Terraform detects the missing resource and destroys it, keeping your configuration and state in sync.

1. Open your Terraform configuration file and delete the `ibm_storage_block` resource block for the volume.
2. Run `terraform apply` to apply the change.

   ```sh
   terraform apply
   ```
   {: pre}

   Terraform displays a plan that shows the resource is marked for destruction. Confirm the plan to proceed.

If you want to destroy a specific volume without editing your configuration, you can use `terraform destroy --target` instead. The following example targets a single resource by its Terraform address.

```sh
terraform destroy --target ibm_storage_block.example
```
{: pre}

Using this method preserves the resource block in your configuration, which creates a drift between your config and your infrastructure state. Remove the resource block from your configuration after you confirm that the volume is deleted.

For more information, see [terraform apply](https://developer.hashicorp.com/terraform/cli/commands/apply){: external} and [terraform destroy](https://developer.hashicorp.com/terraform/cli/commands/destroy){: external}.
