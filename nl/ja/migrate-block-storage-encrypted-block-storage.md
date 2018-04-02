---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} から暗号化 {{site.data.keyword.blockstorageshort}} へのマイグレーション

エンデュランスやパフォーマンス用の暗号化 {{site.data.keyword.blockstoragefull}} が、限られたデータ・センターで使用可能になっています。以下に、ご使用の {{site.data.keyword.blockstorageshort}} を非暗号化から暗号化へマイグレーションする方法についての情報を示します。プロバイダー管理の暗号化ストレージについて詳しくは、[{{site.data.keyword.blockstorageshort}} 保存時の暗号化に関する記事](block-file-storage-encryption-rest.html)を参照してください。アップグレードされたデータ・センターと使用可能な機能のリストを表示するには、[ここ](new-ibm-block-and-file-storage-location-and-features.html)をクリックしてください。

推奨されるマイグレーション・パスは、両方の LUN に同時に接続し、一方のファイル LUN からもう一方にデータを直接転送することです。詳細は、ご使用のオペレーティング・システム、およびコピー操作中にデータの変更が想定されるかどうかによって異なります

参考までに、より一般的なシナリオを概説しました。ご使用のホストに、非暗号化ファイル LUN が既に接続されていることが前提です。接続されていない場合は、以下に示した指示のうち、実行しているオペレーティング・システムに最もよく合う方に従って、このタスクを実行してください。

- [Linux での {{site.data.keyword.blockstorageshort}} へのアクセス](accessing_block_storage_linux.html)

- [Windows での {{site.data.keyword.blockstorageshort}} へのアクセス](accessing-block-storage-windows.html)

 
## 暗号化 LUN の作成

マイグレーション・プロセスを容易にするため、以下のステップを使用して、同一サイズまたはそれ以上の大きさの暗号化された LUN を作成します。
暗号化エンデュランス・ストレージ LUN の注文

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} ホーム・ページから**「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックするか、または {{site.data.keyword.BluSoftlayer_full}} カタログで、**「インフラストラクチャー」**>**「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックします。

2. {{site.data.keyword.blockstorageshort}} ページで、**「{{site.data.keyword.blockstorageshort}} の注文 (Order Block Storage)」**リンクをクリックします。

3. **「エンデュランス」**を選択します。

4. 元の LUN があるデータ・センターを選択します。 <br/> **注**: 暗号化は限られたデータ・センターでのみ使用可能です。

5. 必要な IOPS ティアを選択します。

6. 必要なストレージ・スペースの量を GB 単位で選択します。TB の場合、1 TB は 1,000 GB、12 TB は 12,000 GB です。

7. スナップショット用に必要なストレージ・スペースの量を GB 単位で入力します。

8. ドロップダウン・リストから VMware OS を選択します。

9. 注文を送信します。

## 暗号化パフォーマンス・ストレージ LUN の注文

1. [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} ホーム・ページから**「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックするか、または {{site.data.keyword.BluSoftlayer_full}} カタログで、**「インフラストラクチャー」**>**「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックします。

2. **「{{site.data.keyword.blockstorageshort}} の注文 (Order Block Storage)」**をクリックします。

3. **「パフォーマンス」**を選択します。

4. 元の LUN があるデータ・センターを選択します。暗号化は、アスタリスク (`*`) が付いたデータ・センターでのみ使用可能であることに注意してください。

5. 必要なストレージ・スペースの量を GB 単位で選択します。TB の場合、1 TB は 1,000 GB、12 TB は 12,000 GB です。

6. 必要な IOPS の量を 100 間隔で入力します。

7. ドロップダウン・リストから VMware OS を選択します。

8. 注文を送信します。

ストレージは 1 分未満でプロビジョンされ、{{site.data.keyword.slportal}} の {{site.data.keyword.blockstorageshort}} ページに表示されます。

 
## 新規ボリュームのホストへの接続

「許可」ホストとは、ボリュームへのアクセス権が付与されたホストのことです。ホストに許可を与えなければ、ご使用のシステムからストレージへアクセスすることもストレージを使用することもできません。ホストにボリュームへのアクセスを許可すると、ユーザー名とパスワードの他に、iSCSI 修飾名 (IQN) が生成されます。これは、マルチパス入出力 (MPIO) iSCSI 接続をマウントするために必要です。

1. **「ストレージ」**>**「{{site.data.keyword.blockstorageshort}}」**をクリックし、使用する LUN 名をクリックします。

2. 当該ページの**「許可ホスト (Authorized Hosts)」**セクションにスクロールします。

3. ページの右側にある**「ホストの許可」**リンクをクリックします。ボリュームにアクセスできるホストをすべて選択します。

 
## スナップショットおよび複製

元の LUN に対してスナップショットと複製が確立されていますか? 「はい」の場合、元のボリュームと同じ設定で複製とスナップショット・スペースをセットアップし、新しい暗号化 LUN のスナップショット・スケジュールを作成する必要があります。 

レプリケーション・ターゲット・データ・センターが暗号化用にアップグレードされていない場合、暗号化されたボリュームの複製は、そのデータ・センターがアップグレードされるまで確立できないことに注意してください。

 
## データのマイグレーション

ユーザーは、元のものと暗号化されたもの両方の {{site.data.keyword.blockstorageshort}} LUN に接続されている必要があります。 
- 接続されていない場合は、前述のステップおよび他の記事で言及されているステップを正しく実行したことを確認してください。この 2 つの LUN の接続について支援が必要な場合は、サポート・チケットをオープンしてください。

### データの考慮事項

この時点で、元の {{site.data.keyword.blockstorageshort}} LUN にはどのようなタイプのデータがあり、そのデータを暗号化 LUN にコピーするにはどういう方法が最善かを検討するといいでしょう。お持ちのデータがバックアップ、静的コンテンツ、およびコピー中に変更が想定されないものなら、重要な考慮事項は何もありません。

お使いの {{site.data.keyword.blockstorageshort}} でデータベースまたは仮想マシンを実行している場合は、元の LUN 上のデータがコピー中に変更されないことを確認して、破損が発生しないようにしてください。帯域幅に少しでも不安がある場合は、マイグレーションはオフピーク時に実行してください。これらの考慮事項について支援が必要な場合は、迷わずサポート・チケットをオープンしてください。
 
### Microsoft Windows

元の {{site.data.keyword.blockstorageshort}} LUN から暗号化 LUN にデータをコピーするには、新規ストレージをフォーマットしてから、Windows エクスプローラーを使用してファイルをコピーします。

 
### Linux

rsync を使用してデータをコピーすることを検討するといいでしょう。以下にコマンドの例を示します。


``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

上記のコマンドを、一度 --dry-run フラグを指定して実行し、パスがすべて正しく並んでいることを確認するようお勧めします。このプロセスが中断される場合は、コピーされていた最後の宛先ファイルを削除して、そのファイルが新しい場所に先頭からコピーされるようにするといいでしょう。

-dry-run フラグを使わずにこのコマンドが完了すれば、データは暗号化された {{site.data.keyword.blockstorageshort}} LUN にコピーされます。スクロールアップしてこのコマンドを再度実行し、何も欠落していないことを確認してください。両方の場所を手動で見直し、欠落している可能性のあるものを探すこともできます。

マイグレーションが完了すれば、実動を暗号化された LUN に移動し、構成から元の LUN を切り離して削除することができます。この削除により、元の LUN に関連付けられたターゲット・サイト上のスナップショットやレプリカもすべて削除されることに注意してください。
