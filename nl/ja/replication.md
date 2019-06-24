---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# データのレプリケーション
{: #replication}

レプリケーションでは、いずれかのスナップショット・スケジュールを使用して、スナップショットが自動的にリモート・データ・センター内の宛先ボリュームにコピーされます。 壊滅的なイベントが発生した場合やデータが破損した場合、コピーをリモート・サイトでリカバリーすることができます。

レプリケーションによって、データは 2 つの異なる場所で同期されます。 使用しているボリュームを複製したものを元のボリュームとは独立して使用する場合は、[複製ブロック・ボリュームの作成](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)を参照してください。
{:tip}

レプリケーションを行うには、事前に、まずスナップショット・スケジュールを作成しておく必要があります。
{:important}


## 複製ストレージ・ボリュームのリモート・データ・センターの判別

{{site.data.keyword.cloud}} のデータ・センターは、世界的にプライマリーとリモートを組み合わせたペアになっています。
使用可能なデータ・センターとレプリケーション・ターゲットの完全なリストについては、表 1 を参照してください。

| 米国 1 | 米国 2 | ラテンアメリカ | カナダ  | ヨーロッパ  | アジア太平洋  | オーストラリア  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="表 1 - この表は、各地域の拡張機能を備えたデータ・センターの完全なリストを示しています。 各地域が別々の列に示されています。 一部の都市 (ダラス、サンノゼ、ワシントン DC、アムステルダム、フランクフルト、ロンドン、シドニーなど) には複数のデータ・センターがあります。 米国 1 地域のデータ・センターには、拡張ストレージがありません。 拡張ストレージ機能を備えたデータ・センター内のホストは、米国 1 のデータ・センター内にレプリカ・ターゲットが指定されたレプリケーションを開始できません。" caption-side="top"}

## 初期レプリカの作成

レプリケーションは、スナップショット・スケジュールに基づいて作動します。 レプリケーションを行うには、まずソース・ボリューム用のスナップショット・スペースとスナップショット・スケジュールを作成する必要があります。 レプリケーションを設定しようとしたときに、どちらかの準備ができていなければ、より多くのスペースを購入するか、スケジュールを設定するように求められます。 レプリケーションは、[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}の**「ストレージ」**の**「{{site.data.keyword.blockstorageshort}}」**で管理されます。

1. ストレージ・ボリュームをクリックします。
2. **「レプリカ」**をクリックし、**「レプリケーションの購入 (Purchase a replication)」**をクリックします。
3. レプリケーションに使用する既存のスナップショット・スケジュールを選択します。 リストに、アクティブなすべてのスナップショット・スケジュールが示されます。 <br />
   時間単位、日単位、および週単位が混在する場合でも、選択できるスケジュールは 1 つだけです。 前回のレプリケーション・サイクル以降に収集されたすべてのスナップショットが、それらを生成したスケジュールに関係なく複製されます。<br />スナップショットのセットアップが済んでいない場合は、レプリケーションを注文する前にセットアップを行うよう、プロンプトが表示されます。 詳しくは、[スナップショットの処理](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots)を参照してください。
   {:important}
3. **「ロケーション」**をクリックして、DR サイトであるデータ・センターを選択します。
4. **「続行」**をクリックします。
5. **プロモーション・コード**を入力して (ある場合)、**「再計算」**をクリックします。 ウィンドウのその他のフィールドは、デフォルトで入力されています。

   注文の処理時に割引が適用されます。
   {:note}
6. **「マスター・サービス契約を読み... (I have read the Master Service Agreement…)」**チェック・ボックスをクリックして、**「注文する (Place Order)」**をクリックします。


## 既存のレプリケーションの編集

