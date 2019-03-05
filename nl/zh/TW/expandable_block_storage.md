---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 擴充 Block Storage 容量
{: #expandingcapacity}

使用這個新增特性，現行 {{site.data.keyword.blockstoragefull}} 使用者可以立即擴充其現有 {{site.data.keyword.blockstorageshort}} 大小（以 GB 為增量單位，最多可到 12 TB）。他們不需要建立重複項目，或者手動將資料移轉至較大的磁區。調整大小時，不會發生中斷，也不會無法存取儲存空間。

會自動更新磁區的計費，以將新價格的按比例差額新增至現行計費週期。然後在下一個計費週期計算完整的新金額。

[精選資料中心](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)內提供此特性。

## 可擴充儲存空間的優點

- **成本管理** - 您知道資料可能會成長，但一開始只需要較小的儲存空間量。擴充能力可讓客戶節省儲存空間成本，然後不斷成長以因應需要。  

- **成長中的儲存空間需求** - 快速資料成長的客戶需要一種可快速且輕鬆地增加其儲存空間大小的方式，以管理該成長。

## 擴充儲存空間容量對於抄寫的效果

主要儲存空間上的擴充動作導致自動調整抄本的大小。

## 限制
{: #limitsofexpandingstorage}

此特性適用於[精選資料中心](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)內所佈建的儲存空間。

在此特性發行之前（**2017 年 4 月 - 2017 年 12 月 14 日**期間），這些資料中心內所佈建的儲存空間可以增加至其原始大小的 10 倍，不能再更大。在 **2017 年 12 月 14 日**之後所佈建的儲存空間最多可以增加至 12 TB。

已佈建「耐久性」之 {{site.data.keyword.blockstorageshort}} 的現有大小限制仍然適用（10 IOPS 層級最多為 4 TB，而所有其他層級最多為 12 TB）。

## 調整儲存空間大小
{: #resizingsteps}

1. 從 {{site.data.keyword.slportal}}，按一下**儲存空間** > **{{site.data.keyword.blockstorageshort}}**，或者，從 {{site.data.keyword.BluSoftlayer_full}} 主控台，按一下**基礎架構** > **儲存空間** > **{{site.data.keyword.blockstorageshort}}**。
2. 從清單中選取 LUN，然後按一下**動作** > **修改 LUN**。
3. 輸入新的儲存空間大小（以 GB 為單位）。
4. 檢閱您的選取項目及新的定價。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。
6. 在幾分鐘之後，就可以使用您的新儲存空間配置。

或者，您可以透過 SLCLI 來調整磁區大小。

```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
{:codeblock}

如需在磁區上展開檔案系統（及任何分割區）以使用新空間的相關資訊，請參閱 OS 文件。
{:tip}
