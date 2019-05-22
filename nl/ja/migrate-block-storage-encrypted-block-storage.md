---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# 既存の {{site.data.keyword.blockstorageshort}} の拡張 {{site.data.keyword.blockstorageshort}} へのアップグレード
{: #migratestorage}

限定されたデータ・センターで、拡張 {{site.data.keyword.blockstoragefull}} が使用可能になりました。 アップグレードされたデータ・センターと、調整可能な IOPS レートや拡張可能なボリュームなどの使用可能な機能のリストを確認するには、[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)をクリックしてください。 プロバイダー管理の暗号化ストレージについて詳しくは、[{{site.data.keyword.blockstorageshort}} 保存データの暗号化](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)を参照してください。

推奨されるマイグレーション・パスは、両方の LUN に同時に接続し、一方の LUN からもう一方にデータを直接転送することです。 詳細は、ご使用のオペレーティング・システム、およびコピー操作中にデータの変更が想定されるかどうかによって異なります。

ホストに非暗号化 LUN が既に接続されていることを前提としています。 接続されていない場合は、ご使用のオペレーティング・システムに最適な指示に従って、このタスクを実行してください。

- [Linux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

これらのデータ・センターでプロビジョンされる拡張{{site.data.keyword.blockstorageshort}}・ボリュームはすべて、非暗号化ボリュームとは異なるマウント・ポイントになります。 両方のストレージ・ボリュームに正しいマウント・ポイントを使用するために、コンソールの**「ボリュームの詳細」**ページでマウント・ポイント情報を確認することができます。 API 呼び出し `SoftLayer_Network_Storage::getNetworkMountAddress()` を使用して正しいマウント・ポイントを取得することもできます。
{:tip}

## {{site.data.keyword.blockstorageshort}}の作成

API を使用して注文する場合は、「Storage as a Service」パッケージを指定して、更新済みの機能を新規ストレージと一緒に取得してください。
{:important}

拡張 LUN は IBM Cloud Console および {{site.data.keyword.slportal}} を通して注文することができます。 マイグレーションを円滑にするために、新しい LUN は、元のボリュームと同じかそれより大きいサイズにしてください。

- [定義済み IOPS 層 (エンデュランス) を備えた {{site.data.keyword.blockstorageshort}} の注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [カスタム IOPS (パフォーマンス) を備えた {{site.data.keyword.blockstorageshort}} の注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

新しいストレージが数分後にマウント可能になります。 そのストレージは「リソース・リスト」と「{{site.data.keyword.blockstorageshort}} リスト」に表示されます。

## 新規 {{site.data.keyword.blockstorageshort}} のホストへの接続

「許可」ホストとは、ボリュームに対するアクセス権が付与されたホストのことです。 ホストの許可がなければ、システムからストレージにアクセスすることも、ストレージを使用することもできません。 ホストにボリュームへのアクセスを許可すると、ユーザー名とパスワードの他に、iSCSI 修飾名 (IQN) が生成されます。これは、マルチパス入出力 (MPIO) iSCSI 接続をマウントするために必要です。

1. **「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックし、使用する LUN 名をクリックします。
2. **「許可ホスト (Authorized Hosts)」**にスクロールします。
3. 右側で、**「ホストの許可」**をクリックします。 ボリュームにアクセスできるホストをすべて選択します。


## スナップショットおよび複製

元の LUN に対してスナップショットと複製が確立されていますか? 「はい」の場合、元のボリュームと同じ設定で複製とスナップショット・スペースをセットアップし、新しい LUN のスナップショット・スケジュールを作成する必要があります。

レプリケーション・ターゲット・データ・センターがまだアップグレードされていない場合は、そのデータ・センターがアップグレードされるまで、新規ボリュームの複製を確立することはできません。


## データのマイグレーション

1. 元と新規の両方の {{site.data.keyword.blockstorageshort}} LUN に接続します。

   2 つの LUN をホストに接続する際に支援が必要な場合は、サポート Case をオープンしてください。
   {:tip}

2. 元の {{site.data.keyword.blockstorageshort}} LUN にはどのようなタイプのデータがあり、そのデータを新しい LUN にコピーするにはどういう方法が最善かを検討してください。
  - バックアップや静的コンテンツがあり、コピー中に変更が想定されないものなら、あまり心配する必要はありません。
  - {{site.data.keyword.blockstorageshort}} 上でデータベースまたは仮想マシンを実行している場合は、データ破壊を避けるため、コピー中にデータが変更されないようにしてください。
  - 帯域幅に少しでも不安がある場合は、マイグレーションは非ピーク時に実行してください。
  - これらの考慮事項について支援が必要な場合は、サポート Case をオープンしてください。

3. データ全体をコピーします。
   - **Microsoft Windows** の場合、新しいストレージをフォーマットしてから、Windows エクスプローラーを使用して元の {{site.data.keyword.blockstorageshort}} LUN から新しい LUN にデータをコピーします。
   - **Linux** の場合、`rsync` を使用してデータをコピーできます。
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   上記のコマンドを、一度 `--dry-run` フラグを指定して実行し、パスがすべて正しく並んでいることを確認するようお勧めします。 このプロセスが中断される場合は、コピーされていた最後の宛先ファイルを削除して、そのファイルが最初から新しい場所にコピーされるようにします。<br/>
   `--dry-run` フラグを使わずにこのコマンドが完了すれば、データは新規 {{site.data.keyword.blockstorageshort}} LUN にコピーされます。 このコマンドを再度実行し、何も欠落していないことを確認してください。 両方の場所を手動で見直し、欠落している可能性のあるものを探すこともできます。<br/>
   マイグレーションが完了すれば、実動を新規 LUN に移動できます。 その後、構成から元の LUN を切り離して削除できます。 この削除により、元の LUN に関連付けられたターゲット・サイト上のスナップショットやレプリカもすべて削除されます。
