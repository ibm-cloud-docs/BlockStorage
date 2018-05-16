---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Securing Your Data - Provider-managed Encryption-At-Rest

## {{site.data.keyword.blockstorageshort}} Encryption-At-Rest 

{{site.data.keyword.BluSoftlayer_full}} takes the need for security seriously, and understands the importance of being able to encrypt data to keep it safe. With provider-managed encryption, {{site.data.keyword.blockstoragefull}} provisioned with either Endurance or Performance option is encrypted by default, at no extra cost and no impact to performance.

The provider-managed encryption-at-rest feature uses the following industry standard protocols:

* Industry-Standard AES-256 encryption
* Keys are managed in-house with industry standard Key Management Interoperability Protocol (KMIP)
* Storage is validated for Federal Information Processing Standard (FIPS) Publication 140-2, Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA). Storage is also validated for Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) and EU Data Protection Directive 95/46/EC compliance.

## Encryption-at-Rest for Snapshots or Replicated storage  

All snapshots and replicas of encrypted {{site.data.keyword.blockstorageshort}} are also encrypted by default. This feature can't be turned off on a volume basis.

## Provisioning Storage with Encryption

The provider-managed encryption-at-rest feature is only available for {{site.data.keyword.blockstorageshort}} that is provisioned in [select data centers](new-ibm-block-and-file-storage-location-and-features.html). All storage provisioned in these data centers is automatically provisioned with encryption for data-at-rest.

When ordering your {{site.data.keyword.blockstorageshort}}, select a data center noted with an asterisk (`*`). You'll see a lock icon to the right of the LUN/Volume Name field indicating that it's encrypted.'

![The lock icon indicates that the LUN is encrypted](/images/encryptedstorage.png)
<caption>Figure 1. Example of the lock icon indicating the LUN is encrypted.</caption>



**Note**: non-encrypted storage provisioned before the data center was upgrades **won't** be automatically encrypted. If you have non-encrypted storage in an upgraded data center, you'll need to create a new LUN or volume and perform a data migration. The following article can provide guidance:

* [{{site.data.keyword.blockstorageshort}} Migration in Upgraded Data Centers](migrate-block-storage-encrypted-block-storage.html)
