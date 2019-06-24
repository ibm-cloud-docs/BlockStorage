---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: SLCLI, API, SLCLI usage, Block Storage, provisioning, ordering, managing

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.blockstorageshort}} の SLCLI コマンド
{: #SLCLIcommands}

通常は [{{site.data.keyword.cloud_notm}} コンソール](https://{DomainName}/){: external}で処理する操作を、SLCLI を使用して実行することができます。 例えば、新規ボリュームやスナップショット・スペースおよびレプリケーションの注文、許可の更新、ボリュームのキャンセルなどを、SLCLI で行うことができます。

SLCLI をインストールして使用する方法について詳しくは、[Python API クライアント](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}を参照してください。
{:tip}

## アクセス関連の SLCLI コマンド
* [{{site.data.keyword.blockstorageshort}}の管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## レプリケーション関連の SLCLI コマンド

* [レプリケーション関連の SLCLI コマンド](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## スナップショット関連の SLCLI コマンド

* [スナップショットの注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [スナップショットの管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## ボリューム関連の SLCLI コマンド

* [{{site.data.keyword.blockstorageshort}}・ボリュームの注文](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [複製ボリュームの作成](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [IOPS の調整](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#steps)
  ```
  slcli block volume-modify
  ```
* [キャパシティーの拡張](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#steps)
  ```
  slcli block volume-modify
  ```
* [{{site.data.keyword.blockstorageshort}}の管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [ストレージ制限の管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
