---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Using LUKS in Red Hat Enterprise Linux for Full Disk Encryption

You can encrypt partitions on your Red Hat Enterprise Linux 6 server with Linux Unified Key Setup-on-disk-format (LUKS), which is important when it comes to mobile computers and removable media. LUKS allows multiple user keys to decrypt a master key that is used for the bulk encryption of the partition.

## What LUKS does

- Encrypts entire block devices and is therefore well-suited for protecting the contents of mobile devices such as removable storage media or notebook disk drives.
- The underlying contents of the encrypted block device are arbitrary, making it useful for encrypting swap devices. The encrypting can also be useful with certain databases that use specially formatted block devices for data storage.
- Uses the existing device mapper kernel subsystem.
- Provides passphrase strengthening, which protects against dictionary attaches.
- Allows users to add backup keys or passphrases because LUKS devices contain multiple key slots.


## What LUKS doesn't do

- Allow applications that require many (more than eight) users to have distinct access keys to same devices.
- Work with applications that require file-level encryption, [more information](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}.

## Setting up a LUKS encrypted volume with Endurance {{site.data.keyword.blockstorageshort}}

These steps assume that the server can access a new, unencrypted {{site.data.keyword.blockstoragefull}} volume that was not formatted or mounted. Click [here](accessing_block_storage_linux.html) for how to access {{site.data.keyword.blockstorageshort}} with Linux.

**Note**: The process of data encryption creates a load on the host that might potentially impact performance.

1. Type the following at a shell prompt as root to install the required package:   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Get the disk ID:<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Locate your volume in the listing.
4. Encrypt the block device;

   1. This command initializes the volume, and you can set a passphrase. <br/>
   
      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}
      
   2. Respond with YES (all uppercase letters.)
   
   3. The device now appears as an encrypted volume: 
   
      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```
      
5. Open the volume, and create a mapping.   <br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Enter the passphrase.
7. Verify the mapping, and view status of the encrypted volume.   <br/>
   ```
   # cryptsetup -v status cryptData
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
8. Write random data to `/dev/mapper/cryptData` on the encrypted device. This action ensures that outside world sees this as random data, which means it is protected against disclosure of usage patterns. This step can take a while.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Format the volume.<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Mount the volume.<br/>
   ```
   # mkdir /cryptData
   ```
   {: pre}
   ```
   # mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}
   ```
   # df -H /cryptData
   ```
   {: pre}

### Unmounting and closing the encrypted volume securely
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Remounting and Mounting an existing LUKS encrypted partition
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   # mount /dev/mapper/cryptData /cryptData
   # df -H /cryptData
   # lsblk
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
