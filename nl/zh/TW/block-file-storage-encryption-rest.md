---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# 保護資料安全 - 提供者管理的靜態加密 (Encryption-At-Rest)

## {{site.data.keyword.blockstorageshort}} 與 {{site.data.keyword.filestorage_full_notm}} 靜態加密 

{{site.data.keyword.BluSoftlayer_full}} 很認真看待安全，也瞭解能夠加密資料以確保資料安全的重要性。使用提供者管理的加密，依預設會加密已佈建「耐久性」或「效能」的 {{site.data.keyword.blockstoragefull}} 及 {{site.data.keyword.filestorage_full}}，不需額外付費，而且不會影響效能。

提供者管理的靜態加密特性會使用下列業界標準通訊協定：

* 業界標準 AES-256 加密
* 使用業界標準「金鑰管理交互作業通訊協定 (KMIP)」在內部管理金鑰
* 儲存空間已經過「美國聯邦資訊處理標準 (FIPS) 出版品 140-2」驗證，符合下列規範：「聯邦資訊安全管理法 (FISMA)」、「醫療保險轉移和責任法 (HIPAA)」、「支付卡產業 (PCI)」、Basel II、「加州安全違反資訊行為法案 (SB 1386)」及「歐盟資料保護指令 95/46/EC」

## Snapshot 或已抄寫儲存空間的靜態加密  

依預設，已加密 {{site.data.keyword.blockstorageshort}} 的所有 Snapshot 及抄本也會加密。無法根據磁區來關閉此特性。

## 使用加密來佈建儲存空間

提供者管理的靜態加密特性只適用於在定期新增資料中心可用性之精選資料中心內佈建的 {{site.data.keyword.blockstorageshort}}。在這些資料中心內佈建的所有儲存空間，佈建時都會自動具有靜態資料的加密。按一下[這裡](new-ibm-block-and-file-storage-location-and-features.html)，以查看提供靜態資料之 {{site.data.keyword.blockstorageshort}} 加密的現行資料中心清單。

訂購 {{site.data.keyword.blockstorageshort}} 時，請選取已註記 * 並且有指出加密可用之訊息的資料中心。您會在「LUN/磁區名稱」欄位右側看到一個鎖定圖示，這表示已加密。請參閱圖 1。

![鎖定圖示表示 LUN 已加密](/images/encryptedstorage.png)
<caption>圖 1. 指出 LUN 已加密的鎖定圖示範例。</caption>



**附註**：**不**會自動加密在資料中心升級之前佈建的未加密儲存空間。如果已升級的資料中心內有未加密的儲存空間，則需要建立新的 LUN 或磁區，並執行資料移轉。下列文章可以提供指引。

* [已升級資料中心內的 {{site.data.keyword.blockstorageshort}} 移轉](migrate-block-storage-encrypted-block-storage.html)
