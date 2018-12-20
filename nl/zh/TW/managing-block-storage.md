---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 管理 {{site.data.keyword.blockstorageshort}}

您可以透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 來管理 {{site.data.keyword.blockstoragefull}} 磁區。

## 檢視 {{site.data.keyword.blockstorageshort}} LUN 詳細資料

您可以檢視所選取之儲存空間 LUN 的金鑰資訊摘要，包括已新增至儲存空間的額外 Snapshot 及抄寫功能。

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
2. 從清單中按一下適當的「LUN 名稱」。

## 授權主機存取 {{site.data.keyword.blockstorageshort}}

「授權的」主機是已獲得特定 LUN 存取權的主機。如果沒有主機授權，就無法從系統中存取或使用儲存空間。授權主機存取 LUN 會產生使用者名稱、密碼及 iSCSI 完整名稱 (IQN)（這是裝載多路徑 I/O (MPIO) iSCSI 連線所需要的項目）。

您可以授權及連接與您的儲存空間位於相同資料中心的主機。您可以有多個帳戶，但無法授權某個帳戶的主機存取您在另一個帳戶上的儲存空間。
{:important}

1. 按一下**儲存空間** -> **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 捲動至頁面的**授權主機**區段。
3. 在右側，按一下**授權主機**。選取可以存取該特定 LUN 的主機。



## 檢視獲授權存取 {{site.data.keyword.blockstorageshort}} LUN 的主機清單

1. 按一下**儲存空間** -> **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 向下捲動至**授權主機**區段。

在這裡，您可以看到目前已獲授權存取 LUN 的主機清單。您也可以看到建立連線所需的鑑別資訊 - 使用者名稱、密碼和 IQN 主機。「目標」位址列在**儲存空間詳細資料**頁面。若為 NFS，「目標」位址會描述為 DNS 名稱，若為 iSCSI，它是「探索目標入口網站」的 IP 位址。



## 檢視主機已獲授權的 {{site.data.keyword.blockstorageshort}}

您可以檢視主機具有存取權的 LUN，包括建立連線所需的資訊 -「LUN 名稱」、「儲存空間類型」、「目標位址」、容量及位置：

1. 在 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](http://control.softlayer.com/){:new_window} 中按一下**裝置** -> **裝置清單**，然後按一下適當的裝置。
2. 選取**儲存空間**標籤。

系統會向您呈現此特定主機具有存取權之儲存空間 LUN 的清單。此清單是依儲存空間類型（區塊、檔案、其他）分組。您可以按一下**動作**來授權更多儲存空間或移除存取權。



## 裝載及卸載 {{site.data.keyword.blockstorageshort}}

請根據您主機的「作業系統」來遵循適當的指示。

- [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上連接至 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [配置 Block Storage 以便使用 cPanel 進行備份](configure-backup-cpanel.html)
- [配置 Block Storage 以便使用 Plesk 進行備份](configure-backup-plesk.html)


## 撤銷主機對 {{site.data.keyword.blockstorageshort}} 的存取權

如果您要停止從主機存取特定儲存空間 LUN，則可以撤銷存取權。撤銷存取權時，會從 LUN 中捨棄主機連線。作業系統及應用程式無法再與 LUN 通訊。

若要避免主機端問題，請先從您的作業系統卸載儲存空間 LUN，然後再撤銷存取權，以避免遺漏磁碟機或資料毀損。
{:important}

您可以從**裝置清單**或**儲存空間視圖**中，撤銷存取權。

### 從裝置清單撤銷存取權

1. 從 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 按一下**裝置**、**裝置清單**，然後按兩下適當的裝置。
2. 選取**儲存空間**標籤。
3. 系統會向您呈現此特定主機具有存取權之儲存空間 LUN 的清單。此清單是依儲存空間類型（區塊、檔案、其他）分組。在 LUN 名稱旁，選取**動作**，然後按一下「撤銷存取權」。
4. 確認您要撤銷 LUN 的存取權，因為該動作無法復原。按一下**是**以撤銷 LUN 存取權，或按一下**否**以取消動作。

如果您要中斷多個 LUN 與特定主機的連線，則需要針對每一個 LUN 重複「撤銷存取權」動作。
{:tip}


### 從儲存空間視圖撤銷存取權

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**，然後選取您要撤銷存取權的 LUN。
2. 捲動至**授權主機**。
3. 按一下要撤銷其存取權之主機旁的**動作**，然後選取**撤銷存取權**。
4. 確認您要撤銷 LUN 的存取權，因為該動作無法復原。按一下**是**以撤銷 LUN 存取權，或按一下**否**以取消動作。

如果您要中斷多個 LUN 與特定主機的連線，則需要針對每一個主機重複「撤銷存取權」動作。
{:tip}



## 取消儲存空間 LUN

如果不再需要特定 LUN，您可以隨時將它取消。

為了取消儲存空間 LUN，首先需要撤銷任何主機的存取權。
{:important}

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
2. 按一下要取消之 LUN 的**動作**，然後選取**取消 {{site.data.keyword.blockstorageshort}}**。
3. 確認要立即取消 LUN，還是在佈建 LUN 的週年日取消 LUN。

   如果您選取在其週年日取消 LUN 的選項，則可以在其週年日之前使取消要求失效。
   {:tip}
4. 按一下**繼續**或**關閉**。
5. 按一下**確認通知**勾選框，然後按一下**確認**。
