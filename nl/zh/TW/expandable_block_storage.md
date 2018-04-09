---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 可擴充的 Block Storage 容量

使用這個新增特性，現行 {{site.data.keyword.blockstoragefull}} 使用者可以即時擴充其現有 {{site.data.keyword.blockstorageshort}} 大小（以 GB 為增量單位，最多可到 12 TB），而不需要建立重複項目，或將資料手動移轉至較大的磁區。調整大小時，不會發生中斷，也不會無法存取儲存空間。 

會自動更新磁區的計費，以將新價格的按比例差額新增至現行計費週期，然後在下一個計費週期計算完整的新金額。

只有[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內才提供此特性。 

## 為何要運用可擴充的儲存空間？

- **成本管理** - 您知道資料可能會成長，但一開始只需要較小的儲存空間量。擴充能力可讓客戶節省儲存空間成本，然後不斷成長以因應需要。  

- **成長中的儲存空間需求** - 快速成長的客戶需要一種可快速且輕鬆地增加其儲存空間大小的方式，以管理該成長。

## 擴充儲存空間容量對抄寫的影響為何？

主要儲存空間上的擴充動作將造成抄本自動調整大小。 

## 是否有任何限制？

此特性只適用於佈建在具有加強功能之[資料中心](new-ibm-block-and-file-storage-location-and-features.html)內的儲存空間。 

在此特性發行（2017 年 12 月 14 日）之前，在這些資料中心的已更新儲存空間上佈建的儲存空間，只能增加至其原始大小的 10 倍。在該日期之後佈建的所有其他儲存空間，最多可以增加至 12 TB 大小。 

已佈建「耐久性」之 {{site.data.keyword.blockstorageshort}} 的現有大小限制仍然適用（10 IOPS 層級最多為 4 TB，而所有其他層級最多為 12 TB）。

## 如何分辨是否可擴充已佈建的儲存空間？

已佈建加強型功能的儲存空間一律採取靜態加密。如果入口網站使用者介面中儲存空間旁具有「鎖定」圖示，則可以輕鬆地分辨出儲存空間符合資格。 

## 如何調整儲存空間大小？

1. 從 {{site.data.keyword.slportal}} 中，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或者，從「{{site.data.keyword.BluSoftlayer_full}} 型錄」中，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.blockstorageshort}}**。
2. 從清單中選取 LUN，然後按一下**動作** > **修改 LUN**。
3. 輸入新的儲存空間大小（以 GB 為單位）。
4. 檢閱您的選擇及新的定價。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。
6. 在幾分鐘之後，應該就可以使用您的新儲存空間配置。
  
