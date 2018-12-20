---

copyright:
  years: 2015, 2018
lastupdated: "2018-12-10"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# 疑難排解 - Windows 2012 R2 iSCSI 修正程式可看到兩個以上的裝置

如果您在「磁碟管理程式」中只看到兩個裝置，則需要手動連接至每部伺服器上的「iSCSI 起始器」中的每個裝置。如果您使用兩個以上的 iSCSI 裝置，可能會發現此程序很實用；尤其是如果所有 4 個 iSCSI 分配項目都是來自相同的 NetApp 裝置時。

1. 開啟「Windows iSCSI 起始器」。
2. 按一下**目標**標籤，然後按一下**裝置**。

   ![iSCSI 起始器內容](/images/win12-ts1.png)
3. 確認所顯示的裝置數目。如果您看到 2 個裝置，而不是已授權的 4 個裝置，請繼續進行下一步。
4. 依序按一下**目標**、**連接**。
5. 依序選取**多路徑**、**進階**。
6. 選取「Microsoft iSCSI 起始器」作為「本端配接卡」。「起始器 IP」屬於您的伺服器。
7. 選取「目標入口網站 IP」清單中顯示的第一個 IP 位址。

   ![進階設定，IP 位址](/images/win12-ts3.png)

   您必須針對所有列出的 IP 位址重複此步驟。
{:tip}

8. 選取**啟用 CHAP** 方框，並輸入伺服器的 CHAP ID 和密碼。

   ![進階設定，CHAP](/images/win12-ts4.png)
9. 按一下**確定**。
10. 針對您在「iSCSI 起始器」中輸入的每個 IP，重複 5 到 9。完成後，按一下**裝置**標籤，並檢閱結果。預期會看到您設定的每個 LUN 列出兩次。

    ![「裝置」標籤](/images/win12-ts5.png)
11. 按一下**確定**。
12. 開啟「磁碟管理程式」，驗證您的磁碟機現在已顯示。

    ![裝置管理程式](/images/win12-ts6.png)
