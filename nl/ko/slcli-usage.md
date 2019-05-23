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

# {{site.data.keyword.blockstorageshort}}의 SLCLI 명령
{: #SLCLIcommands}

SLCLI를 사용하여 일반적으로 [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}을 통해 처리되는 조치를 수행할 수 있습니다. 예를 들어, SLCLI를 사용하면 볼륨, 스냅샷 영역, 복제에 대한 주문, 권한 업데이트 및 볼륨 취소 등의 작업을 수행할 수 있습니다.

SLCLI 설치 및 사용 방법에 관한 자세한 정보는 [Python API 클라이언트](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}를 참조하십시오.
{:tip}

## 액세스 관련 SLCLI 명령
* [{{site.data.keyword.blockstorageshort}} 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## 복제 관련 SLCLI 명령

* [복제 관련 SLCLI 명령](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## 스냅샷 관련 SLCLI 명령

* [스냅샷 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [스냅샷 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## 볼륨 관련 SLCLI 명령

* [{{site.data.keyword.blockstorageshort}} 볼륨 주문](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [복제 볼륨 작성](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [IOPS 조정](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli block volume-modify
  ```
* [용량 확장](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#resizingsteps)
  ```
  slcli block volume-modify
  ```
* [{{site.data.keyword.blockstorageshort}} 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [스토리지 한계 관리](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
