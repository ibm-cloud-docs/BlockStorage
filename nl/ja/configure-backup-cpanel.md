---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# cPanel を使用したバックアップのための {{site.data.keyword.blockstorageshort}} の構成

この記事は、{{site.data.keyword.blockstoragefull}} に保管される、cPanel でのバックアップの構成に役立ちます。root または sudo SSH でのアクセスが可能であり、WebHost Manager (WHM) のフル・アクセス権限があることを前提としています。 ここでの説明は、**CentOS 7** ホストに基づいています。

**注**: **バックアップ・ディレクトリーの構成**に関する cPanel の資料は、 [ここ](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}で参照できます。

1. SSH 経由でホストに接続します。

2. マウント・ポイント・ターゲットが存在することを確認します。 <br />
   **注**: デフォルトで、cPanel システムはバックアップ・ファイルを `/backup` ディレクトリーにローカルに保存します。 本書では、`/backup` が存在し、バックアップが含まれていることを前提としているため、新しいマウント・ポイントとして `/backup2` を使用します。
   
3. [Linux での MPIO iSCSI LUN への接続](accessing_block_storage_linux.html)の説明に従って、{{site.data.keyword.blockstorageshort}} を構成します。 マウント先は必ず `/backup2` にし、ブート時のマウントを可能にするように `/etc/fstab` で構成してください。

4. **オプション**: 既存のバックアップを新規ストレージにコピーします。 `rsync` を使用できます。
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **注**: このコマンドは、ハード・リンクを除いて、データを可能な限り保持しながら、データを圧縮して送信します。どのようなファイルが転送されているかに関する情報に加え、最後に簡単な要約を提供します。
    
5. WebHost Manager にログインし、**「ホーム (Home)」** > **「バックアップ (Backup)」** > **「バックアップ構成 (Backup Configuration)」**をクリックして、バックアップ構成にナビゲートします。

6. 構成を編集して、新しいマウント・ポイントにバックアップを保存します。 
    - /backup/ ディレクトリーの代わりに新しい場所の絶対パスを入力して、デフォルトのバックアップ・ディレクトリーを変更します。 
    - **「バックアップ・ドライブのマウントを有効にする (Enable to mount a backup drive)」**を選択します。 この設定により、バックアップ構成プロセスが `/etc/fstab` ファイルでバックアップ・マウント (`/backup2`) をチェックするようになります。 <br /> **注**: ステージング・ディレクトリーと同じ名前のマウントが存在する場合、バックアップ構成プロセスはドライブをマウントし、そのマウントに情報をバックアップします。 バックアップ・プロセスが終了すると、ドライブは取り外されます。 

7. **「バックアップ構成 (Backup Configuration)」**インターフェースの下部にある**「構成の保存 (Save Configuration)」**をクリックして、変更を適用します。

8. **オプション**: ご使用の特定のユース・ケースおよびビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントから取り消します。

