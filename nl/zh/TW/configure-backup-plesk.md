---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 配置 {{site.data.keyword.blockstorageshort}} 以便使用 Plesk 進行備份

請使用這些指示，配置 {{site.data.keyword.blockstoragefull}} 以便在 Plesk 裡進行備份。我們假設可以使用 root 或 sudo SSH 及完整管理層次 Plesk 存取權。這些指示以 CentOS7 主機為基礎。

如需相關資訊，請參閱 [Plesk 的備份及還原文件 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}。
{:tip}

1. 透過 SSH 連接至主機。
2. 確定裝載點目標已存在。

   Plesk 有兩個用來儲存備份的選項。一個選項是內部 Plesk 儲存空間（位於 Plesk 伺服器上的備份儲存空間）。另一個選項是外部 FTP 儲存空間（位於 Web 或本端網路中某個外部伺服器上的備份儲存空間）。通常，在 Plesk 機器上，內部備份儲存在 `/var/lib/psa/dumps` 中，並使用 `/tmp` 作為暫存目錄。在此範例中，暫存目錄會保留在本端，但 dumps 目錄會移至 {{site.data.keyword.blockstorageshort}} 目標 (`/backup/psa/dumps`)。不需要任何 FTP 使用者認證。
   {:note}   
3. 依照[在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html) 的說明，配置您的 {{site.data.keyword.blockstorageshort}}。將 {{site.data.keyword.blockstorageshort}} 裝載至 `/backup`，並配置 `/etc/fstab` 以啟用在啟動時進行裝載。
4. **選用**：將現有備份複製到新的儲存空間。您可以使用 `rsync`。
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    這個指令會壓縮並傳輸您的資料，並儘可能保留越多內容（但固定鏈結除外）。它提供要傳送哪些檔案的相關資訊，也會在尾端附上簡短摘要。
    {:tip}    
5. 編輯 `/etc/psa/psa.conf`，以將 `DUMP_D` 值指向新目標。
    - 它會顯示為：`DUMP_D /backup/psa/dumps`。
6. **選用**：根據您的特定使用案例和商業需要，從伺服器中移除舊的儲存空間並從帳戶中取消。
