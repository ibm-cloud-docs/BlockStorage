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


# 在 Linux 上連接至 MPIO iSCSI LUN

這些指示主要適用於 RHEL6 和 CentOS6。我們已為其他 OS 新增附註，但本文件**並未**涵蓋所有 Linux 發行套件。如果您使用其他 Linux 作業系統，則請參閱特定發行套件的文件，並確保多路徑支援 ALUA 以設定路徑優先順序。
{:note}

例如，您可以在[這裡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} 找到 Ubuntu 的「iSCSI 起始器配置」指示，以及在[這裡 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window} 找到 Ubuntu 的「DM 多路徑」設定指示。
{: tip}

開始之前，請確定存取 {{site.data.keyword.blockstoragefull}} 磁區的主機先前已透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 獲得授權。
{:important}

1. 從 {{site.data.keyword.blockstorageshort}} 的清單頁面中，找出新的磁區，然後按一下**動作**。
2. 按一下**授權主機**。
3. 從清單中，選取可以存取磁區的主機，然後按一下**提交**。

## 裝載 {{site.data.keyword.blockstorageshort}} 磁區

以下是將 Linux 型「{{site.data.keyword.BluSoftlayer_full}} 運算」實例連接至多路徑輸入/輸出 (MPIO)「網際網路小型電腦系統介面 (iSCSI)」邏輯裝置號碼 (LUN) 所需的步驟。

指示中所參照的「主機 IQN」、使用者名稱、密碼及目標位址，可從 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 中的 **{{site.data.keyword.blockstorageshort}} 詳細資料**畫面取得。
{: tip}

最好是在 VLAN 上執行儲存空間資料流量，這樣會略過防火牆。透過軟體防火牆執行儲存空間資料流量，會增加延遲，而且會對儲存空間效能造成不利的影響。
{:important}

1. 將 iSCSI 及多路徑公用程式安裝至主機。
  - RHEL 及 CentOS
     ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu 及 Debian

    ```
    sudo apt-get update
    sudo apt-get install multipath-tools
    ```
    {: pre}

2. 建立或編輯您所需要的多路徑配置檔。
  - RHEL 6 及 CENTOS 6
    * 編輯含有下列最少配置的 **/etc/multipath.conf**。

      ```
      defaults {
      user_friendly_names no
      max_fds max
      flush_on_last_del yes
      queue_without_daemon no
      dev_loss_tmo infinity
      fast_io_fail_tmo 5
      }
      # All data under blacklist must be specific to your system.
      blacklist {
      wwid "SAdaptec*"
      devnode "^hd[a-z]"
      devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]*"
      devnode "^cciss.*"  
      }
      devices {
      device {
      vendor "NETAPP"
      product "LUN"
      path_grouping_policy group_by_prio
      features "3 queue_if_no_path pg_init_retries 50"
      prio "alua"
      path_checker tur
      failback immediate
      path_selector "round-robin 0"
      hardware_handler "1 alua"
      rr_weight uniform
      rr_min_io 128
      }
      }
      ```
      {: codeblock}

    - 重新啟動 `iscsi` 和 `iscsid` 服務，讓變更生效。

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - RHEL7 及 CentOS7 的 `multipath.conf` 可以是空白，因為 OS 具有內建配置。
  - Ubuntu 不使用 `multipath.conf`，因為它已內建到 `multipath-tools`。

3. 載入多路徑模組、啟動多路徑服務，並將其設為在開機時啟動。
  - RHEL 6
     ```
     modprobe dm-multipath
     ```
    {: pre}

    ```
     service multipathd start
     ```
    {: pre}

    ```
     chkconfig multipathd on
     ```
    {: pre}

  - CentOS 7
     ```
     modprobe dm-multipath
     ```
    {: pre}

    ```
     systemctl start multipathd
     ```
    {: pre}

    ```
     systemctl enable multipathd
     ```
    {: pre}

  - Ubuntu
     ```
     service multipath-tools start
     ```
    {: pre}

  - 若為其他發行套件，請參閱 OS 供應商文件。

4. 驗證多路徑正在運作中。
  - RHEL 6
     ```
    multipath -l
    ```
    {: pre}

    如果傳回空白，表示它正在運作中。
  - CentOS 7
     ```
     multipath -ll
     ```
    {: pre}

    RHEL 7 和 CentOS 7 可能會傳回「無 fc_host 裝置」，可以忽略此訊息。

