---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 透過 SL CLI 訂購 {{site.data.keyword.blockstorageshort}}

您可以使用 SL CLI 來訂購通常是透過 [{{site.data.keyword.slportal}} ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://control.softlayer.com/){:new_window} 來訂購的產品。在 SL API 中，一張訂單可能是由多重訂單容器所組成。訂單 CLI 只能用於一個訂單容器。

若要進一步瞭解如何安裝及使用 SL CLI，請參閱 [Python API 用戶端 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}。
{:tip}

## 搜尋可用的 {{site.data.keyword.blockstorageshort}} 供應項目

當您下訂單時，所要尋找的第一個元件是套件。套件分散在 {{site.data.keyword.BluSoftlayer_full}} 中可供訂購的不同頂層產品之間。套件的部分範例包括適用於 VSI 的 CLOUD_SERVER、適用於裸機伺服器的 BARE_METAL_SERVER，以及適用於 {{site.data.keyword.blockstorageshort}} 和 {{site.data.keyword.filestorage_short}} 的 STORAGE_AS_A_SERVICE_STAAS。

在套件中，有些項目會再細分為種類。有些套件有預設項目以方便您使用，有些套件則需要個別指定項目。如果需要套件的種類，則必須從該種類中選擇項目來訂購套件。種類中的某些項目可能會互斥，視種類而定。

每個訂單都必須要有相關聯的位置（資料中心）。當您訂購 {{site.data.keyword.blockstorageshort}} 時，請確定其佈建在與運算實例相同的位置。
{:important}

您可以使用 `slcli order package-list` 指令來尋找您想要訂購的套件。有提供 `–keyword` 選項來執行簡單的搜尋和過濾。此選項可讓您更容易找到所需的套件。

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

*需要如何尋找 Storage-as-a-Service Package 759 的指示*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         name        :       keyName       :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               name               :    categoryCode   : isRequired :
:..................................:...................:............:
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:..................................:...................:............:
```

使用 `item-list` 指令來選取訂單的其餘項目。套件通常會有許多項目可供選擇，您可以使用 `–category` 選項，只從您感興趣的種類中擷取項目。

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         keyName          :                description                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

如需透過 API 來訂購 {{site.data.keyword.blockstorageshort}} 的相關資訊，請參閱 [order_block_volume ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}。
若要能夠存取所有新增特性，請訂購 `Storage-as-a-Service Package 759`。
{:tip}

## 驗證訂單

如果您不確定訂單中是否遺漏了必要的種類，您可以使用 `place` 指令搭配 `–verify` 旗標。若有遺漏任何種類，就會列印在畫面上。


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                keyName                       :                   description                   : cost :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

輸出會顯示所訂購的每個項目，以及該項目的相關成本。如果訂單通過驗證，則表示沒有衝突的項目，且所有必要種類都有訂單中所指定的項目。

## 下訂單

下一步是下訂單。

```
$ slcli order place .....

This action will incur charges on your account. Continue? [y/N]: y

API response
```

依預設，您可以佈建總計 250 個 {{site.data.keyword.blockstorageshort}} 磁區。若要增加磁區數目，請與業務代表聯絡。如需增加限制的相關資訊，請參閱[管理儲存空間限制](managing-storage-limits.html)。
{:important}

## 授權主機存取新的儲存空間

TBD

若要進一步瞭解如何授權主機透過 API 來存取 {{site.data.keyword.blockstorageshort}}，請參閱 [authorize_host_to_volume ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}。
{:tip}

如需同時授權的限制，請參閱[常見問題](faqs.html)。
{:important}

## 連接新的儲存空間

根據主機的作業系統而定，遵循適當的鏈結。
- [在 Linux 上連接至 MPIO iSCSI LUN](accessing_block_storage_linux.html)
- [在 CloudLinux 上連接至 MPIO iSCSI LUN](configure-iscsi-cloudlinux.html)
- [在 Microsoft Windows 上連接至 MPIO iSCSI LUN](accessing-block-storage-windows.html)
- [配置 Block Storage 以便使用 cPanel 進行備份](configure-backup-cpanel.html)
- [配置 Block Storage 以便使用 Plesk 進行備份](configure-backup-plesk.html)
