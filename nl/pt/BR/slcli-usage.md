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

# Comandos SLCLI para o {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

É possível usar a SLCLI para executar ações que normalmente são manipuladas por meio do [console
do {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}. Por exemplo, com a SLCLI, é possível fazer pedidos de volumes, espaço de captura instantânea, replicação, atualizar autorizações, cancelar volumes e assim por diante.

Para obter mais informações sobre como instalar e usar a SLCLI, consulte [Cliente da API do Python](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Comandos da SLCLI relacionados a acesso
* [Gerenciando o {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## Comandos da SLCLI relacionados a replicação

* [Comandos da SLCLI relacionados a replicação](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## Comandos da SLCLI relacionados a capturas instantâneas

* [Pedindo capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [Gerenciando capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## Comandos da SLCLI relacionados a volume

* [Pedindo um volume do {{site.data.keyword.blockstorageshort}} ](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [Criando um volume duplicado](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [Ajustando o IOPS](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#steps)
  ```
  slcli block volume-modify
  ```
* [Expandindo a capacidade](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#steps)
  ```
  slcli block volume-modify
  ```
* [Gerenciando o {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [Gerenciando limites de armazenamento](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
