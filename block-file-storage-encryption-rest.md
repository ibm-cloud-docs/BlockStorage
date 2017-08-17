---

copyright:
  years: 2014, 2017
lastupdated: "2017-06-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Block and File Storage Encryption-At-Rest 
IBM Bluemix Infrastructure takes the need for security seriously, and understand the importance of being able to encrypt data to keep it safe. With provider managed encryption, both Block and File storage provisioned with either Endurance or Performance are encrypted by default at no additional cost and no impact to performance.

The provider managed encryption-at-rest feature uses the following industry standard protocols:

* Industry-Standard AES-256 encryption
* Keys are managed in-house with industry standard Key Management Improbability Protocol (KMIP)
* Storage is Federal Information Processing Standard (FIPS) Publication 140-2 validated for Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA), Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) and EU Data Protection Directive 95/46/EC compliance

## Encryption-at-Rest for Snapshots or Replicated storage  

All snapshots and replicas of encrypted block and file storage are also encrypted by default.

## Provisioning Storage with Encryption

The provider managed encryption-at-rest feature is only available for Block and File storage that is provisioned in select data centers with new data center availability being added regularly. All storage provisioned in these data centers is automatically provisioned with encryption for data-at-rest.   Click here to see the current list of data centers where Block and File storage encryption for data-at-rest is available.


When ordering your Block or File storage, select a data center noted with an * and message stating that encryption is available. You will see a lock icon to the right of the LUN/Volume Name field indicating that it is encrypted. See Figure 1.

![The lock icon indicates that the LUN is encrypted](/images/encryptedstorage.png)
<caption>Figure 1. Example of the lock icon indicating the LUN is encrypted.</caption>



**Note** that non-encrypted storage provisioned prior to a data center upgrade will **not** be automatically encrypted. If you have non-encrypted storage in an upgraded data center, you will need to create a new LUN or volume and perform a data migration. The following articles can provide guidance.

* [Block Storage Migration in Upgraded Data Centers](migrate-block-storage-encrypted-block-storage.html)
