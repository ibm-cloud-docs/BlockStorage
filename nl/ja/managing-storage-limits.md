---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# ストレージ制限の管理
{: #managingstoragelimits}

デフォルトでは、合計 250 の{{site.data.keyword.blockstorageshort}}・ボリュームと{{site.data.keyword.filestorage_short}}・ボリュームをグローバルにプロビジョンできます。

保有しているボリューム数が不明な場合は、次の `slcli` コマンドを使用して、データ・センターごとにボリュームをリストできます。
```
# slcli block volume-count --help
Usage: slcli block volume-count [OPTIONS]

Options:
  -d, --datacenter TEXT  Datacenter shortname
  --sortby TEXT          Column to sort by
  -h, --help             Show this message and exit.
```

[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} でチケットを送信することによって、制限の引き上げを要求できます。 要求が承認されると、特定のデータ・センターに対して設定されているボリューム制限が分かります。  

制限の引き上げを要求するには、チケットをオープンして営業担当員に送信します。

このチケットには、以下の情報を指定してください。

- **チケット・サブジェクト**: データ・センターのボリューム数ストレージ制限の引き上げ要求

- **追加ボリューム要求のユース・ケースは何か ?** <br />
*例えば、新しい VMware データ・ストア、新しい開発とテスト (dev/test) 環境、SQL データベース、ロギングなどの回答が考えられます。*

- **タイプ、サイズ、IOPS、およびロケーションごとに必要な追加のブロック・ボリュームの数は ?** <br />
*例えば、「DAL09 に Endurance 2 TB @ 4 IOPS を 25 個」や「WDC04 に Performance 4 TB @ 2 IOPS を 25 個」などの回答が考えられます。*

- **タイプ、サイズ、IOPS、およびロケーションごとに必要な追加のファイル・ボリュームの数は ?** <br />
*例えば、「DAL09 に Performance 20 GB @ 10 IOPS を 25 個」や「SJC03 に Endurance 2 TB @ 0.25 IOPS を 50 個」などの回答が考えられます。*

- **要求したボリュームの増強すべてのプロビジョンをどのくらいで終わらせたいか見積もってください。** <br />
 *例えば、「90 日」などの回答が考えられます。*

- **これらのボリュームの 90 日間で想定される平均容量使用率の予測を指定してください。** <br />
*例えば、「30 日間で 25% 使用、60 日間で 50% 使用、90 日間で 75% 使用が想定される」などの回答が考えられます。*

上記の要求内のすべての質問と命題に回答してください。 処理と承認を行う際にこれらが必要になります。
{:important}

チケット・プロセスによって制限が更新されたことが通知されます。
