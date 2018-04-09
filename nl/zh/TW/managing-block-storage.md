---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# 管理 {{site.data.keyword.blockstorageshort}}

您可以透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 來管理 {{site.data.keyword.blockstoragefull}} 磁區。本文章提供最常見作業的指示。

## 請參閱佈建的 {{site.data.keyword.blockstorageshort}} LUN 詳細資料。

您可以檢視所選取之儲存空間 LUN 的金鑰資訊摘要，包括其他已新增至儲存空間之 Snapshot 及抄寫功能。

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
2. 從清單中按一下適當的「LUN 名稱」。

## 授權主機存取 {{site.data.keyword.blockstorageshort}}

「授權」主機是已獲授與特定 LUN 存取權的主機。如果沒有主機授權，就無法從系統中存取或使用儲存空間。授權主機存取 LUN 會產生「使用者名稱」、「密碼」及 iSCSI 完整名稱 (IQN)（這是裝載多路徑 I/O (MPIO) iSCSI 連線所需要的項目）。

**附註**：您只能授權及連接與儲存空間位於相同資料中心的主機。如果您有多個帳戶，則無法授權某個帳戶的主機存取您在另一個帳戶上的儲存空間。

1. 按一下**儲存空間** -> **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 捲動至頁面的「授權主機」區段。
3. 按一下頁面右側的**授權主機**鏈結。選取可以存取該特定 LUN 的主機。

 

## 檢視已獲 {{site.data.keyword.blockstorageshort}} LUN 授權之主機的清單

請使用下列步驟，檢視已獲 LUN 授權之主機的清單。

1. 按一下**儲存空間** -> **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 向下捲動至頁面底端的「授權主機」區段。

在這裡，您會看到目前已獲授權存取 LUN 的主機清單，尤其對於 iSCSI，需要鑑別資訊才能建立連線 -「使用者名稱」、「密碼」及「IQN 主機」。「目標」位址位於「儲存空間詳細資料」頁面頂端。若為 NFS，其會描述為 DNS 名稱，若為 iSCSI，則會描述為「探索目標入口網站」的 IP 位址。

 

## 檢視主機已獲授權的 {{site.data.keyword.blockstorageshort}}

您可以檢視主機具有存取權的 LUN，包括建立連線所需的資訊 -「LUN 名稱」、「儲存空間類型」、「目標位址」、容量及位置：

1. 從 [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} 中，按一下**裝置** -> **裝置清單**，然後按一下適當的裝置。
2. 選取**儲存空間**標籤。

接著，會呈現此特定主機具有存取權之儲存空間 LUN 的清單，全部都依儲存空間類型（區塊、檔案、其他）分組。從各自的「動作」功能表中，您可以授權其他儲存空間或移除存取權。

 

## 裝載及卸載 {{site.data.keyword.blockstorageshort}}

請參閱下列文章，其中含有從主機裝載及卸載 {{site.data.keyword.blockstorageshort}} 的詳細資料。

- [Linux 上的 {{site.data.keyword.blockstorageshort}}](accessing_block_storage_linux.html)

- [Microsoft Windows 上的 {{site.data.keyword.blockstorageshort}}](accessing-block-storage-windows.html)

 

## 撤銷主機對 {{site.data.keyword.blockstorageshort}} 的存取權

如果您要停止從主機存取特定儲存空間 LUN，則可以撤銷存取權。撤銷存取權時，將從 LUN 中捨棄主機連線，而作業系統及應用程式都無法與 LUN 通訊。

**附註**：若要避免主機端問題，請先從您的作業系統卸載儲存空間 LUN，然後再撤銷存取權，以避免遺漏磁碟機或資料毀損。

您可以從「裝置清單」或「儲存空間」視圖中，撤銷「儲存空間」的存取權。

### 從「裝置清單」撤銷存取權：

1. 從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中，按一下**裝置**、**裝置清單**，然後按兩下適當的裝置。
2. 選取**儲存空間**標籤。
3. 接著，會呈現此特定主機具有存取權之儲存空間 LUN 的清單，全部都依儲存空間類型（區塊、檔案、其他）分組。選取您要撤銷存取權之 LUN 旁邊的個別「動作」功能表，然後按一下**撤銷存取權**。
4. 系統將詢問您是否要撤銷 LUN 的存取權，因為該動作無法復原。按一下**是**以撤銷 LUN 存取權，或按一下**否**以取消動作。

**附註**：如果想要從特定主機中斷多個 LUN 的連線，您將需要針對每一個 LUN 重複「撤銷存取權」動作。


### 從「儲存空間視圖」撤銷存取權：

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**，然後選取您要撤銷存取權的 LUN。
2. 向下捲動至頁面的**授權主機**區段。
3. 按一下要撤銷其存取權之主機旁邊的**動作**下拉箭頭，然後選取**撤銷存取權**。
4. 系統將詢問您是否要撤銷 LUN 的存取權，因為該動作無法復原。按一下**是**以撤銷 LUN 存取權，或按一下**否**以取消動作。

**附註**：如果想要從特定 LUN 中斷多個主機的連線，您將需要針對每一個主機重複「撤銷存取權」動作。

 

## 取消儲存空間 LUN

如果不再需要特定 LUN，您可以取消它。為了取消儲存空間 LUN，首先需要撤銷任何主機的存取權。

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
2. 按一下要取消之 LUN 的**動作**下拉箭頭，然後選取**取消 {{site.data.keyword.blockstorageshort}}**。
3. 系統將要求您確認要立即還是在佈建 LUN 的紀念日取消 LUN。按一下**繼續**或**關閉**。
**附註**：如果選取要在其紀念日取消 LUN 的選項，您可以在其紀念日之前使取消要求作廢。
4. 按一下**確認通知**勾選框，然後按一下**確認**。

 

