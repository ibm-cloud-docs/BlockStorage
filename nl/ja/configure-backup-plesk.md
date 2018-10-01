---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Plesk を使用したバックアップのための {{site.data.keyword.blockstorageshort}} の構成

以下の説明に従って、Plesk でのバックアップ用に {{site.data.keyword.blockstoragefull}} を構成します。 root または sudo SSH でのアクセスが可能であり、管理者レベルの Plesk フル・アクセス権限があることを前提としています。 ここでの説明は、CentOS7 ホストに基づいています。

**注**: バックアップとリストアに関する Plesk の資料は、[ここ](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}で参照できます。

1. SSH 経由でホストに接続します。

2. マウント・ポイント・ターゲットが存在することを確認します。 <br />
   **注**: Plesk には、バックアップを保管するための 2 つのオプションがあります。 1 つのオプションは、内部 Plesk ストレージ (Plesk サーバー上のバックアップ・ストレージ) です。 もう 1 つのオプションは、外部 FTP ストレージ (Web またはローカル・ネットワーク内の一部の外部サーバー上にあるバックアップ・ストレージ) です。 一般的に、Plesk ボックスでは、内部バックアップは `/var/lib/psa/dumps` に保管され、一時ディレクトリーとして `/tmp` を使用します。 この例では、一時ディレクトリーはローカルのままにしますが、ダンプ・ディレクトリーは STaaS ターゲット (`/backup/psa/dumps`) に移動します。 FTP ユーザー資格情報は不要です。
   
3. [Linux での MPIO iSCSI LUN への接続](accessing_block_storage_linux.html)の説明に従って、{{site.data.keyword.blockstorageshort}} を構成します。 {{site.data.keyword.blockstorageshort}} を `/backup` にマウントし、開始時のマウントを可能にするように `/etc/fstab` を構成します。

4. **オプション**: 既存のバックアップを新規ストレージにコピーします。 `rsync` を使用できます。
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **注**: このコマンドはデータを圧縮して送信し、ハード・リンクを除いてデータを可能な限り保持します。 どのようなファイルが転送されているかに関する情報に加え、最後に簡単な要約を提供します。
    
5. `/etc/psa/psa.conf` を編集して、`DUMP_D` の値が新しいターゲットを指すようにします。 
    - `DUMP_D /backup/psa/dumps` のようになります。 

6. **オプション**: ご使用の特定のユース・ケースおよびビジネス・ニーズに応じて、古いストレージをサーバーから削除し、アカウントから取り消します。


