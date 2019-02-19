---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# SL-CLI-Befehle für {{site.data.keyword.blockstorageshort}}
{: #SLCLIcommands}

Sie können die SL-CLI verwenden, um Aktionen, wie z. B. das Bestellen neuer Datenträger, eines Snapshotbereichs oder einer Replikation, das Aktualisieren von Berechtigungen, das Stornieren von Datenträgern usw., auszuführen, die normalerweise über das [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} vorgenommen werden. 

Weitere Informationen zur Installation und Verwendung der SL-CLI finden Sie unter [Python-API-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## SL-CLI-Befehl im Zusammenhang mit der Zugriffssteuerung
* [{{site.data.keyword.blockstorageshort}} verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)  
  ```
  slcli block access-authorize
  slcli block access-list
  slcli block access-password
  slcli block access-revoke
  ```

## SL-CLI-Befehle im Zusammenhang mit der Replikation

* [SL-CLI-Befehle im Zusammenhang mit der Replikation](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#clicommands)
  ```
  slcli block access-revoke
  slcli block replica-failback
  slcli block replica-failover
  slcli block replica-locations
  slcli block replica-order
  slcli block replica-partners
  ```

## SL-CLI-Befehle im Zusammenhang mit Snapshots

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

## SL-CLI-Befehle im Zusammenhang mit Datenträgern

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
