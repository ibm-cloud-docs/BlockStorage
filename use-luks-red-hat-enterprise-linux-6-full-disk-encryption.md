---

copyright:
  years: 2014, 2024
lastupdated: "2024-02-15"

keywords: Block Storage, encryption, LUKS, RHEL, Linux, security, auxiliary storage

subcollection: BlockStorage

---
{{site.data.keyword.attribute-definition-list}}

# Achieving full disk encryption with LUKS in RHEL
{: #LUKSencryption}

You can encrypt partitions on your RHEL server with Linux&reg; Unified Key Setup-on-disk-format (LUKS), which is important when it comes to mobile Computers and removable media. LUKS allows multiple user keys to decrypt a main key that is used for the bulk encryption of the partition. The following instructions are applicable to RHEL versions RHEL6 or newer.
{: shortdesc}

These steps assume that the server can access a new, unencrypted {{site.data.keyword.blockstoragefull}} volume that was not formatted or mounted. For more information about connecting {{site.data.keyword.blockstorageshort}} to a Linux&reg; host, see [Connecting to storage on Linux&reg;](/docs/BlockStorage?topic=BlockStorage-mountingLinux).

All new {{site.data.keyword.blockstorageshort}} is automatically provisioned with provider-managed encryption-at-rest. For more information, see [Securing Your Data - Provider-managed Encryption-At-Rest](/docs/BlockStorage?topic=BlockStorage-mng-data).
{: note}

## What LUKS does
{: #LUKSinscope}

- LUKS encrypts entire block devices and is therefore suited for protecting the contents of mobile devices such as removable storage media or Notebook disk drives.
- The underlying contents of the encrypted block device are arbitrary, making it useful for encrypting swap devices. The encrypting can also be useful with certain databases that use specially formatted block devices for data storage.
- LUKS uses the existing device mapper kernel subsystem.
- LUKS provides passphrase strengthening, which protects against dictionary attaches.
- LUKS contains multiple key slots, so users can add backup keys or passphrases.


## What LUKS doesn't do
{: #LUKSoutofscope}

- LUKS does not allow applications that require many (more than eight) users to have distinct access keys to the same devices.
- LUKS does not work with applications that require file-level encryption. For more information, see [RHEL Security Guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/security_guide/index){: external}.

## Setting up a LUKS-encrypted volume with Endurance {{site.data.keyword.blockstorageshort}}
{: #enryptwithLUKS}

The process of data encryption creates a load on the host that might potentially impact performance.
{: note}

1. Type the following command at a shell prompt as root to install the required package:
   ```sh
   yum install cryptsetup-luks
   ```
   {: pre}

2. Get the disk ID:
   ```sh
   fdisk –l | grep /dev/mapper
   ```
   {: pre}

3. Locate your volume in the listing.
4. Encrypt the block device.

   1. This command initializes the volume, and you can set a passphrase.

      ```sh
      cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. Respond with `YES` (all uppercase letters).

   3. The device now appears as an encrypted volume:

      ```sh
      $ blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```
      {: screen}

5. Open the volume, and create a mapping.

   ```sh
   cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}

6. Enter the passphrase.
7. Verify the mapping, and view the status of the encrypted volume.

   ```sh
   $ cryptsetup -v status cryptData
   /dev/mapper/cryptData is active.
     type:  LUKS1
     cipher:  aes-cbc-essiv:sha256
     keysize: 256 bits
     device:  /dev/mapper/3600a0980383034685624466470446564
     offset:  4096 sectors
     size:    41938944 sectors
     mode:    read/write
     Command successful
   ```
   {: screen}

8. Write random data to `/dev/mapper/cryptData` on the encrypted device. This action ensures that the outside world sees this as random data, which means it is protected against disclosure of usage patterns. This step can take a while.
   
   ```sh
   shred -v -n1 /dev/mapper/cryptData
   ```
   {: pre}

9. Format the volume.
   
   ```sh
   mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}

10. Mount the volume.
   
   ```sh
   mkdir /cryptData
   ```
   {: pre}

   ```sh
   mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}

   ```sh
   df -H /cryptData
   ```
   {: pre}

### Unmounting and closing the encrypted volume securely
{: #unmountLUKS}

   ```sh
   umount /cryptData
   cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Remounting and mounting an existing LUKS encrypted partition
{: #remountLUKS}

   ```sh
   $ cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   $ mount /dev/mapper/cryptData /cryptData
   $ df -H /cryptData
   $ lsblk
   NAME                                       MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
   xvdb                                       202:16   0    2G  0 disk
   └─xvdb1                                    202:17   0    2G  0 part  [SWAP]
   xvda                                       202:0    0   25G  0 disk
   ├─xvda1                                    202:1    0  256M  0 part  /boot
   └─xvda2                                    202:2    0 24.8G  0 part  /
   sda                                          8:0    0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                         253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                         253:1    0   20G  0 crypt /cryptData
   ```
   {: screen}
