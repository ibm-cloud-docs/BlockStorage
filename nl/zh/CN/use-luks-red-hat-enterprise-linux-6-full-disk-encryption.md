---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block storage, encryption, LUKS, RHEL, Linux, security, auxiliary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 利用 Red Hat Enterprise Linux 中的 LUKS 实现全磁盘加密
{: #LUKSencryption}

可以通过 Linux Unified Key Setup-on-disk-format (LUKS) 在 Red Hat Enterprise Linux 6 服务器上加密分区，这对于移动计算机和可移动介质非常重要。LUKS 支持使用多个用户密钥对用于分区批量加密的主密钥进行解密。

以下步骤假定服务器可以访问尚未格式化或安装的新的未加密 {{site.data.keyword.blockstoragefull}} 卷。有关将 {{site.data.keyword.blockstorageshort}} 连接到 Linux 主机的更多信息，请参阅[在 Linux 上连接到 iSCSI LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)。

将使用提供者管理的静态加密自动供应在[精选数据中心](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)内供应的 {site.data.keyword.blockstorageshort}}。有关更多信息，请参阅[确保数据安全 - 提供者管理的静态加密](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)。
{:note}

## LUKS 可执行以下操作

- 对整个块设备进行加密，因此非常适合保护移动设备（例如，可移动存储介质或笔记本电脑磁盘驱动器）的内容。
- 加密块设备的底层内容是任意的，因此对于加密交换设备非常有用。该加密对于某些使用特殊格式化的块设备进行数据存储的数据库也很有用。
- 使用现有设备映射器内核子系统。
- 提供口令增强功能，以防止字典连接。
- 允许用户添加备份密钥或口令，因为 LUKS 设备包含多个密钥槽。


## LUKS 不执行以下操作

- 允许需要多个（超过 8 个）用户的应用程序对相同设备具有不同的访问密钥。
- 使用需要文件级别加密的应用程序。有关更多信息，请参阅 [RHEL 安全指南](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){: external}。

## 使用耐久性 {{site.data.keyword.blockstorageshort}} 设置 LUKS 加密卷

数据加密过程会在主机上产生负载，这可能会影响性能。
{:note}

1. 在 shell 提示符处以 root 用户身份输入以下命令来安装必需的包：<br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. 获取磁盘标识：<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. 在列表中找到您的卷。
4. 加密块设备：

   1. 此命令会初始化卷，并且您可以设置口令。<br/>

      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. 以 YES（全大写字母）进行响应。

   3. 现在设备将显示为加密卷：

      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```

5. 打开该卷并创建映射。<br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. 输入口令。
7. 验证映射并查看加密卷的状态。<br/>
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
8. 将随机数据写入加密设备上的 `/dev/mapper/cryptData`。这可确保外部世界将此视为随机数据，这意味着数据受到保护，不会泄露使用模式。此步骤可能需要一段时间。<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. 对卷进行格式化。<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. 安装卷。<br/>
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

### 安全地卸装和关闭加密卷
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### 重新安装和安装现有 LUKS 加密分区
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
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   ```
