---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 保護資料安全 - 提供者管理的靜態加密 (Encryption-At-Rest)

## {{site.data.keyword.blockstorageshort}} 靜態加密

{{site.data.keyword.BluSoftlayer_full}} 很認真看待安全，也瞭解能夠加密資料以確保資料安全的重要性。使用提供者管理的加密，依預設會加密已佈建「耐久性」或「效能」選項的 {{site.data.keyword.blockstoragefull}}，不需額外付費，而且不會影響效能。

提供者管理的靜態加密特性會使用下列業界標準通訊協定：

* 業界標準 AES-256 加密
* 使用業界標準「金鑰管理交互作業通訊協定 (KMIP)」在內部管理金鑰
* 儲存空間已經過「美國聯邦資訊處理標準 (FIPS) 出版品 140-2」、「聯邦資訊安全管理法 (FISMA)」、「醫療保險轉移和責任法 (HIPAA)」驗證。儲存空間已經過驗證，符合「支付卡產業 (PCI)」、Basel II、「加州安全違反資訊行為法案 (SB 1386)」及「歐盟資料保護指令 95/46/EC」規範。

## 提供 Snapshot 或已抄寫儲存空間的靜態加密  

依預設，已加密 {{site.data.keyword.blockstorageshort}} 的所有 Snapshot 及抄本也會加密。無法根據磁區來關閉此特性。

## 佈建具有加密的儲存空間

提供者管理的靜態加密特性適用於[精選資料中心](new-ibm-block-and-file-storage-location-and-features.html)內所佈建的 {{site.data.keyword.blockstorageshort}}。在這些資料中心內訂購的所有儲存空間，佈建時都會自動具有加密。

訂購 {{site.data.keyword.blockstorageshort}} 時，請選取已註記星號 (`*`) 的資料中心。您可以看到「LUN/磁區名稱」欄位的右側有一個鎖定圖示，這表示磁區已加密。

![鎖定圖示表示 LUN 已加密](/images/encryptedstorage.png)
<caption>圖 1. 顯示 LUN 已加密的鎖定圖示範例。</caption>



資料中心升級之前佈建的未加密儲存空間都**不會**自動加密。如果您在已升級的資料中心內擁有未加密的儲存空間，並且想要加密的儲存空間，則需要建立新的磁區並移轉資料。如需相關資訊，請參閱[已升級資料中心內的 {{site.data.keyword.blockstorageshort}} 移轉](migrate-block-storage-encrypted-block-storage.html)。
{:important}
