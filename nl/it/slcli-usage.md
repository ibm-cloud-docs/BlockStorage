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

# Comandi SLCLI per {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

Puoi utilizzare la SLCLI per eseguire delle azioni che vengono normalmente gestite tramite la [console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}. Ad esempio, con la SLCLI puoi inserire degli ordini per i volumi, lo spazio di istantanea, la replica, aggiornare le autorizzazioni, eliminare i volumi e così via.

Per ulteriori informazioni su come installare e utilizzare la SLCLI, vedi [Python API Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Comandi SLCLI correlati all'accesso
* [Gestione di {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## Comandi SLCLI correlati alla replica

* [Comandi SLCLI correlati alla replica](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## Comandi SLCLI correlati alle istantanee

* [Ordinazione di istantanee](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [Gestione delle istantanee](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## Comandi SLCLI correlati al volume

* [Ordinazione di un volume {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [Creazione di un volume duplicato](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [Modifica dell'IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#steps)
  ```
  slcli block volume-modify
  ```
* [Espansione della capacità](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#steps)
  ```
  slcli block volume-modify
  ```
* [Gestione di {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [Gestione dei limiti di archiviazione](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
