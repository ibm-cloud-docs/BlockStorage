---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-25"

---
{:new_window: target="_blank"}

# 既存の {{site.data.keyword.blockstorageshort}} の拡張 {{site.data.keyword.blockstorageshort}} へのアップグレード

限定されたデータ・センターで、拡張 {{site.data.keyword.blockstoragefull}} が使用可能になりました。 アップグレードされたデータ・センターと、調整可能な IOPS レートや拡張可能なボリュームなどの使用可能な機能のリストを確認するには、[ここ](new-ibm-block-and-file-storage-location-and-features.html)をクリックしてください。 プロバイダー管理の暗号化ストレージについて詳しくは、[{{site.data.keyword.blockstorageshort}} 保存時の暗号化](block-file-storage-encryption-rest.html)に関する記事を参照してください。

推奨されるマイグレーション・パスは、両方の LUN に同時に接続し、一方の LUN からもう一方にデータを直接転送することです。 詳細は、ご使用のオペレーティング・システム、およびコピー操作中にデータの変更が想定されるかどうかによって異なります。 

ご使用のホストに、非暗号化 LUN が既に接続されていることが前提です。 接続されていない場合は、ご使用のオペレーティング・システムに最適な指示に従って、このタスクを実行してください。

- [Linux での {{site.data.keyword.blockstorageshort}} へのアクセス](accessing_block_storage_linux.html)
- [Windows での {{site.data.keyword.blockstorageshort}} へのアクセス](accessing-block-storage-windows.html)

 
## 新規 {{site.data.keyword.blockstorageshort}} の作成

**重要**! API を使用して注文する場合は、「Storage as a Service」パッケージを指定して、更新済みの機能を新規ストレージと一緒に取得してください。

以下の手順は、{{site.data.keyword.slportal}}から拡張 LUN を注文する場合のものです。 マイグレーションを円滑にするために、新しい LUN は、元のボリュームと同じかそれより大きいサイズにしてください。

### エンデュランス LUN の注文

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} から**「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックするか、または  {{site.data.keyword.BluSoftlayer_full}} カタログから、**「インフラストラクチャー」>「ストレージ」>「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. 右上隅で、**「{{site.data.keyword.blockstorageshort}} の注文」**をクリックします。
3. **「ストレージ・タイプの選択」**リストから**「エンデュランス」**を選択します。
4. デプロイメント・**ロケーション** (データ・センター) を選択します。
   - 新規ストレージは、必ず前のボリュームと同じロケーションに追加してください。
5. 請求オプションを選択します。 時間単位と月単位の請求から選択できます。
6. IOPS ティアを選択します。
7. **「ストレージ・サイズの選択」**をクリックし、リストから目的のストレージ・サイズを選択します。
8. **「スナップショット・スペース・サイズの指定」**をクリックし、リストからスナップショット・サイズを選択します。 このスペースは、使用可能なスペースに加算されます。 スナップショット・スペースの考慮事項および推奨事項については、『[スナップショットの注文](ordering-snapshots.html)』を参照してください。
9. リストからご使用の**「OS タイプ (OS Type)」**を選択します。
10. **「続行」**をクリックします。 月額課金と日割り計算額が表示されます。これが注文の詳細を確認できる最後の機会となります。
11. **「マスター・サービス契約を読み ...」**チェック・ボックスをクリックし、**「注文」**をクリックします。

### パフォーマンス LUN の注文

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} から**「ストレージ」**、**「{{site.data.keyword.blockstorageshort}}」**をクリックするか、または  {{site.data.keyword.BluSoftlayer_full}} カタログから、**「インフラストラクチャー」>「ストレージ」>「{{site.data.keyword.blockstorageshort}}」**をクリックします。
2. 右側で、**「{{site.data.keyword.blockstorageshort}} の注文」**をクリックします。
3. **「ストレージ・タイプの選択 (Select Storage Type)」**リストから、**「パフォーマンス」**を選択します。
4. **「ロケーション」**をクリックして、データ・センターを選択します。
   - 新規ストレージは、以前に注文したホストと同じロケーションに追加するようにしてください。
