---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# 複製ブロック・ボリュームの作成
{: #duplicatevolume}

既存の {{site.data.keyword.blockstoragefull}} の複製を作成できます。 複製ボリュームは元のボリュームの容量とパフォーマンスのオプションをデフォルトで継承し、スナップショットの時点までのデータの複製を保管します。   

複製は特定時点のスナップショットのデータに基づいているため、複製を作成するには、元のボリュームにスナップショット・スペースが必要です。 スナップショットの詳細、およびスナップショット・スペースの注文方法については、[スナップショット](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots)の説明を参照してください。  

複製は、**1 次**ボリュームからでも**レプリカ**・ボリュームからでも作成できます。 新規の複製は元のボリュームと同じデータ・センターに作成されます。 レプリカ・ボリュームから複製を作成すると、レプリカ・ボリュームと同じデータ・センターに新規ボリュームが作成されます。

ストレージがプロビジョンされるとすぐに、ホストは複製ボリュームにアクセスして、読み取り/書き込みを行うことができます。 ただし、元のボリュームから複製へのデータ・コピーが完了するまで、スナップショットおよびレプリケーションは許可されません。

データ・コピーが完了すると、複製は、完全に独立したボリュームとして管理したり使用したりできるようになります。

この機能は、ほとんどのロケーションで使用できます。 詳しくは、[使用可能なデータ・センターのリスト](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC)を参照してください。

{{site.data.keyword.containerlong}} の「専用」アカウント・ユーザーである場合は、[{{site.data.keyword.containerlong_notm}} 資料](/docs/containers?topic=containers-block_storage#block_backup_restore)にある、ボリュームを複製するためのオプションを参照してください。
{:tip}

複製ボリュームの一般的な使用例を以下に示します。
- **災害復旧テスト**: 災害が発生してもレプリカ・ボリュームのデータが完全なままであり、そのデータを使用できることを確認するため、レプリケーションを中断せずに、レプリカ・ボリュームの複製を作成する。
- **ゴールデン・コピー**: ストレージ・ボリュームを、それを元にさまざまな用途のための複数のインスタンスを作成できるゴールデン・コピーとして使用する。
- **データ・リフレッシュ**: 非実稼働環境にマウントする実動データのコピーをテスト用に作成する。
- **スナップショットからのリストア**: スナップショット・リストア機能で、元のボリューム全体を上書きせずに、特定のファイルおよび日付を指定してスナップショットから元のボリュームのデータをリストアする。
- **開発とテスト (dev/test)**: ボリュームの同時複製を一度に 4 つまで作成して、開発およびテスト用の複製データを作成する。
- **ストレージのサイズ変更**: データの移動の必要なく、新しいサイズまたは IOPS レート (あるいはその両方) を指定したボリュームを作成する。  

[{{site.data.keyword.cloud_notm}} コンソール](https://{DomainName}/){: external}を介して、いくつかの方法で重複ボリュームを作成できます。


## ストレージ・リスト内の特定のボリュームから複製を作成する方法

1. {{site.data.keyword.cloud_notm}} コンソールで**「インフラストラクチャー」**>**「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックして、{{site.data.keyword.blockstorageshort}} のリストに移動します。
2. リストからボリュームを選択し、**「アクション」** > **「LUN (ボリューム) の複製 (Duplicate LUN (Volume))」**をクリックします。
3. スナップショット・オプションを選択します。
    - **非レプリカ**・ボリュームから注文する場合は、次のようにします。
      - **「新規スナップショットから作成 (Create from new snapshot)」**を選択します。この操作により、複製に使用するスナップショットが作成されます。 ボリュームに現行のスナップショットがない場合、またはその時点で複製を作成する必要がある場合は、このオプションを使用します。<br/>
      - **「最新のスナップショットから作成 (Create from latest snapshot)」**を選択します。この操作により、このボリュームに対して存在する最新のスナップショットから複製が作成されます。
    - **レプリカ**ボリュームから注文する場合、スナップショットの唯一のオプションは、使用可能な最新のスナップショットを使用することです。
4. ストレージ・タイプとロケーションは、元のボリュームと同じままです。
5. 毎時請求または月次請求 - 複製 LUN をプロビジョンする場合に毎時請求にするか月次請求にするかを選択できます。 元のボリュームの請求タイプが自動的に選択されます。 複製ストレージに別の請求タイプを選択する場合は、ここでその選択を行うことができます。
5. 必要に応じて、新規ボリュームの IOPS または IOPS ティアを指定できます。 元のボリュームの IOPS 指定が、デフォルトで設定されています。 使用可能なパフォーマンスとサイズの組み合わせが表示されます。
    - 元のボリュームが 0.25 IOPS エンデュランス層の場合、新しい選択を行うことはできません。
    - 元のボリュームが 2 IOPR、4 IOPR、または 10 IOPR のエンデュランス層の場合、新規ボリュームではこれらのどのティアにでも移すことができます。
6. 新規ボリュームを元のボリュームよりも大きいサイズに更新できます。 元のボリュームのサイズが、デフォルトで設定されています。

   {{site.data.keyword.blockstorageshort}}は、ボリュームの元のサイズの 10 倍にサイズ変更できます。
   {:tip}
7. 新規ボリュームのスナップショット・スペースを更新して、スナップショット・スペースを追加、縮小、またはスナップショット・スペースなしにすることができます。 元のボリュームのスナップショット・スペースが、デフォルトで設定されています。
8. **「続行」**をクリックして、注文します。

## 特定スナップショットからの複製の作成

1. {{site.data.keyword.blockstorageshort}}のリストに進みます。
2. リストから「LUN」をクリックして、詳細ページを表示します。 (レプリカ・ボリュームまたは非レプリカ・ボリュームのいずれかになります。)
3. 詳細ページでスクロールダウンし、リストから既存のスナップショットを選択して、**「アクション」** > **「複製」**をクリックします。   
4. ストレージ・タイプ (エンデュランスまたはパフォーマンス) およびロケーションは、元のボリュームと同じままです。
5. 使用可能なパフォーマンスとサイズの組み合わせが表示されます。 元のボリュームの IOP 指定が、デフォルトで設定されています。 新規ボリュームの IOPS または IOPS ティアを指定できます。
    - 元のボリュームが 0.25 IOPS エンデュランス層の場合、新しい選択を行うことはできません。
    - 元のボリュームが 2 IOPS、4 IOPS、または 10 IOPS のエンデュランス層の場合、新規ボリュームではこれらのどのティアにでも移すことができます。
6. 新規ボリュームを元のボリュームよりも大きいサイズに更新できます。 元のボリュームのサイズが、デフォルトで設定されています。

   {{site.data.keyword.blockstorageshort}}は、ボリュームの元のサイズの 10 倍にサイズ変更できます。
   {:tip}
7. 新規ボリュームのスナップショット・スペースを更新して、スナップショット・スペースを追加、縮小、またはスナップショット・スペースなしにすることができます。 元のボリュームのスナップショット・スペースが、デフォルトで設定されています。
8. **「続行」**をクリックして、複製を注文します。


## SLCLI を使用した複製の作成
SLCLI の次のコマンドを使用すると、{{site.data.keyword.blockstorageshort}}・ボリュームの複製を作成できます。

```
# slcli block volume-duplicate --help
Usage: slcli block volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate block volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Potential Sizes:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] Minimum: [the size of the
                                  origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin block volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```
{:codeblock}

## 複製ボリュームの管理

元のボリュームから複製ボリュームにデータが複製されている間、複製が進行中であることを示す状況が詳細ページに表示されます。 この間、ホストに接続してボリュームへの読み取りと書き込みを行うことはできますが、スナップショット・スケジュールを作成することはできません。 複製処理が完了すると、新規ボリュームは元のボリュームから独立し、スナップショットとレプリケーションを使用して通常どおりに管理できます。
