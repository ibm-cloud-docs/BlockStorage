---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-11"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 抄寫資料
{: #replication}

抄寫使用您的其中一個 Snapshot 排程，自動將 Snapshot 複製到遠端資料中心內的目的地磁區。如果發生災難性事件，或者您的資料毀損，則可以在遠端網站中回復副本。

抄寫是將資料同步保留在兩個不同位置。如果您想要複製磁區，並與原始磁區分開使用，請參閱[建立重複的區塊磁區](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)。
{:tip}

您必須先建立 Snapshot 排程，才能進行抄寫。{:important}


## 判斷我的已抄寫儲存空間磁區的遠端資料中心

{{site.data.keyword.cloud}} 的資料中心已配對成全球主要與遠端組合。如需完整的資料中心可用性及抄寫目標清單，請參閱表 1。

|美國 1 |美國 2|拉丁美洲|加拿大|歐洲|亞太地區|澳洲|
|-----|-----|-----|-----|-----|-----|-----|
|DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01|SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13|MEX01<br />SAO01|TOR01<br />MON01|AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01|HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />                                CHE01|SYD01<br />SYD04<br />          SYD05<br />MEL01|
{: caption="表 1 - 此表格顯示每一個地區中具有加強功能的完整資料中心清單。每個地區都是個別的直欄。有些城市（例如「達拉斯」、「聖荷西」、「華盛頓特區」、「阿姆斯特丹」、「法蘭克福」、「倫敦」及「雪梨」）會有多個資料中心。美國 1 地區中的資料中心沒有加強儲存空間。具有加強儲存空間功能之資料中心內的主機無法開始抄本目標位於美國 1 資料中心的抄寫。" caption-side="top"}

## 建立起始抄本

抄寫是根據 Snapshot 排程運作。您必須先有來源磁區的 Snapshot 空間及 Snapshot 排程，然後才能進行抄寫。如果您嘗試設定抄寫，但其中一者還未就緒，則會提示您購買更多空間，或是設定排程。抄寫是在 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 的**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 下管理。

1. 按一下儲存空間磁區。
2. 按一下**抄本**，然後按一下**購買抄寫**。

3. 選取您要抄寫遵循的現有 Snapshot 排程。此清單包含所有作用中 Snapshot 排程。<br />
   您只能選取一個排程，即使是混合使用每小時、每日及每週。將會抄寫自前次抄寫週期以來擷取到的所有 Snapshot，不論其原始的排程為何。<br />如果您未設定 Snapshot，則系統會先提示您這樣做，才能訂購抄寫。如需詳細資料，請參閱[使用 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots)。
   {:important}
3. 按一下**位置**，然後選取作為您 DR 網站的資料中心。
4. 按一下**繼續**。
5. 如果您有**促銷代碼**，請輸入促銷代碼，然後按一下**重新計算**。依預設，會完成視窗中的其他欄位。

   折扣會在處理訂單時套用。
   {:note}
6. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。


## 編輯現有抄寫

您可以從 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 之**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 下的**主要**或**抄本**標籤中編輯抄寫排程，以及變更抄寫空間。


## 編輯抄寫排程

您的抄寫排程是以現有的 Snapshot 排程為基礎。若要將抄本排程從「每小時」變更為「每日」或「每週」（反之亦然），您必須取消抄本磁區並設定新的磁區。

不過，如果您想要在**每日**抄寫發生時變更當日時間，可以在「主要」或「抄本」標籤上調整現有的排程。

1. 在**主要**或**抄本**標籤上，按一下**動作**。
2. 選取**編輯 Snapshot 排程**。
3. 查看**排程**下的 **Snapshot** 頁框，以判定您要用於抄寫的排程。變更所需的排程。
4. 按一下**儲存**。


## 變更抄寫空間

您的主要 Snapshot 空間與抄本空間必須相同。如果您在**主要**或**抄本**標籤上變更空間，則會自動將空間新增至來源及目的地資料中心。增加 Snapshot 空間也會觸發立即抄寫更新。

1. 在**主要**或**抄本**標籤上，按一下**動作**。
2. 選取**新增其他 Snapshot 空間**。
3. 從清單中選取儲存空間大小，然後按一下**繼續**。
4. 如果您有**促銷代碼**，請輸入促銷代碼，然後按一下**重新計算**。依預設，會完成對話框中的其他欄位。
5. 按一下**我已閱讀主要服務合約...** 勾選框，然後按一下**下訂單**。


## 在磁區清單中檢視抄本磁區

您可以在**儲存空間 > {{site.data.keyword.blockstorageshort}}** 的 {{site.data.keyword.blockstorageshort}} 頁面上檢視抄寫磁區。**LUN 名稱**顯示主要磁區的名稱，其後接著 REP。**類型**為「耐久性或效能 - 抄本」。**目標位址**為 N/A，因為抄本磁區未裝載在抄本資料中心，且**狀態**顯示「非作用中」。


## 在抄本資料中心檢視已抄寫磁區的詳細資料

您可以在**儲存空間**、**{{site.data.keyword.blockstorageshort}}** 下的**抄本**標籤上檢視抄本磁區詳細資料。另一個選項是從 **{{site.data.keyword.blockstorageshort}}** 頁面選取抄本磁區，然後按一下**抄本**標籤。


## 在主要資料中心增加 Snapshot 空間時，在抄本資料中心中增加 Snapshot 空間

主要及抄本儲存空間磁區的磁區大小必須相同。其中一個不能大於另一個。當您增加主要磁區的 Snapshot 空間時，即會自動增加抄本空間。增加 Snapshot 空間會觸發立即抄寫更新。兩個磁區的增加量會在您的發票中顯示為行項目，並且視需要按比例分配。

如需增加 Snapshot 空間的相關資訊，請參閱[訂購 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)。
{:tip}


## 檢視抄寫歷程

可以在**管理**下的**帳戶**標籤上，於**審核日誌**中檢視抄寫歷程。主要及抄本磁區都會顯示相同的抄寫歷程。歷程中包含下列項目。

- 抄寫的類型（失效接手或失效回復）。
- 開始的時間。
- 用於抄寫的 Snapshot。
- 抄寫的大小。
- 完成抄寫的時間。


## 建立抄本的重複項目

您可以建立現有 {{site.data.keyword.cloud}} {{site.data.keyword.blockstoragefull}} 的重複項目。依預設，重複磁區會繼承原始磁區的容量及效能選項，而且會有到達 Snapshot 中該時間點之前的資料副本。

您可以從主要及抄本磁區建立重複磁區。新的重複磁區會建立在與原始磁區相同的資料中心內。如果您建立抄本磁區的重複磁區，則新的磁區會建立在與抄本磁區相同的資料中心內。

佈建儲存空間之後，主機就可以存取重複磁區來進行讀寫。不過，除非從原始磁區到重複磁區的資料複製已完成，否則不容許進行 Snapshot 及抄寫。

如需相關資訊，請參閱[建立重複的區塊磁區](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)。

## 災難來襲時，使用抄本進行失效接手

當您失效接手時，會將「開關」從主要資料中心的儲存空間磁區快速切換到遠端資料中心的目的地磁區。例如，您的主要資料中心是「倫敦」，而次要資料中心是「阿姆斯特丹」。如果發生故障事件，您會失效接手至「阿姆斯特丹」- 從「阿姆斯特丹」的運算實例連接至現行主要磁區。修復「倫敦」中的磁區之後，會建立「阿姆斯特丹」磁區的 Snapshot，以從「倫敦」的運算實例失效回復至「倫敦」及那個再度成為主要磁區的磁區。

* 如果主要位置即將發生危險或嚴重受到影響，請參閱[使用可存取的主要磁區進行失效接手](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible)。
* 如果主要位置已停機，請參閱[使用無法存取的主要磁區進行失效接手](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible)。


## 取消現有抄寫

您可以立即或在週年日取消抄寫，這會導致計費結束。您可以從**主要**或**抄本**標籤中取消抄寫。

1. 在 **{{site.data.keyword.blockstorageshort}}** 頁面上，按一下磁區。
2. 在**主要**或**抄本**標籤上，按一下**動作**。
3. 選取**取消抄本**。
4. 選取何時取消。選擇**立即**或**週年日**，然後按一下**繼續**。
5. 按一下**我確認因為取消而可能發生資料流失**，然後按一下**取消抄本**。


## 取消主要磁區後取消抄寫

取消主要磁區後，即會刪除抄寫排程及抄本資料中心內的磁區。抄本是從 {{site.data.keyword.blockstorageshort}} 頁面進行取消。

 1. 在 **{{site.data.keyword.blockstorageshort}}** 頁面上，強調顯示磁區。
 2. 按一下**動作**，然後選取**取消 {{site.data.keyword.blockstorageshort}}**。
 3. 選取何時取消。選擇**立即**或**週年日**，然後按一下**繼續**。
 4. 按一下**我確認因為取消而可能發生資料流失**，然後按一下**取消**。

## SLCLI 中的抄寫相關指令
{: #clicommands}

* 列出特定磁區適合的抄寫資料中心。
  ```
  # slcli block replica-locations --help
  Usage: slcli block replica-locations [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* 訂購區塊儲存空間抄本磁區。
  ```
  # slcli block replica-order --help
  Usage: slcli block replica-order [OPTIONS] VOLUME_ID

  Options:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshot schedule to use for replication,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [required]
  -l, --location TEXT             Short name of the data center for the
                                  replicant (e.g.: dal09)  [required]
  --tier [0.25|2|4|10]            Endurance Storage Tier (IOPS per GB) of the
                                  primary volume for which a replicant is
                                  ordered [optional]
  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  Operating System Type (e.g.: LINUX) of the
                                  primary volume for which a replica is
                                  ordered [optional]
  -h, --help                      Show this message and exit.
  ```

* 列出區塊磁區的現有抄本磁區。
  ```
  # slcli block replica-partners --help
  Usage: slcli block replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* 將區塊磁區失效接手至特定抄本磁區。
  ```
  # slcli block replica-failover --help
  Usage: slcli block replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help      Show this message and exit.
```

* 從特定抄本磁區中失效回復區塊磁區。
  ```
  # slcli block replica-failback --help
  Usage: slcli block replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