5. 使用來自 {{site.data.keyword.slportal}} 的 IQN 更新 `/etc/iscsi/initiatorname.iscsi` 檔案。請以小寫字體輸入此值。
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. 使用來自 {{site.data.keyword.slportal}} 的使用者名稱及密碼，編輯 `/etc/iscsi/iscsid.conf` 中的 CHAP 設定。請對 CHAP 名稱使用大寫字體。
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   請將其他 CHAP 設定保持註解狀態。{{site.data.keyword.BluSoftlayer_full}} 儲存空間僅會使用單向鑑別。請勿啟用 Mutual CHAP。
   {:important}

7. 將 iSCSI 設為在開機時啟動，並立即啟動。
  - RHEL 6
     ```
    chkconfig iscsi on
    ```
    {: pre}

    ```
    chkconfig iscsid on
    ```
    {: pre}

    ```
    service iscsi start
    ```
    {: pre}

    ```
    service iscsid start
    ```
    {: pre}

  - CentOS 7
    ```
    systemctl enable iscsi
    ```
    {: pre}

    ```
    systemctl enable iscsid
    ```
    {: pre}

    ```
    systemctl start iscsi
    ```
    {: pre}

    ```
    systemctl start iscsid
    ```
    {: pre}

   - 若為其他發行套件，請參閱 OS 供應商文件。

8. 使用從 {{site.data.keyword.slportal}} 取得的「目標」IP 位址來探索裝置。

   A. 針對 iSCSI 陣列執行探索。
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
    ```
    {: pre}

   B. 將主機設為自動登入 iSCSI 陣列。
    ```
    iscsiadm -m node -L automatic
    ```
    {: pre}

9. 驗證主機已登入 iSCSI 陣列，並維護其階段作業。
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   這個指令會報告路徑。

10. 驗證裝置已連接。依預設，裝置將連接至 `/dev/mapper/mpathX`，其中 X 是所連接裝置的產生 ID。
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  這個指令會報告與下列範例類似的項目。
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```
  磁區現在已裝載且可在主機上存取。

## 建立檔案系統（選用）

請遵循這些步驟，以便在新裝載的磁區上建立檔案系統。大部分應用程式都需要檔案系統，才能使用磁區。請對小於 2 TB 的磁碟使用 `fdisk`，而對大於 2 TB 的磁碟使用 `parted`。

### 使用 `fdisk` 建立檔案系統

1. 取得磁碟名稱。
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   傳回的磁碟名稱看起來類似於 `/dev/mapper/XXX`。

2. 在磁碟上建立分割區。

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX 代表步驟 1 中傳回的磁碟名稱。<br />

   進一步向下捲動，以取得 `fdisk` 指令表格中列出的指令程式碼。
   {: tip}

3. 在新的分割區上建立檔案系統。

   ```
   fdisk -l /dev/mapper/XXX
   ```
   {: pre}

   - 新的分割區會與磁碟一起列出，類似於 `XXXlp1`，後面接著大小、類型 (83) 及 Linux。
   - 記下分割區名稱，您將在下一步中需要它。（XXXlp1 代表分割區名稱。）
   - 建立檔案系統：

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. 為檔案系統建立裝載點，並裝載它。
   - 建立一個分割區名稱 `PerfDisk`，或您要裝載檔案系統的位置。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - 使用分割區名稱來裝載儲存空間。
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 確認您在清單中看到新的檔案系統。
     ```
     df -h
     ```
     {: pre}

5. 將新的檔案系統新增至系統的 `/etc/fstab` 檔案，以在開機時啟用自動裝載。
   - 將下行附加至 `/etc/fstab` 的結尾（使用步驟 3 中的分割區名稱）。<br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### `fdisk` 指令表格

<table border="0" cellpadding="0" cellspacing="0">
	<caption><code>fdisk</code> 指令表格包含左側的指令以及右側的預期結果。</caption>
    <thead>
	<tr>
		<th style="width:40%;">指令</th>
		<th style="width:60%;">結果</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Command: n</code></td>
		<td>建立分割區。&#42;</td>
	</tr>
	<tr>
		<td><code>Command action: p</code></td>
		<td>使分割區成為主要分割區。</td>
	</tr>
	<tr>
		<td><code>Partition number (1-4): 1</code></td>
		<td>變成磁碟上的分割區 1。</td>
	</tr>
	<tr>
		<td><code>First cylinder (1-8877): 1 (default)</code></td>
		<td>從磁柱 1 開始。</td>
	</tr>
	<tr>
		<td><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></td>
		<td>按 Enter 鍵以移至最後一個磁柱。</td>
	</tr>
	<tr>
		<td><code>Command: t</code></td>
		<td>設定分割區的類型。&#42;</td>
	</tr>
	<tr>
		<td><code>Select partition 1.</code></td>
		<td>選取要設為特定類型的分割區 1。</td>
	</tr>
	<tr>
		<td><code>Hex code: 83</code></td>
		<td>選取 Linux 作為「類型」（83 是適用於 Linux 的十六進位碼）。&#42;&#42;</td>
	 </tr>
	<tr>
		<td><code>Command: w</code></td>
		<td>將新的分割區資訊寫入磁碟中。&#42;</td>
	</tr>
   </tbody>
