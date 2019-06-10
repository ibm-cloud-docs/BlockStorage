---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# コンソールを使用した {{site.data.keyword.blockstorageshort}} の注文
{: #orderingthroughConsole}

容量および IOPS のニーズを満たすように、{{site.data.keyword.blockstorageshort}} をプロビジョンして微調整を行うことができます。 パフォーマンスを指定するための 2 つのオプションを使用して、ストレージを最大限に活用します。

- パフォーマンス要件が明確に定義されていないワークロードに合わせて、事前定義されたパフォーマンス・レベルを備えたエンデュランス IOP 層から選択できます。
- 「パフォーマンス」で IOPS の合計数を指定することで、特定のパフォーマンス要件を満たすようにストレージを微調整できます。

## 事前定義の IOPS 層 (エンデュランス) を備えた {{site.data.keyword.blockstorageshort}} の注文

1. [IBM Cloud カタログ](https://{DomainName}/catalog){: external}にログインし、**「ストレージ」**をクリックします。 次に、**「{{site.data.keyword.blockstorageshort}}」**を選択し、**「作成」**をクリックします。

   または、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}にログインして、**「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックすることもできます。右上で、**「{{site.data.keyword.blockstorageshort}} の注文」**をクリックします。

2. デプロイメント・**ロケーション** (データ・センター) を選択します。
   - 新規ストレージは、所持している計算ホストと同じロケーションに追加するようにしてください。
3. 請求処理。 機能が改善されたデータ・センター (アスタリスクでマークされている) を選択した場合は、月次請求と毎時請求のいずれかを選択できます。
     1. **「毎時」**の請求で、アカウントにブロック LUN が存在した時間数は、LUN が削除されるとき、または請求サイクルが終わるときに計算されます。 いずれか早いほうのタイミングです。 使用期間が数日ないし 1 カ月未満のストレージには毎時請求が適しています。 毎時請求が選択できるのは、これらの [限られたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)にプロビジョンされたストレージのみです。
     2. **月次**請求では、価格は作成日から請求サイクル終了までで案分計算され、即時に請求が行われます。 請求サイクルの終了前にブロック LUN が削除された場合でも、返金されません。 長期間 (1 カ月以上) 保管およびアクセスする必要があるデータを使用する実動ワークロードで使用されるストレージには月次請求が適しています。

        改善された機能で更新**されていない**データ・センターにプロビジョンされたストレージには、デフォルトで月次請求タイプが使用されます。
        {:important}
4. **「新規ストレージ・サイズ (New Storage Size)」**フィールドにストレージ・サイズを入力します。
5. **「ストレージ IOPS オプション (Storage IOPS Options)」**セクションで、**「エンデュランス (層化 IOPS) (Endurance (tiered IOPS))」**を選択します。
6. アプリケーションに必要な IOPS 層を選択します。
    - **0.25 IOPS/GB** は、入出力負荷が低いワークロード用に設計されています。 通常、これらのワークロードは、一時に非アクティブになっているデータの割合が高いという特徴があります。 アプリケーションの例として、メールボックスの保管や部門レベルのファイル共有が挙げられます。
    - **2 IOPS/GB** は、大部分の一般的な用途のために設計されています。 アプリケーションの例として、Web アプリケーションをバッキングする小規模なデータベースのホスティングや、ハイパーバイザーの仮想マシン・ディスク・イメージが挙げられます。
    - **4 IOPS/GB** は、高負荷ワークロード用に設計されています。 通常、これらのワークロードには、一時に大部分のデータがアクティブであるという特徴があります。 アプリケーションの例として、トランザクション・データベースやその他の高いパフォーマンスを必要とするデータベースが挙げられます。
    - **10 IOPS/GB** は、NoSQL データベースや Analytics のデータ処理などで作成される最も厳しいワークロード用に設計されています。 この層は、[限定されたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)にプロビジョンされた最大サイズ 4 TB のストレージに対して使用可能です。
7. **「スナップショット・スペース・サイズの指定」**をクリックし、リストからスナップショット・サイズを選択します。 このスペースは、使用可能なスペースに加算されます。 スナップショット・スペースの考慮事項および推奨事項については、『[スナップショットの注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)』を参照してください。
8. リストからご使用の**「OS タイプ (OS Type)」**を選択します。<br/>

   この選択は、ホストが実行されているオペレーティング・システムに基づき、後で変更することはできません。 例えば、サーバーが Ubuntu または RHEL の場合は、「Linux」を選択します。 ホストが Windows Server 2012 または Windows Server 2016 の場合は、リストから、「Windows 2008+」オプションを選択します。 さまざまな Windows オプションについて詳しくは、[FAQ](/docs/infrastructure/BlockStorage?topic=block-storage-faqs)を参照してください。
   {:tip}
9. 右方で発注要約を確認し、割引コードがある場合は適用します。

   注文の処理時に割引が適用されます。
   {:note}
10. ご使用条件を確認したら、**「サード・パーティー・サービス契約を読み、同意します」**ボックスにチェック・マークを入れます。
11. **「作成」**をクリックします。 新規ストレージ割り振りは数分後に使用可能になります。

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} ボリュームをプロビジョンできます。 ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 制限の引き上げについては、[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)を参照してください。<br/><br/>同時許可の制限については、[FAQ](/docs/infrastructure/BlockStorage?topic=block-storage-faqs) を参照してください。
{:important}

