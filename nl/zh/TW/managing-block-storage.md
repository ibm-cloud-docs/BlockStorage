---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 管理 {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

您可以透過 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/classic){: external}來管理 {{site.data.keyword.blockstoragefull}} 磁區。從**功能表**中，選取**標準基礎架構**以與標準服務互動。

## 檢視 {{site.data.keyword.blockstorageshort}} LUN 詳細資料

您可以檢視所選取之儲存空間 LUN 的金鑰資訊摘要，包括已新增至儲存空間的額外 Snapshot 及抄寫功能。

1. 按一下**儲存空間**、**{{site.data.keyword.blockstorageshort}}**。
2. 從清單中按一下適當的「LUN 名稱」。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## 授權主機存取 {{site.data.keyword.blockstorageshort}}

「授權的」主機是已獲得特定 LUN 存取權的主機。如果沒有主機授權，就無法從系統中存取或使用儲存空間。授權主機存取 LUN 會產生使用者名稱、密碼及 iSCSI 完整名稱 (IQN)（這是裝載多路徑 I/O (MPIO) iSCSI 連線所需要的項目）。

您可以授權及連接與您的儲存空間位於相同資料中心的主機。您可以有多個帳戶，但無法授權某個帳戶的主機存取您在另一個帳戶上的儲存空間。
{:important}

1. 按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 捲動至頁面的**授權主機**區段。
3. 在右側，按一下**授權主機**。選取可以存取該特定 LUN 的主機。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to authorize.
  -v, --virtual-id TEXT     The ID of a virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of an IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

## 檢視獲授權存取 {{site.data.keyword.blockstorageshort}} LUN 的主機清單

1. 按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，然後按一下「LUN 名稱」。
2. 向下捲動至**授權主機**區段。

在這裡，您可以看到目前已獲授權存取 LUN 的主機清單。您也可以看到建立連線所需的鑑別資訊 - 使用者名稱、密碼和 IQN 主機。「目標」位址列在**儲存空間詳細資料**頁面。若為 NFS，「目標」位址會描述為 DNS 名稱，若為 iSCSI，它是「探索目標入口網站」的 IP 位址。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## 檢視主機已獲授權的 {{site.data.keyword.blockstorageshort}}

您可以檢視主機具有存取權的 LUN，包括建立連線所需的資訊 -「LUN 名稱」、「儲存空間類型」、「目標位址」、容量及位置：

1. 按一下**裝置** -> **裝置清單**，然後按一下適當的裝置。
2. 選取**儲存空間**標籤。

系統會向您呈現此特定主機具有存取權之儲存空間 LUN 的清單。此清單是依儲存空間類型（區塊、檔案、其他）分組。您可以按一下**動作**來授權更多儲存空間或移除存取權。

無法授權主機同時存取不同 OS 類型的 LUN。只能授權主機存取單一 OS 類型的 LUN。如果您嘗試授權存取多個具有不同 OS 類型的 LUN，則作業會導致錯誤。
{:note}

## 裝載及卸載 {{site.data.keyword.blockstorageshort}}

請根據您主機的「作業系統」來遵循適當的指示。

- [在 Linux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [在 CloudLinux 上連接至 LUN](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [在 Microsoft Windows 上連接至 LUNS](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [配置 Block Storage 以便使用 cPanel 進行備份](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [配置 Block Storage 以便使用 Plesk 進行備份](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## 撤銷主機對 {{site.data.keyword.blockstorageshort}} 的存取權

如果您要停止從主機存取特定儲存空間 LUN，則可以撤銷存取權。撤銷存取權時，會從 LUN 中捨棄主機連線。作業系統及應用程式無法再與 LUN 通訊。

若要避免主機端問題，請先從您的作業系統卸載儲存空間 LUN，然後再撤銷存取權，以避免遺漏磁碟機或資料毀損。
{:important}

您可以從**裝置清單**或**儲存空間視圖**中，撤銷存取權。

### 從裝置清單撤銷存取權

1. 從 [{{site.data.keyword.cloud}} 主控台](https://{DomainName}/classic){: external}中，按一下**裝置**、**裝置清單**，然後按兩下適當的裝置。
2. 選取**儲存空間**標籤。
3. 系統會向您呈現此特定主機具有存取權之儲存空間 LUN 的清單。此清單是依儲存空間類型（區塊、檔案、其他）分組。在 LUN 名稱旁，選取**動作**，然後按一下**撤銷存取權**。
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

### 透過 SLCLI 撤銷存取權。

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The ID of a hardware server to revoke authorization.
  -v, --virtual-id TEXT     The ID of a virtual server to revoke authorization.
  -i, --ip-address-id TEXT  The ID of an IP address to revoke authorization.
  -p, --ip-address TEXT     An IP address to revoke authorization.
  --help                    Show this message and exit.
```

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

或者，您可以在 SLCLI 中使用下列指令。
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```

您可以預期 LUN 在「儲存空間」清單中會保持可見至少 24 小時（立即取消）或直到週年日為止。特定特性即將無法再使用，但磁區會保持可見直到收回為止。不過，在您按一下「刪除」/「取消」之後，會立即停止計費。

作用中抄本可能會封鎖收回「儲存空間磁區」。請確定不再裝載磁區、撤銷主機權限，以及在您嘗試取消原始磁區之前取消抄寫。
