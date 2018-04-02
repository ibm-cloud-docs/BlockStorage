---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Plesk を使用したバックアップのための {{site.data.keyword.blockstorageshort}} の構成

この記事では、Plesk でのバックアップのために {{site.data.keyword.blockstoragefull}} を構成する方法を説明します。root または sudo SSH でのアクセスが可能であり、管理者レベルの Plesk フル・アクセス権限があることを前提としています。この例は、CentOS7 ホストに基づいています。

**注**: バックアップとリストアに関する Plesk の資料は、[ここ](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}で参照できます。

1. SSH 経由でホストに接続します。

2. マウント・ポイント・ターゲットが存在することを確認します。<br />
   **注**: Plesk には、バックアップを保管するための 2 つのオプションがあります。1 つは、内部 Plesk ストレージ (ご使用の Plesk サーバー上にあるバックアップ・ストレージ) です。もう 1 つは、外部 FTP ストレージ (Web またはお客様のローカル・ネットワーク内の外部サーバー上にあるバックアップ・ストレージ) です。一般的に、Plesk ボックスでは、内部バックアップは `/var/lib/psa/dumps` に保管され、一時ディレクトリーとして `/tmp` を使用します。この例では、一時ディレクトリーはローカルのままにしますが、ダンプ・ディレクトリーは STaaS ターゲット (`/backup/psa/dumps`) に移動します。FTP ユーザー資格情報は不要です。
   
3. [Linux での MPIO iSCSI LUN への接続](accessing_block_storage_linux.html)の説明に従って、{{site.data.keyword.blockstorageshort}} を構成します。{{site.data.keyword.blockstorageshort}} を `/backup` にマウントし、ブート時のマウントを可能にするように `/etc/fstab` を構成します。

4. **オプション**: 既存のバックアップを新規ストレージにコピーします。例えば、以下のように `rsync` を使用します。
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **注**: このコマンドは、データを可能な限り保持しながら (ハードリンクを除く)、圧縮されたデータを送信し、どのようなファイルが転送されているかに関する情報に加え、最後に簡単な要約を提供します。
    
5. `/etc/psa/psa.conf` を編集して、`DUMP_D` の値が新しいターゲットを指すようにします。 
    -  `DUMP_D /backup/psa/dumps` のようになります。 

6. **オプション**: ご使用の特定のユース・ケースおよびビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントから取り消します。


