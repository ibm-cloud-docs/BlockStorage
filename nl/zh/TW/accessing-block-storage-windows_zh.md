---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 在 Microsoft Windows 上連接至 MPIO iSCSI LUN

開始之前，請確定已透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 授權正在存取 {{site.data.keyword.blockstoragefull}} 磁區的主機：

1. 從 {{site.data.keyword.blockstorageshort}} 清單頁面中，按一下與新佈建之磁區相關聯的**動作**按鈕，然後按一下**授權主機**。
2. 從清單中選取所需的主機，然後按一下**提交**；這會授權主機存取磁區。

## 如何裝載 {{site.data.keyword.blockstorageshort}} 磁區

下列是將 Windows 型「{{site.data.keyword.BluSoftlayer_full}} 運算」實例連接至多路徑輸入/輸出 (MPIO)「網際網路小型電腦系統介面 (iSCSI)」邏輯裝置號碼 (LUN) 所需的步驟。此範例以 Windows Server 2012 為基礎。您可以根據作業系統 (OS) 供應商文件來調整其他 Windows 版本的步驟。

### 配置 MPIO 特性

1. 啟動「伺服器管理程式」，並導覽至**管理**、**新增角色及特性**。
2. 按**下一步**以移至「特性」功能表。
3. 向下捲動並按一下**多路徑 I/O** 勾選框。
4. 按一下**安裝**，以在主伺服器上安裝 MPIO。
![新增角色及特性](/images/Roles_Features.png)

### 新增 MPIO 的 iSCSI 支援

1. 開啟「MPIO 內容」。若要開啟「MPIO 內容」，請按一下**開始**、指向「系統管理工具」，然後按一下 **MPIO**。
2. 按一下**探索多路徑**標籤
3. 選取**新增 iSCSI 裝置的支援**勾選框，然後按一下**新增**。系統提示您重新啟動電腦時，請按一下**是**。

**附註**：若為 Windows Server 2008，新增 iSCSI 的支援容許「Microsoft 裝置特定模組 (MSDSM)」針對 MPIO 宣告所有的 iSCSI 裝置，此舉首先需要連線至「iSCSI 目標」。

### 配置 iSCSI 起始器

1. 從「伺服器管理程式」中啟動「iSCSI 起始器」，並選取**工具**、**iSCSI 起始器**。
2. 按一下**配置**標籤。
    - 「起始器名稱」欄位可能已移入與 iqn.1991-05.com.microsoft: 類似的項目。
    - 按一下**變更**，將現有值取代為「iSCSI 完整名稱 (IQN)」。您可以從「入口網站」中的「{{site.data.keyword.blockstorageshort}} 詳細資料」畫面取得 IQN 名稱。
    ![iSCSI 起始器內容](/images/iSCSI.png)
    - 按一下**探索**標籤，然後按一下**探索入口網站**。
    - 輸入 iSCSI 目標的 IP 位址，並將「埠」保留為預設值 3260。 
    - 按一下**進階**以啟動「進階設定」視窗。
    - 選取**啟用 CHAP 登入**來開啟 CHAP 鑑別。
    ![啟用 CHAP 登入](/images/Advanced_0.png)
    **附註：**「名稱」及「目標密碼」 欄位會區分大小寫。
         - 刪除「名稱」欄位中的任何現有項目，然後輸入來自入口網站的使用者名稱。
         - 在「目標密碼」欄位中，輸入來自入口網站的密碼。<br/>
         **附註：**您可以從入口網站中的「{{site.data.keyword.blockstorageshort}} 詳細資料」畫面取得「名稱」及「目標密碼」值，分別作為「使用者名稱」及「密碼」。
    - 在「進階設定」及「探索目標入口網站」視窗上，按一下**確定**，以回到主要「iSCSI 起始器內容」畫面。如果您收到鑑別錯誤，請重新檢查使用者名稱及密碼項目。
    ![非作用中目標](/images/Inactive_0.png)
    請注意，您的目標名稱會出現在「已探索目標」區段中，並處於「非作用中」狀態。 
    
    下列步驟顯示如何啟動目標。
    
### 啟動目標

