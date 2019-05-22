---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage Encryption, industry standard protocols, IBM Block Storage, LUN, provider-managed encryption

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Provider-managed Encryption-At-Rest
{: #encryption}

## {{site.data.keyword.blockstorageshort}} Encryption-At-Rest

{{site.data.keyword.BluSoftlayer_full}} takes the need for security seriously, and understands the importance of being able to encrypt data to keep it safe. With provider-managed encryption, {{site.data.keyword.blockstoragefull}} that is provisioned with either Endurance or Performance option is encrypted by default, at no extra cost and no impact to performance.

The provider-managed encryption-at-rest feature uses the following industry standard protocols:

* Industry-Standard AES-256 encryption
* Keys are managed in-house with industry standard Key Management Interoperability Protocol (KMIP)
* Storage is validated for Federal Information Processing Standard (FIPS) Publication 140-2, Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA). Storage is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386), and EU Data Protection Directive 95/46/EC compliance.

## Providing Encryption-at-Rest for Snapshots or Replicated storage  

All snapshots and replicas of encrypted {{site.data.keyword.blockstorageshort}} are also encrypted by default. This feature can't be turned off on a volume basis.

## Provisioning Storage with Encryption

The provider-managed encryption-at-rest feature is available for {{site.data.keyword.blockstorageshort}} that is provisioned in [select data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-news). All storage that is ordered in these data centers is automatically provisioned with encryption.

When you order {{site.data.keyword.blockstorageshort}}, select a data center noted with an asterisk (`*`). You can see a lock icon to the right of the LUN/Volume Name field that indicates that the volume is encrypted.

![The lock icon indicates that the LUN is encrypted](/images/encryptedstorage.png)
<caption>Figure 1. Example of the lock icon that shows that the LUN is encrypted.</caption>



Non-encrypted storage that was provisioned before the data center was upgraded **isn't** automatically encrypted. If you own non-encrypted storage in an upgraded data center and you want encrypted storage, then you need to create a new volume and migrate your data. For more information, see [{{site.data.keyword.blockstorageshort}} Migration in Upgraded Data Centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage).
{:important}
