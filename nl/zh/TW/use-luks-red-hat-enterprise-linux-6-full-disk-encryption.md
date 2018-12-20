---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 使用 Red Hat Enterprise Linux 中的 LUKS 進行全磁碟加密

您可以將具有 Linux Unified Key Setup-on-disk-format (LUKS) 之 Red Hat Enterprise Linux 6 伺服器上的分割區加密，這在涉及行動電腦及抽取式媒體時很重要。LUKS 容許使用多個使用者金鑰來解密用於分割區大量加密的主要金鑰。

這些步驟假設伺服器可以存取尚未格式化或裝載的全新未加密 {{site.data.keyword.blockstoragefull}} 磁區。如需將 {{site.data.keyword.blockstorageshort}} 連接到 Linux 主機的相關資訊，請參閱[連接至 Linux 上的 MPIO iSCSI LUN](accessing_block_storage_linux.html)。

佈建在[選取資料中心](new-ibm-block-and-file-storage-location-and-features.html)的 {{site.data.keyword.blockstorageshort}}，會使用提供者管理的靜態加密自動佈建。如需相關資訊，請參閱[保護資料安全 - 提供者管理的靜態加密 (Encryption-At-Rest)](block-file-storage-encryption-rest.html)。
{:note}

## LUKS 可以執行的作業

- 加密整個區塊裝置，因此非常適合用來保護行動裝置的內容，例如抽取式儲存媒體或筆記型電腦磁碟機。
- 已加密區塊裝置的基礎內容是任意內容，這使它有助於加密交換裝置。加密也適用於使用特殊格式的區塊裝置進行資料儲存的特定資料庫。
- 使用現有的裝置對映器核心子系統。
- 提供通行詞組強化，以避免字典攻擊。
- 容許使用者新增備用金鑰或通行詞組，因為 LUKS 裝置包含多個金鑰槽。


## LUKS 不會執行的作業

- 容許需要多個（超過 8 個）使用者的應用程式，對相同的裝置具有不同的存取金鑰。
- 使用需要檔案層次加密的應用程式（[相關資訊 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}）。

## 使用耐久性 {{site.data.keyword.blockstorageshort}} 來設定 LUKS 加密磁區

資料加密處理程序會在主機上產生可能影響效能的負載。
{:note}

1. 在 Shell 提示上，以 root 使用者身分鍵入下列指令，以安裝必要套件：<br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. 取得磁碟 ID：<br/>
   ```
   # fdisk --l | grep /dev/mapper
   ```
   {: pre}
3. 在清單中找出您的磁區。
4. 加密區塊裝置；

   1. 這個指令會起始設定磁區，您可以設定通行詞組。<br/>

      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. 以 YES 回應（全為大寫字母）。

   3. 裝置現在會顯示為加密磁區：

      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```

5. 開啟磁區並建立對映。<br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. 輸入通行詞組。
7. 驗證對映並檢視加密磁區的狀態。<br/>
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
8. 將隨機資料寫入加密裝置上的 `/dev/mapper/cryptData`。這個動作可確保外面的世界會將此資料視為隨機資料，表示它受到保護而不會揭露使用模式。此步驟需要一些時間。<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. 格式化磁區。<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. 裝載磁區。<br/>
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

### 安全地卸載並關閉已加密的磁區
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### 重新裝載和裝載現有的 LUKS 加密分割區
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
