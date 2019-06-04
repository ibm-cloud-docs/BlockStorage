---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: SLCLI, API, SLCLI usage, Block Storage, provisioning, ordering, managing

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# {{site.data.keyword.blockstorageshort}} 的 SLCLI 指令
{: #SLCLIcommands}

您可以使用 SLCLI 採取通常透過 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} 處理的動作。例如，使用 SLCLI 您可以針對磁區、Snapshot 空間、抄寫、更新授權、取消磁區等等而下訂單。

若要進一步瞭解如何安裝及使用 SLCLI，請參閱 [Python API 用戶端](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}。
{:tip}

## 存取相關的 SLCLI 指令
* [管理 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## 抄寫相關的 SLCLI 指令

* [抄寫相關的 SLCLI 指令](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## Snapshot 相關的 SLCLI 指令

* [訂購 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [管理 Snapshot](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## 磁區相關的 SLCLI 指令

* [訂購 {{site.data.keyword.blockstorageshort}} 磁區](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [建立重複磁區](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [調整 IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#steps)
  ```
  slcli block volume-modify
  ```
* [擴充容量](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#steps)
  ```
  slcli block volume-modify
  ```
* [管理 {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [管理儲存空間限制](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
