---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: SLCLI, API, SLCLI usage, Block Storage, provisioning, ordering, managing

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# SLCLI-Befehle für {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

Sie können die SLCLI verwenden, um Aktionen auszuführen, die normalerweise über das [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} ausgeführt werden. So können Sie über die SLCLI beispielsweise Datenträger, Snapshotbereiche und Replikationen bestellen, Berechtigungen aktualisieren, Datenträger stornieren usw. 

Weitere Informationen zur Installation und Verwendung der SLCLI finden Sie unter [Python-API-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## SLCLI-Befehle im Zusammenhang mit dem Zugriff
* [{{site.data.keyword.blockstorageshort}} verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## SLCLI-Befehle im Zusammenhang mit der Replikation

* [SLCLI-Befehle im Zusammenhang mit der Replikation](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## SLCLI-Befehle im Zusammenhang mit Snapshots

* [Snapshots bestellen](ordering-/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#ordering-snapshot-space-through-the-slcli)
  ```
  slcli block snapshot-order
  ```

* [Snapshots verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
  ```
  slcli block snapshot-create
  slcli block snapshot-list
  slcli block snapshot-restore
  slcli block snapshot-cancel
  slcli block snapshot-delete
  slcli block snapshot-disable
  slcli block snapshot-enable
  ```

## SLCLI-Befehle im Zusammenhang mit Datenträgern

* [{{site.data.keyword.blockstorageshort}}-Datenträger bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI)
* [Duplikat eines Datenträgers erstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)
  ```
  slcli block volume-duplicate
  ```
* [IOPS anpassen](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS#steps)
  ```
  slcli block volume-modify
  ```
* [Kapazität erweitern](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity#steps)
  ```
  slcli block volume-modify
  ```
* [{{site.data.keyword.blockstorageshort}} verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block volume-cancel
  slcli block volume-count
  slcli block volume-detail
  slcli block volume-list
  slcli block volume-set-lun-id
  ```
* [Speichergrenzwerte verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)  
  ```
  slcli block volume-count
  ```
