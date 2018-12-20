---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 在 Microsoft Windows 上連接至 MPIO iSCSI LUN

開始之前，請確定存取 {{site.data.keyword.blockstoragefull}} 磁區的主機已透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 獲得授權。

1. 從 {{site.data.keyword.blockstorageshort}} 的清單頁面中，找出新的磁區，然後按一下**動作**。按一下**授權主機**。
2. 從清單中，選取要存取磁區的主機，然後按一下**提交**。

## 裝載 {{site.data.keyword.blockstorageshort}} 磁區

以下是將 Windows 型「{{site.data.keyword.BluSoftlayer_full}} 運算」實例連接至多路徑輸入/輸出 (MPIO)「網際網路小型電腦系統介面 (iSCSI)」邏輯裝置號碼 (LUN) 所需的步驟。此範例以 Windows Server 2012 為基礎。您可以根據作業系統 (OS) 的供應商文件來調整其他 Windows 版本的步驟。

### 配置 MPIO 特性

1. 啟動「伺服器管理程式」，並瀏覽至**管理**、**新增角色及特性**。
2. 按**下一步**以移至「特性」功能表。
3. 向下捲動，並勾選**多路徑 I/O**。
4. 按一下**安裝**，以在主伺服器上安裝 MPIO。
![在伺服器管理程式中新增角色及特性](/images/Roles_Features.png)

### 新增 MPIO 的 iSCSI 支援

1. 按一下**開始**，並指向**系統管理工具**，然後按一下 **MPIO**，以開啟「MPIO 內容」視窗。
2. 按一下**探索多路徑**。
3. 勾選**新增 iSCSI 裝置的支援**，然後按一下**新增**。系統提示您重新啟動電腦時，請按一下**是**。

在 Windows Server 2008 中，新增 iSCSI 的支援可容許「Microsoft 裝置特定模組 (MSDSM)」針對 MPIO 宣告所有的 iSCSI 裝置，此舉需要先連線至「iSCSI 目標」。
{:note}

### 配置 iSCSI 起始器

1. 從「伺服器管理程式」中啟動「iSCSI 起始器」，並選取**工具**、**iSCSI 起始器**。
2. 按一下**配置**標籤。
    - 「起始器名稱」欄位可能已移入與 `iqn.1991-05.com.microsoft:` 類似的項目。
    - 按一下**變更**，將現有值取代為「iSCSI 完整名稱 (IQN)」。
    ![iSCSI 起始器內容](/images/iSCSI.png)

      您可以從 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 中的「{{site.data.keyword.blockstorageshort}} 詳細資料」畫面取得 IQN 名稱。
      {: tip}

    - 按一下**探索**標籤，然後按一下**探索入口網站**。
    - 輸入 iSCSI 目標的 IP 位址，並將「埠」保留為預設值 3260。
    - 按一下**進階**，以開啟「進階設定」視窗。
    - 選取**啟用 CHAP 登入**來開啟 CHAP 鑑別。
    ![啟用 CHAP 登入](/images/Advanced_0.png)
        「名稱」及「目標密碼」欄位有區分大小寫。
    {:important}
         - 在**名稱**欄位中，刪除任何現有的項目，並從 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 輸入使用者名稱。
         - 在**目標密碼**欄位中，輸入 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 提供的密碼。
    - 在**進階設定**及**探索目標入口網站**視窗上，按一下**確定**，以回到主要「iSCSI 起始器內容」畫面。如果您收到鑑別錯誤，請檢查使用者名稱及密碼項目。
    ![非作用中目標](/images/Inactive_0.png)
        您的目標名稱會出現在「已探索目標」區段中，並處於`非作用中`狀態。
    {:note}


### 啟動目標

1. 按一下**連接**來連接至目標。
2. 選取**啟用多路徑**勾選框，以啟用目標的多路徑 IO。
</br>
   ![啟用多路徑](/images/Connect_0.png)
3. 按一下**進階**，然後選取**啟用 CHAP 登入**。
</br>
   ![啟用 CHAP](/images/chap_0.png)
4. 在「名稱」欄位中輸入使用者名稱，然後在「目標密碼」欄位中輸入密碼。

   您可以從「{{site.data.keyword.blockstorageshort}} 詳細資料」畫面取得「名稱」及「目標密碼」欄位值。
   {:tip}
5. 按一下**確定**，直到顯示 **iSCSI 起始器內容**視窗。**已探索目標**區段中的目標狀態會從**非作用中**變更為**已連接**。
</br>
   ![已連接狀態](/images/Connected.png)


### 在 iSCSI 起始器中配置 MPIO

