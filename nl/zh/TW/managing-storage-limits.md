---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-06"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 管理儲存空間限制

依預設，您可以在全球佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。 

您可以在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} 中提交問題單，以要求提高限制。要求經核准之後，會導致針對特定資料中心設定磁區限制。  

若要要求更新或提高限制，請開立問題單，並將它導向給您的業務代表。

在問題單中，請提供下列資訊：

- **問題單主旨**：要求增加資料中心磁區計數儲存空間限制

- **額外磁區要求的使用案例為何？** <br />
*例如，您的答案可能類似於新 VM 資料儲存庫、新開發/測試環境、SQL Database、記載等等。*

- **依類型、大小、IOPS 及位置列出需要多少額外的「區塊」磁區？** <br />
*例如，您的答案可能類似於 "25x Endurance 2TB @ 4 IOPS in DAL09" 或 "25x Performance 4TB @ 2 IOPS in WDC04"。*

- **依類型、大小、IOPS 及位置列出需要多少額外的「檔案」磁區？** <br />
*例如，您的答案可能類似於 "25x Performance 20GB @ 10 IOPS in DAL09" 或 "50x Endurance 2TB @ 0.25 IOPS in SJC03"。*
 
- **提供您預期佈建所有要求磁區增加的預估時間。** <br />
*例如，您的答案可能類似於 "90 days"。*

- **提供這些磁區的預期平均容量使用率的 90 天預測。** <br />
*例如，您的答案可能類似於 "expect 25% utilized in 30 days, 50% utilized in 60 days and 75% utilized in 90 days"。*

需要回答上述所有問題。您的限制若有更新，系統會透過問題單處理程序通知您。 
