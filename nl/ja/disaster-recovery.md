---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, inaccessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# 災害復旧 - 1 次ボリュームがアクセス不能になった場合のフェイルオーバー
{: #dr-inaccessible}

1 次サイトで壊滅的な障害や災害が発生し、1 次サイトが停止した場合は、以下の操作を実行して、2 次サイトのデータに速やかにアクセスできます。

## 2 次サイトにあるレプリカ・ボリュームの複製を使用するフェイルオーバー

1. [IBM Cloud コンソール](https://{DomainName}/){: external} にログインし、左上の**「メニュー」**アイコンをクリックします。 **「クラシック・インフラストラクチャー」**を選択します。


   または、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} にログインできます。
2. **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックします。
3. リスト内の LUN のレプリカをクリックして、**「詳細」**ページを表示します。
4. **「詳細」**ページで、下にスクロールして既存のスナップショットを選択し、**「アクション」** > **「複製」**をクリックします。
5. 新規ボリュームの容量 (サイズを増やす) または IOP に対して、必要な更新を行います。
6. 必要に応じて、新規ボリュームのスナップショット・スペースを更新します。
7. **「続行」**をクリックして、複製を注文します。

ボリュームが作成されるとすぐに、ホストにアタッチして読み取り/書き込み操作を実行できます。 元のボリュームから複製ボリュームにデータがコピーされている間、複製が進行中であることが詳細ページに表示されます。 複製処理が完了すると、新規ボリュームは元のボリュームから独立し、スナップショットとレプリケーションを使用して通常どおりに管理できます。

## 元の 1 次サイトへのフェイルバック

元の 1 次サイトに実動を戻す場合は、以下のステップを実行する必要があります。

1. [IBM Cloud コンソール](https://{DomainName}/){: external} にログインし、左上の**「メニュー」**アイコンをクリックします。 **「クラシック・インフラストラクチャー」**を選択します。


   または、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} にログインできます。
2. **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**をクリックします。
3. LUN 名をクリックし、スナップショット・スケジュールを作成します (まだ存在していない場合)。

   スナップショット・スケジュールについて詳しくは、[スナップショットの管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots#addingschedule)を参照してください。
   {:tip}
4. **「レプリカ」**をクリックし、**「レプリケーションの購入 (Purchase a replication)」**をクリックします。
5. レプリケーションに使用する既存のスナップショット・スケジュールを選択します。 リストに、アクティブなすべてのスナップショット・スケジュールが示されます。
6. **「ロケーション」**をクリックして、元の実動場所のデータ・センターを選択します。
7. **「続行」**をクリックします。
8. **「マスター・サービス契約を読み... (I have read the Master Service Agreement…)」**チェック・ボックスをクリックして、**「注文する (Place Order)」**をクリックします。

レプリケーションが完了したら、新しいレプリカの複製ボリュームを作成する必要があります。
{:important}

1. **「ストレージ」** > **「{{site.data.keyword.blockstorageshort}}」**に戻ります。
2. リスト内の LUN のレプリカをクリックして、**「詳細」**ページを表示します。
3. **「詳細」**ページで、下にスクロールして既存のスナップショットを選択し、**「アクション」** > **「複製」**をクリックします。
4. 新規ボリュームの容量 (サイズを増やす) または IOP に対して、必要な更新を行います。
5. 必要に応じて、新規ボリュームのスナップショット・スペースを更新します。
6. **「続行」**をクリックして、複製を注文します。

複製処理が完了したら、レプリケーションと、元の 1 次サイトにデータを戻すために使用されたボリュームをキャンセルできます。 複製ストレージが 1 次ストレージになり、元の 2 次サイトへのレプリケーションが再び確立できます。