## カスタム IOPS (パフォーマンス) を備えた {{site.data.keyword.blockstorageshort}} の注文

1. [IBM Cloud カタログ](https://{DomainName}/catalog){: external}にログインし、**「ストレージ」**をクリックします。 次に、「{{site.data.keyword.blockstorageshort}}」を選択し、**「作成」**をクリックします。

   または、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}にログインして、**「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックすることもできます。右上で、**「{{site.data.keyword.blockstorageshort}} の注文」**をクリックします。
2. **「ロケーション」**をクリックして、データ・センターを選択します。
   - 新規ストレージは、所持している計算ホストと同じロケーションに追加するようにしてください。
3. 請求処理。 機能が改善されたデータ・センター (アスタリスクでマークされている) を選択した場合は、月次請求と毎時請求のいずれかを選択できます。
     1. **「毎時」**の請求で、アカウントにブロック LUN が存在した時間数は、LUN が削除されるとき、または請求サイクルが終わるときに計算されます。 いずれか早いほうのタイミングです。 使用期間が数日ないし 1 カ月未満のストレージには毎時請求が適しています。 毎時請求が選択できるのは、これらの [限られたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)にプロビジョンされたストレージのみです。
     2. **月次**請求では、価格は作成日から請求サイクル終了までで案分計算され、即時に請求が行われます。 請求サイクルの終了前にブロック LUN が削除された場合でも、返金されません。 長期間 (1 カ月以上) 保管およびアクセスする必要があるデータを使用する実動ワークロードで使用されるストレージには月次請求が適しています。

        改善された機能で更新**されていない**データ・センターにプロビジョンされたストレージには、デフォルトで月次請求タイプが使用されます。
        {:note}
4. **「新規ストレージ・サイズ (New Storage Size)」**フィールドにストレージ・サイズを入力します。
5. **「ストレージ IOPS オプション (Storage IOPS Options)」**セクションで、**「パフォーマンス (割り振り IOPS) (Performance (Allocated IOPS))」**を選択します。
6. **「パフォーマンス (割り振り IOPS) (Performance (Allocated IOPS))」**フィールドに IOPS を入力します。
7. **「スナップショット・スペース・サイズの指定」**をクリックし、リストからスナップショット・サイズを選択します。 このスペースは、使用可能なスペースに加算されます。 スナップショット・スペースの考慮事項および推奨事項については、『[スナップショットの注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots)』を参照してください。
8. リストからご使用の**「OS タイプ (OS Type)」**を選択します。<br/>

   この選択は、ホストが実行されているオペレーティング・システムに基づき、後で変更することはできません。 例えば、サーバーが Ubuntu または RHEL の場合は、「Linux」を選択します。 ホストが Windows Server 2012 または Windows Server 2016 の場合は、リストから、「Windows 2008+」オプションを選択します。 さまざまな Windows オプションについて詳しくは、[FAQ](/docs/infrastructure/BlockStorage?topic=block-storage-faqs)を参照してください。
   {:tip}
9. 右方で発注要約を確認し、割引コードがある場合は適用します。

   注文の処理時に割引が適用されます。
   {:note}
10. ご使用条件を確認したら、**「サード・パーティー・サービス契約を読み、同意します」**ボックスにチェック・マークを入れます。
11. **「作成」**をクリックします。 新規ストレージ割り振りは数分後に使用可能になります。

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} ボリュームをプロビジョンできます。 ご使用のボリュームの数を増やすには、営業担当員にお問い合わせください。 制限の引き上げについては、[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)を参照してください。<br/><br/>同時許可の制限については、[FAQ](/docs/infrastructure/BlockStorage?topic=block-storage-faqs) を参照してください。
{:important}

## 新規ストレージの接続
{: #mountingnewLUN}

プロビジョニング要求が完了したら、ホストに対して新規ストレージへのアクセスを許可し、接続を構成します。 ホストのオペレーティング・システムに応じて、適切なリンクをたどってください。
- [Linux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [XenServer 共有ストレージでの LUN のマウント](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [cPanel を使用したバックアップ用のブロック・ストレージの構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Plesk を使用したバックアップ用のブロック・ストレージの構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## 災害復旧の際の考慮事項

データ損失を防ぎ、事業継続性を確保するために、サーバーおよびストレージを別のデータ・センターにレプリケーションすることを検討してください。 レプリケーションすることで、データはスナップショット・スケジュールに従い 2 つの異なる場所で同期されます。 詳しくは、[データのレプリケーション](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication)を参照してください。

使用しているボリュームを複製したものを元のボリュームとは独立して使用する場合は、[複製ブロック・ボリュームの作成](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)を参照してください。

## 請求書の {{site.data.keyword.blockstorageshort}} の識別

すべての LUN は、明細項目として請求書に表示されます。 「エンデュランス」は「エンデュランス・ストレージ・サービス」と表示され、「パフォーマンス」は「パフォーマンス・ストレージ・サービス」と表示されます。価格はご使用のストレージ・レベルによって異なります。 「エンデュランス」や「パフォーマンス」を展開すれば、それが {{site.data.keyword.blockstorageshort}} であることを確認できます。