5. 請求オプションを選択します。 時間単位と月単位の請求から選択できます。
6. 適切な**ストレージ・サイズ**を選択します。
7. **「IOPS の指定 (Specify IOPS)」**フィールドに IOPS を入力します。
8. **「続行」**をクリックします。 月額課金と日割り計算額が表示されます。これが注文の詳細を確認できる最後の機会となります。 注文を変更する場合は、**「戻る」**をクリックします。
9. **「マスター・サービス契約を読み ...」**チェック・ボックスをクリックし、**「注文」**をクリックします。

ストレージは 1 分もしないうちにプロビジョンされ、{{site.data.keyword.slportal}}の「{{site.data.keyword.blockstorageshort}}」ページに表示されます。


 
## 新規 {{site.data.keyword.blockstorageshort}} のホストへの接続

「許可」ホストとは、ボリュームに対するアクセス権が付与されたホストのことです。 ホストの許可がなければ、システムからストレージにアクセスすることも、ストレージを使用することもできません。 ホストにボリュームへのアクセスを許可すると、ユーザー名とパスワードの他に、iSCSI 修飾名 (IQN) が生成されます。これは、マルチパス入出力 (MPIO) iSCSI 接続をマウントするために必要です。

1. **「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックし、使用する LUN 名をクリックします。

2. **「許可ホスト (Authorized Hosts)」**にスクロールします。

3. 右側で、**「ホストの許可」**をクリックします。 ボリュームにアクセスできるホストをすべて選択します。

 
## スナップショットおよび複製

元の LUN に対してスナップショットと複製が確立されていますか? 「はい」の場合、元のボリュームと同じ設定で複製とスナップショット・スペースをセットアップし、新しい LUN のスナップショット・スケジュールを作成する必要があります。 

レプリケーション・ターゲット・データ・センターがまだアップグレードされていない場合は、そのデータ・センターがアップグレードされるまで、新規ボリュームの複製を確立することはできません。

 
## データのマイグレーション

1. 元の {{site.data.keyword.blockstorageshort}} LUN と新規の {{site.data.keyword.blockstorageshort}} LUN の両方に接続します。 
  - 2 つの LUN をホストに接続する際に支援が必要な場合は、サポート・チケットをオープンしてください。

2. 元の {{site.data.keyword.blockstorageshort}} LUN にはどのようなタイプのデータがあり、そのデータを新しい LUN にコピーするにはどういう方法が最善かを検討してください。 
  - バックアップや静的コンテンツがあり、コピー中に変更が想定されないものなら、重要な考慮事項は何もありません。
  - {{site.data.keyword.blockstorageshort}} 上でデータベースまたは仮想マシンを実行している場合は、データ破壊を避けるため、コピー中にデータが変更されないようにしてください。 帯域幅に少しでも不安がある場合は、マイグレーションは非ピーク時に実行してください。 これらの考慮事項について支援が必要な場合は、サポート・チケットをオープンしてください。
 
3. データ全体をコピーします。
   - **Microsoft Windows** - 元の {{site.data.keyword.blockstorageshort}} LUN から新規 LUN にデータをコピーするには、新規ストレージをフォーマットしてから、Windows エクスプローラーを使用してファイルをコピーします。
   - **Linux** - `rsync` を使用して、データをコピーできます。 次に例を示します。
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```
   
   上記のコマンドを、一度 `--dry-run` フラグを指定して実行し、パスがすべて正しく並んでいることを確認するようお勧めします。 このプロセスが中断される場合は、コピーされていた最後の宛先ファイルを削除して、そのファイルが最初から新しい場所にコピーされるようにします。<br/>
   `--dry-run` フラグを使わずにこのコマンドが完了すれば、データは新規 {{site.data.keyword.blockstorageshort}} LUN にコピーされます。 このコマンドを再度実行し、何も欠落していないことを確認してください。 両方の場所を手動で見直し、欠落している可能性のあるものを探すこともできます。<br/>
   マイグレーションが完了すれば、実動を新規 LUN に移動できます。 その後、構成から元の LUN を切り離して削除できます。 この削除により、元の LUN に関連付けられたターゲット・サイト上のスナップショットやレプリカもすべて削除されます。
