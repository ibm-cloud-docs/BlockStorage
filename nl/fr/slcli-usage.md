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

# Commandes SLCLI de {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

Vous pouvez utiliser l'interface SLCLI pour effectuer des actions normalement gérées via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. Par exemple, avec l'interface SLCLI vous pouvez passer des commandes de volumes, d'espace d'instantané et de réplication, mettre à jour des autorisations, annuler des volumes, etc.

Pour plus d'informations sur l'installation et l'utilisation de l'interface SLCLI, voir [Client API Python](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Commandes SLCLI liées aux accès
* [Gestion de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## Commandes SLCLI liées à la réplication

* [Commandes SLCLI liées à la réplication](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## Commandes SLCLI liées aux instantanés

* [Commande d'instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [Gestion des instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## Commandes SLCLI liées aux volumes

* [Commande d'un volume {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [Création d'un volume dupliqué](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [Réglage des IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#adjustingsteps)
  ```
  slcli block volume-modify
  ```
* [Augmentation de la capacité](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#resizingsteps)
  ```
  slcli block volume-modify
  ```
* [Gestion de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [Gestion des limites de stockage](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
