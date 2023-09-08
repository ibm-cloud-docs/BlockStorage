---

copyright:
  years: 2020, 2023
lastupdated: "2023-09-08"

keywords: data encryption in Block Storage, data storage for Block Storage, bring your own keys for Block Storage, BYOK for Block Storage, key management for Block Storage, key encryption for Block Storage, personal data in Block Storage, data deletion for Block Storage, data in Block Storage, data security in Block Storage

subcollection: BlockStorage

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in {{site.data.keyword.blockstorageshort}}
{: #mng-data}

It's important to know exactly how data is stored, how it is encrypted, and how you can delete personal data, to rest assured that your data in {{site.data.keyword.blockstorageshort}} is securely managed. 
{: shortdesc}

## How your data is stored and encrypted in {{site.data.keyword.blockstorageshort}}
{: #data-storage}

{{site.data.keyword.blockstoragefull}} that is provisioned with either Endurance or Performance option is secured with provider-managed encryption, at no extra cost and no impact to performance.
{: shortdesc}

The provider-managed encryption-at-rest feature uses the following industry standard protocols:

* Industry-Standard AES-256 encryption.
* Keys are managed in-house with industry standard Key Management Interoperability Protocol (KMIP).
* Storage is validated for US Federal Information Processing Standard (FIPS) Publication 140-2, Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA). Storage is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386), and EU General Data Protection Regulation (GDPR) compliance.

## Securing your snapshots or replicated storage
{: #SecureSnapshotBlock}

All snapshots and replicas of encrypted file storage are also encrypted by default. This feature can’t be turned off on a volume basis.
All cluster-to-cluster traffic is encrypted with TLS.

## Provisioning Storage with Encryption
{: #createencryptedLUN1}

The provider-managed encryption-at-rest feature is available for {{site.data.keyword.blockstorageshort}} in all 
{{site.data.keyword.cloud}} [data centers](/docs/BlockStorage?topic=BlockStorage-selectDC).

When you order {{site.data.keyword.blockstorageshort}}, select a data center noted with an asterisk (`*`). You can see a lock icon to the right of the LUN/Volume Name field that indicates that the volume is encrypted.

![Figure 1. Example of the lock icon that indicates that the LUN is encrypted.](/images/encryptedstorage.svg){: caption="FFigure 1. Example of the lock icon that indicates that the LUN is encrypted." caption-side="bottom"}

Nonencrypted storage that was provisioned before the data center was upgraded **isn't** automatically encrypted. If you own nonencrypted storage in an upgraded data center and you want encrypted storage, then you need to create a new volume and migrate your data. For more information, see [{{site.data.keyword.blockstorageshort}} Migration in Upgraded Data Centers](/docs/BlockStorage?topic=BlockStorage-migratestorage).
{: important}

## Deleting {{site.data.keyword.blockstorageshort}} instances
{: #service-delete}

If you no longer need a specific LUN, you can cancel it at any time. {{site.data.keyword.blockstoragefull}} presents Block volumes to customers on physical storage that is wiped before any reuse. For more information, see the [FAQs](/docs/BlockStorage?topic=BlockStorage-block-storage-faqs#deleted).

Customers with special requirements for compliance such as NIST 800-88 Guidelines for Media Sanitization can perform the data sanitization procedure before they delete their storage.

To delete a storage LUN, it's necessary to revoke access from any hosts first. Active replicas and dependent duplicates can also block reclamation of the Storage volume. Make sure that the volume is no longer mounted, host authorizations are revoked, replication is canceled, and no dependent duplicates exist before you attempt to cancel the original volume.
{: important}

1. Click **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Select the volume to be canceled, click **Actions**, and select **Delete {{site.data.keyword.blockstorageshort}}**.
3. Confirm if want to cancel the volume immediately or on the anniversary date of when the LUN was provisioned.

   If you select the option to delete the volume on its anniversary date, you can void the cancellation request before its anniversary date.
   {: tip}

4. Click **Continue**.
5. Click the **Acknowledgment** checkbox and click **Confirm**.

You can expect the LUN to remain visible in your Storage list for at least 24 hours (immediate cancellation) or until the anniversary date. Certain features aren't going to be available any longer, but the volume remains visible until it is reclaimed. However, billing is stopped immediately after you cancel.

After the Storage LUN is reclaimed, the disk is wiped, and data can't be restored. When drives are decommissioned, IBM destroys them before they can be disposed of. The drives become unusable. Any data that was written to that drive becomes inaccessible.