[{{site.data.keyword.cloud}} コンソール](https://{DomainName}/classic){: external}で、**「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**の下にある**「プライマリー」**または**「レプリカ」**のいずれかのタブから、レプリケーション・スケジュールを編集したり、レプリケーション・スペースを変更したりできます。


## レプリケーション・スケジュールの編集

レプリケーション・スケジュールは、既存のスナップショット・スケジュールに基づいています。 レプリケーション・スケジュールを「毎時」から「毎日」や「毎週」に、またはその逆に変更するには、レプリカ・ボリュームをキャンセルして新しいものをセットアップする必要があります。

ただし、**「毎日」**のレプリケーションが行われる時刻を変更する場合には、「プライマリー」タブまたは「レプリカ」タブで既存のスケジュールを調整することができます。

1. **「プライマリー (Primary)」**または**「レプリカ」**のいずれかのタブで、**「アクション」**をクリックします。
2. **「スナップショット・スケジュールの編集」**を選択します。
3. **「スケジュール**」の下の**「スナップショット」**フレームで、レプリケーションに使用しているスケジュールを判別します。 必要に応じてスケジュールを変更します。
4. **「保存」**をクリックします。


## レプリケーション・スペースの変更

プライマリー・スナップショット・スペースとレプリカ・スペースは同じものであることが必要です。 **「プライマリー (Primary)」**タブまたは**「レプリカ」**タブでスペースを変更すると、ソースと宛先の両方のデータ・センターに自動的にスペースが追加されます。 スナップショット・スペースを増やすと、レプリケーションの即時更新もトリガーされます。

1. **「プライマリー (Primary)」**または**「レプリカ」**のいずれかのタブで、**「アクション」**をクリックします。
2. **「スナップショット・スペースの追加」**を選択します。
3. リストからストレージ・サイズを選択して、**「続行」**をクリックします。
4. **プロモーション・コード**を入力して (ある場合)、**「再計算」**をクリックします。 ダイアログ・ボックスのその他のフィールドは、デフォルトで入力されています。
5. **「マスター・サービス契約を読み... (I have read the Master Service Agreement…)」**チェック・ボックスをクリックして、**「注文する (Place Order)」**をクリックします。


## ボリューム・リスト内のレプリカ・ボリュームの表示

**「ストレージ」>「{{site.data.keyword.blockstorageshort}}」**の下の「{{site.data.keyword.blockstorageshort}}」ページでレプリケーション・ボリュームを表示できます。 **「LUN 名」**には、プライマリー・ボリュームの名前とそれに続いて REP が表示されます。 **「タイプ」**は、「エンデュランス - レプリカ」または「パフォーマンス - レプリカ」です。 **「ターゲット・アドレス」**は、「N/A」であり (レプリカ・ボリュームはレプリカ・データ・センターでマウントされていないため)、**「状況」**には「非アクティブ」と表示されます。


## レプリカ・データ・センターでの複製ボリュームの詳細の表示

**「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**の**「レプリカ」**タブで、レプリカ・ボリュームの詳細を表示できます。 もう 1 つのオプションとしては、**「{{site.data.keyword.blockstorageshort}}」**ページからレプリカ・ボリュームを選択して、**「レプリカ」**タブをクリックします。


## プライマリー・データ・センターでスナップショット・スペースが増加したときに、レプリカ・データ・センターのスナップショット・スペースを増やす

プライマリーとレプリカのストレージ・ボリュームで、ボリューム・サイズが同じであることが必要です。 一方をもう一方より大きくすることはできません。 プライマリー・ボリュームのスナップショット・スペースを増やすと、レプリカ・スペースも自動的に増えます。 スナップショット・スペースを増やすと、レプリケーションの即時更新がトリガーされます。 両方のボリュームの増加は、請求書に行項目として表示され、必要に応じて日割り計算されます。

スナップショット・スペースの増加について詳しくは、[スナップショットの注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)を参照してください。
{:tip}


## レプリケーション履歴の表示

レプリケーション履歴は、**「管理」**の下の**「アカウント」**タブで、**「監査ログ」**に表示されます。 プライマリーとレプリカの両方のボリュームに、同一のレプリケーション履歴が表示されます。 履歴には以下の項目が含まれています。

- レプリケーションのタイプ (フェイルオーバーまたはフェイルバック)。
- 開始日時。
- レプリケーションに使用されたスナップショット。
- レプリケーションのサイズ。
- レプリケーションが完了した時刻。


## レプリカの複製の作成

既存の {{site.data.keyword.cloud}} {{site.data.keyword.blockstoragefull}} の複製を作成できます。 複製ボリュームは元のボリュームの容量とパフォーマンスのオプションをデフォルトで継承し、スナップショットの時点までのデータの複製を保管します。

複製はプライマリー・ボリュームからでもレプリカ・ボリュームからでも作成できます。 新規の複製は元のボリュームと同じデータ・センターに作成されます。 レプリカ・ボリュームから複製を作成すると、レプリカ・ボリュームと同じデータ・センターに新規ボリュームが作成されます。

ストレージがプロビジョンされるとすぐに、ホストは複製ボリュームにアクセスして、読み取り/書き込みを行うことができます。 ただし、元のボリュームから複製へのデータ・コピーが完了するまで、スナップショットおよびレプリケーションは許可されません。

詳しくは、[複製ブロック・ボリュームの作成](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)を参照してください。

## 災害発生時にフェイルオーバーするためのレプリカの使用

フェイルオーバーすると、プライマリー・データ・センター内のストレージ・ボリュームからリモート・データ・センター内の宛先ボリュームへの「反転切り替え」が行われます。 例えば、プライマリー・データ・センターがロンドンにあり、2 次データ・センターがアムステルダムにあるとします。 障害が発生した場合は、アムステルダムにフェイルオーバーします。つまり、アムステルダムにあるコンピューティング・インスタンスの新しいプライマリー・ボリュームに接続することになります。 ロンドンのボリュームが修復された後は、ロンドンにフェイルバックするためにアムステルダムのボリュームのスナップショットが作成され、ロンドンのコンピューティング・インスタンスに含まれる再びプライマリーになったボリュームに接続します。

* 1 次ロケーションに危険が差し迫っているか、大きな影響を受けた場合は、[アクセス可能 1 次ボリュームを使用したフェイルオーバー](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible)を参照してください。
* 1 次ロケーションがダウンしている場合は、[アクセス不能 1 次ボリュームを使用したフェイルオーバー](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible)を参照してください。


## 既存のレプリケーションのキャンセル

レプリケーションは、即時にキャンセルすることも、支払い日にキャンセルすることもでき、そこで請求が終了します。 レプリケーションのキャンセルは、**「プライマリー (Primary)」**と**「レプリカ」**のどちらのタブからでも行うことができます。

1. **「{{site.data.keyword.blockstorageshort}}」**ページでボリュームをクリックします。
2. **「プライマリー (Primary)」**または**「レプリカ」**のいずれかのタブで、**「アクション」**をクリックします。
3. **「レプリカのキャンセル」**を選択します。
4. キャンセルするタイミングを選択します。 **「即時」**または**「請求料金の確定日」**を選択し、**「続行」**をクリックします。
5. **「私は、キャンセルによりデータ損失が生じる可能性があることを了解します」**をクリックして、**「レプリカのキャンセル」**をクリックします。


## プライマリー・ボリュームがキャンセルされたときのレプリケーションのキャンセル

プライマリー・ボリュームがキャンセルされると、レプリケーション・スケジュールとレプリカ・データ・センター内のボリュームが削除されます。 レプリカのキャンセルは、「{{site.data.keyword.blockstorageshort}}」ページから行います。

 1. **「{{site.data.keyword.blockstorageshort}}」**ページで、ボリュームを強調表示します。
 2. **「アクション」**をクリックして、**「 {{site.data.keyword.blockstorageshort}}のキャンセル」**を選択します。
 3. キャンセルするタイミングを選択します。 **「即時」**または**「請求料金の確定日」**を選択し、**「続行」**をクリックします。
 4. **「私は、キャンセルによりデータ損失が生じる可能性があることを了解します」**をクリックして、**「キャンセル」**をクリックします。

 LUN は、少なくとも 24 時間 (即時キャンセルの場合)、または支払い日まで、ストレージ・リストにそのまま表示されます。 特定の機能が使用できなくなりますが、ボリュームは再利用処理が施されるまで引き続き表示されます。 ただし、「削除」/「キャンセル」をクリックした直後に課金は停止されます。

 アクティブなレプリカがあると、ストレージ・ボリュームの再利用処理がブロックされます。 ボリュームがマウントされていないこと、ホストの許可が取り消されていること、レプリケーションがキャンセルされていることを確認した後で、元のボリュームのキャンセルを試みてください。


## SLCLI のレプリケーション関連コマンド
{: #clicommands}

* 指定されたボリュームに適したレプリケーション・データ・センターをリストします。
  ```
  # slcli block replica-locations --help
  Usage: slcli block replica-locations [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Long Name, Short Name
  -h, --help      Show this message and exit.
  ```

* Block Storage レプリカ・ボリュームを注文します。
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

* ブロック・ボリュームの既存レプリカ・ボリュームをリストします。
  ```
  # slcli block replica-partners --help
  Usage: slcli block replica-partners [OPTIONS] VOLUME_ID

  Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Show this message and exit.
  ```

* ブロック・ボリュームを特定のレプリカ・ボリュームにフェイルオーバーします。
  ```
  # slcli block replica-failover --help
  Usage: slcli block replica-failover [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  --immediate          Failover to replicant immediately.
  -h, --help           Show this message and exit.
  ```

* ブロック・ボリュームを特定のレプリカ・ボリュームからフェイルオーバーします。
  ```
  # slcli block replica-failback --help
  Usage: slcli block replica-failback [OPTIONS] VOLUME_ID

  Options:
  --replicant-id TEXT  ID of the replicant volume
  -h, --help           Show this message and exit.
  ```