1. 按一下**連接**來連接至目標。
2. 選取**啟用多路徑**勾選框，以啟用目標的多路徑 IO。
![啟用多路徑](/images/Connect_0.png)
3. 按一下**進階**，然後選取**啟用 CHAP 登入**。
![啟用 CHAP](/images/chap_0.png)
4. 在「名稱」欄位中輸入使用者名稱，然後在「目標密碼」欄位中輸入密碼。<br/>
**附註：**您可以從入口網站中的「{{site.data.keyword.blockstorageshort}} 詳細資料」畫面取得「名稱」及「目標密碼」欄位值，分別作為「使用者名稱」及「密碼」。
5. 按一下**確定**，直到顯示「iSCSI 起始器內容」視窗。「已探索目標」區段中的目標狀態會從「非作用中」變更為「已連接」。
![已連接狀態](/images/Connected.png) 


### 在 iSCSI 起始器中配置 MPIO

1. 啟動「iSCSI 起始器」，然後在「目標」標籤上，按一下**內容**。
2. 在「內容」視窗上按一下**新增階段作業**，以啟動「連接至目標」視窗。
3. 選取**啟用多路徑**勾選框，然後按一下**進階...**。
  ![目標](/images/Target.png) 
  
4. 在「進階設定」視窗中
   - 將「預設值」保留為「本端配接器」和「起始器 IP」欄位的值。對於具有多個介面連至 iSCSI 的主伺服器，您需要為「起始器 IP」欄位選擇適當的值。
   - 從「目標」入口網站 IP 下拉清單中，選取 iSCSI 儲存空間的 IP。
   - 按一下**啟用 CHAP 登入**勾選框。
   - 輸入從入口網站取得的「名稱」及「目標密碼」值，然後按一下**確定**。
   - 在「連接至目標」視窗上按一下**確定**，以回到「內容」視窗。現在，「內容」視窗應該在 ID 視窗內顯示多個階段作業。您目前有多個階段作業連至 iSCSI 儲存空間。
   ![設定](/images/Settings.png) 
   
5. 在「內容」視窗中，按一下**裝置**，然後啟動「裝置」視窗。裝置介面名稱應該在裝置名稱的開頭處具有 mpio。<br/>
  ![裝置](/images/Devices.png) 
  
6. 按一下 **MPIO** 以啟動「裝置詳細資料」視窗。此視窗可讓您選擇 MPIO 的負載平衡原則，同時也會顯示 iSCSI 的路徑。在此範例中，會顯示兩個路徑，供具有「以子網路循環配置資源」負載平衡原則的 MPIO 使用。
  ![DeviceDetails](/images/DeviceDetails.png)
  
7. 按一下**確定**數次，以結束「iSCSI 起始器」。



## 如何驗證是否在 Windows OS 中正確地設置 MPIO

若要驗證是否已配置 Windows MPIO，首先您必須確定「MPIO 附加程式」已啟用，而且已完成必要的重新開機：

![Roles_Features_0](/images/Roles_Features_0.png)

一旦重新開機完成並新增了「效能儲存裝置」後，我們就可以驗證 MPIO 是否已配置且正在運作中。若要這樣做，請查看**目標裝置詳細資料**，然後按一下 **MPIO**：
![DeviceDetails_0](/images/DeviceDetails_0.png)

如果未在「效能儲存裝置」上配置 MPIO，且「{{site.data.keyword.BluSoftlayer_full}} 團隊」執行「維護」或發生「網路中斷」，則您的「儲存裝置」將中斷連線並變成無法使用。MPIO 將確保在那些事件發生期間有額外的連線功能層次，並保持已建立的階段作業對 LUN 具有作用中的讀取/寫入。

## 如何卸載 {{site.data.keyword.blockstorageshort}} 磁區

下列是將 Windows 型 Bluemix 運算實例與 MPIO iSCSI LUN 中斷連線所需的步驟。此範例以 Windows Server 2012 為基礎。您可以根據 OS 供應商文件來調整其他 Windows 版本的步驟。

### 啟動「iSCSI 起始器」。

1. 按一下**目標**標籤。
2. 選取您要移除的目標，然後按一下**中斷連線**。

### 如果您不再需要存取 iSCSI 目標，則為選用項目：

1. 按一下「iSCSI 起始器」中的**探索**標籤。
2. 強調顯示與儲存空間磁區相關聯的目標入口網站，然後按一下**移除**。