</table>

  (`*`) 鍵入 m 以取得「說明」。

  (`**`) 鍵入 L 以列出十六進位碼。

### 使用 `parted` 建立檔案系統

在許多 Linux 發行套件上，會預先安裝 `parted`。若其未包含在您的發行套件中，則可以使用下列方式來安裝它：
- Debian 及 Ubuntu
  ```
  sudo apt-get install parted
  ```
  {: pre}

- RHEL 及 CentOS
  ```
  yum install parted
  ```
  {: pre}


若要使用 `parted` 來建立檔案系統，請遵循下列步驟。

1. 執行 `parted`。

   ```
   parted
   ```
   {: pre}

2. 在磁碟上建立分割區。
   1. 除非另有指定，否則 `parted` 會使用您的主要磁碟機，在大部分情況下是 `/dev/sda`。使用 **select** 指令，切換至您要分割的磁碟。將 **XXX** 取代為新的裝置名稱。

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. 執行 `print`，以確認您位在正確的磁碟上。

      ```
      print
      ```
      {: pre}

   3. 建立 GPT 分割區表格。

      ```
      mklabel gpt
      ```
      {: pre}

   4. `Parted` 可以用來建立主要及邏輯磁碟分割區，涉及的步驟相同。若要建立分割區，`parted` 會使用 `mkpart`。您可以為它提供其他參數，如 **primary** 或 **logical**，視您想要建立的分割區類型而定。<br />

   列出的單位預設為百萬位元組 (MB)。若要建立 10 GB 分割區，請從 1 開始，並在 10000 結束。想要的話，您也可以輸入 `unit TB`，將大小單位變更為兆位元組 (TB)。
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. 使用 `quit` 來結束 `parted`。

      ```
      quit
      ```
      {: pre}

3. 在新的分割區上建立檔案系統。

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   在執行這個指令時，務必選取正確的磁碟及分割區。<br />請列印分割區表格來驗證結果。在檔案系統直欄下，您可以看到 ext3。
   {:important}

4. 為檔案系統建立裝載點，並裝載它。
   - 建立一個分割區名稱 `PerfDisk`，或您要裝載檔案系統的位置。

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - 使用分割區名稱來裝載儲存空間。

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - 確認您在清單中看到新的檔案系統。

     ```
     df -h
     ```
     {: pre}

5. 將新的檔案系統新增至系統的 `/etc/fstab` 檔案，以在開機時啟用自動裝載。
   - 將下行附加至 `/etc/fstab` 的結尾（使用步驟 3 中的分割區名稱）。<br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## 驗證 MPIO 配置

1. 若要檢查多路徑是否正在挑選裝置，請列出裝置。如果配置正確，則只會顯示兩台 NETAPP 裝置。

  ```
   multipath -l
   ```
  {: pre}

  ```
root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
7:0:0:101 sde 8:64 active undef running
```

2. 檢查磁碟是否存在。必須有兩個具有相同 ID 的磁碟，以及大小相同且具有相同 ID 的 `/dev/mapper` 清單。`/dev/mapper` 裝置是多路徑設定的裝置。
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - 正確配置的輸出範例。

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```
  - 不正確配置的輸出範例。

    ```
No multipath output root@server:~# multipath -l root@server:~#
```

    ```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```

3. 確認本端磁碟未包含在多路徑裝置中。下列指令顯示列入黑名單的裝置。
   ```
   multipath -l -v 3 | grep sd <date and time>
   ```
   {: pre}

   ```
root@server:~# multipath -l -v 3 | grep sd Feb 17 19:55:02
| sda: device node name blacklisted Feb 17 19:55:02
| sdb: device node name blacklisted Feb 17 19:55:02
| sdc: device node name blacklisted Feb 17 19:55:02
| sdd: device node name blacklisted Feb 17 19:55:02
| sde: device node name blacklisted Feb 17 19:55:02
```

## 卸載 {{site.data.keyword.blockstorageshort}} 磁區

1. 卸載檔案系統。
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. 如果您在該目標入口網站中沒有任何其他磁區，可登出該目標。
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. 如果您在該目標入口網站中沒有任何其他磁區，請刪除目標入口網站記錄，以防止未來的登入嘗試。
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   如需相關資訊，請參閱 [`iscsiadm` 手冊 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://linux.die.net/man/8/iscsiadm)。
   {:tip}
