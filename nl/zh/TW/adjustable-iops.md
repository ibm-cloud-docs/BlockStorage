---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 調整 IOPS

使用這個新增特性，{{site.data.keyword.blockstoragefull}} 儲存空間使用者就可以立即調整其現有 {{site.data.keyword.blockstorageshort}} 的 IOPS，而不需要建立重複項目，或將資料手動移轉至新的儲存空間。進行調整時，使用者不會遇到任何類型的中斷，也不會無法存取儲存空間。 

將更新儲存空間的計費，以將新價格的按比例差額新增至現行計費週期。在下一個計費週期會計算完整的新金額。

只有[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內才提供此特性。 

## 為何要運用可調式 IOPS？

- 成本管理 - 部分客戶可能只有在用量尖峰時間才需要高 IOPS。例如，大型零售商店在假日期間會有用量尖峰，因此其儲存空間可能在當時比起盛夏而言會需要較高的 IOPS。此特性可讓它們管理其成本，並且只有在真正需要時才支付較高的 IOPS 費用。

## 是否有任何限制？

此特性只適用於佈建在具有加強型功能之[資料中心](new-ibm-block-and-file-storage-location-and-features.html)內的儲存空間。 

用戶端在調整其 IOPS 時無法切換「耐久性」與「效能」。使用者可以根據下列準則/限制來指定儲存空間的新 IOPS 層級或 IOPS 層次： 

- 如果原始磁區是「耐久性 0.25」層級，則無法更新 IOPS 層級。
- 如果原始磁區是具有 < 0.30 IOPS/GB 的「效能」磁區，則可用的選項應該只包含結果會得到 < 0.30 IOPS/GB 的大小及 IOPS 組合。 
- 如果原始磁區是具有 >= 0.30 IOPS/GB 的「效能」磁區，則可用的選項應該只包含結果會得到 >= 0.30 IOPS/GB 的大小及 IOPS 組合。大小（大於或等於原始磁區）



## 調整 IOPS 對抄寫的影響為何？

如果磁區已有抄寫，則會自動更新抄本，以符合主要磁區的 IOPS 選項。 

## 如何調整儲存空間上的 IOPS？

1. 從 {{site.data.keyword.slportal}} 中，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 型錄中，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.blockstorageshort}}**。
2. 從清單中選取 LUN，然後按一下**動作** > **修改 LUN**。
3. 在**儲存空間 IOPS 選項**下，進行新的選擇：
    - 耐久性（分層 IOPS）：為儲存空間選取大於 0.25 IOPS/GB 的「IOPS 層級」。您隨時可以增加 IOPS 層級。不過，一個月只能減少一次。
    - 效能（已配置的 IOPS）：針對您的儲存空間指定新的 IOPS 選項，方法是輸入 100-48,000 IOPS 之間的值。（請務必在訂單表格中查看大小所需的任何特定界限）。
4. 檢閱您的選擇及新的定價。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。
6. 在幾分鐘之後，應該就可以使用您的新儲存空間配置。