1. 啟動「iSCSI 起始器」，然後在「目標」標籤上，按一下**內容**。
2. 在「內容」視窗上按一下**新增階段作業**，以開啟「連接至目標」視窗。
3. 在「連接至目標」對話框中，選取**啟用多路徑**勾選框，然後按一下**進階**。
  ![目標](/images/Target.png)

4. 在「進階設定」視窗![設定](/images/Settings.png)
   - 在「本端配接卡」清單中，選取「Microsoft iSCSI 起始器」。
   - 在「起始器 IP」清單中，選取主機的 IP 位址。
   - 在「目標入口網站 IP」清單中，選取裝置介面的 IP。
   - 按一下**啟用 CHAP 登入**勾選框。
   - 輸入從入口網站取得的「名稱」及「目標密碼」值，然後按一下**確定**。
   - 在「連接至目標」視窗上按一下**確定**，以回到「內容」視窗。

5. 按一下**內容**。在「內容」對話框中，再次按一下**新增階段作業**，以新增第二個路徑。
6. 在「連接至目標」視窗中，選取**啟用多路徑**勾選框。按一下**進階**。
7. 在「進階設定」視窗中，
   - 在「本端配接卡」清單中，選取「Microsoft iSCSI 起始器」。
   - 在「起始器 IP」清單中，選取對應於主機的 IP 位址。在此情況下，您會將裝置上的兩個網路介面連接至主機上的單一網路介面。因此，這個介面與為第一個階段作業所提供的介面相同。
   - 在「目標入口網站 IP」清單中，選取裝置上已啟用之第二個資料介面的 IP 位址。
   - 按一下**啟用 CHAP 登入**勾選框。
   - 輸入從入口網站取得的「名稱」及「目標密碼」值，然後按一下**確定**。
   - 在「連接至目標」視窗上按一下**確定**，以回到「內容」視窗。
8. 現在，「內容」視窗會在 ID 窗格內顯示多個階段作業。您有多個階段作業連至 iSCSI 儲存空間。

   如果您的主機有多個介面，而您想要將它們連接至 ISCSI 儲存空間，您可以在「起始器 IP」欄位中設定與另一片 NIC 之 IP 位址的另一個連線。不過，在試圖建立連線之前，請務必在 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 中授權第二個起始器 IP 位址。
   {:note}
9. 在「內容」視窗中，按一下**裝置**，以開啟「裝置」視窗。裝置介面名稱的開頭為 `mpio`。<br/>
  ![裝置](/images/Devices.png)

10. 按一下 **MPIO**，以開啟**裝置詳細資料**視窗。您可以在此視窗中選擇 MPIO 的負載平衡原則，並會顯示 iSCSI 的路徑。在此範例中，會顯示兩個可供 MPIO 使用的路徑，以及「以子網路循環配置資源」負載平衡原則。
  ![「裝置詳細資料」視窗顯示兩個可供 MPIO 使用的路徑，以及「以子網路循環配置資源」負載平衡原則](/images/DeviceDetails.png)

11. 按一下**確定**數次，以結束「iSCSI 起始器」。



## 驗證是否在 Windows 作業系統中正確地設置 MPIO

若要驗證是否已配置 Windows MPIO，您必須先確定已啟用「MPIO 附加程式」，並重新啟動伺服器。

![Roles_Features_0](/images/Roles_Features_0.png)

重新啟動完成並新增「儲存裝置」後，可以驗證 MPIO 是否已配置且正在運作中。若要這樣做，請查看**目標裝置詳細資料**，然後按一下 **MPIO**：
![DeviceDetails_0](/images/DeviceDetails_0.png)

如果未正確配置 MPIO，則在發生網路中斷或「{{site.data.keyword.BluSoftlayer_full}} 團隊」執行維護時，儲存裝置可能會中斷連線並且看似已停用。MPIO 可確保在那些事件發生期間，有多一層連線功能，並保持已建立的階段作業對 LUN 持續進行讀取/寫入作業。

## 卸載 {{site.data.keyword.blockstorageshort}} 磁區

以下是將 Windows 型 {{site.data.keyword.Bluemix_short}} 運算實例與 MPIO iSCSI LUN 中斷連線所需的步驟。此範例以 Windows Server 2012 為基礎。您可以根據 OS 供應商文件來調整其他 Windows 版本的步驟。

### 啟動 iSCSI 起始器

1. 按一下**目標**標籤。
2. 選取您要移除的目標，然後按一下**中斷連線**。

### 移除目標
當您不再需要存取 iSCSI 目標時，此為選用步驟。

1. 按一下「iSCSI 起始器」中的**探索**。
2. 強調顯示與儲存空間磁區相關聯的目標入口網站，然後按一下**移除**。
