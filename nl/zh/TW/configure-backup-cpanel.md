---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# 使用 cPanel 配置 {{site.data.keyword.blockstorageshort}} 進行備份

本文將協助您在 cPanel 中配置要儲存在 {{site.data.keyword.blockstoragefull}} 中的備份。我們假設可以使用 root 或 sudo SSH 及完整 WebHost Manager (WHM) 存取權。我們的指示以 **CentOS 7** 主機為基礎。

**附註**：您可以在[這裡](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}找到**配置備份目錄**的 cPanel 文件。

1. 透過 SSH 連接至主機。

2. 確定裝載點目標已存在。<br />
   **附註**：依預設，cPanel 系統會在本端將備份檔儲存至 `/backup` 目錄。基於本文件的用途，我們假設 `/backup` 存在且包含備份，因此我們將使用 `/backup2` 作為新的裝載點。
   
3. 依照[在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html) 的說明，配置您的 {{site.data.keyword.blockstorageshort}}。請確定您將它裝載至 `/backup2`，並在 `/etc/fstab` 中加以配置，以啟用在開機時進行裝載。

4. **選用**：將現有備份複製到新的儲存空間。您可以使用 `rsync`：
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **附註**：此指令會傳輸壓縮的資料，同時保留儘可能多的內容（但固定鏈結除外）。它提供要傳送哪些檔案的相關資訊，也會在尾端附上簡短摘要。
    
5. 登入 WebHost Manager，然後按一下**首頁** > **備份** > **備份配置**來導覽至備份配置。

6. 編輯配置以將備份儲存在新的裝載點中。 
    - 輸入新位置的絕對路徑來取代 /backup/ 目錄，以變更預設備份目錄。 
    - 選取**啟用以裝載備份磁碟機**。此設定可讓備份配置處理程序檢查 `/etc/fstab` 檔案中是否有備份裝載 (`/backup2`)。<br /> **附註**：如果存在的裝載名稱與暫置目錄相同，則「備份配置」處理程序會裝載磁碟機，並將資訊備份至該裝載。在備份處理程序完成之後，它會卸載磁碟機。 

7. 套用變更，方法是按一下**備份配置**介面底端的**儲存配置**。

8. **選用**：根據您的特定使用案例和商業需要，從伺服器中移除舊的儲存空間並從帳戶中取消。

