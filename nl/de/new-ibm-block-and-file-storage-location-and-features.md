---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-10"

keywords: Block Storage, new features, new locations, Block Storage, mount point changes, select data centers, ISCSI,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Neue Standorte und Features
{: #news}

{{site.data.keyword.cloud}} führt eine neue Version von {{site.data.keyword.blockstoragefull}} ein!

Der neue Speicher ist in ausgewählten Rechenzentren verfügbar und wird durch Flashspeicher auf höherem IOPS-Niveau mit Verschlüsselung ruhender Daten auf Datenträgerebene gesichert. Der gesamte in den aktualisierten Rechenzentren bereitgestellte Speicher wird automatisch mit der neuen Version erstellt.

Der NFS-Mountpunkt für neue Datenträger unterscheidet sich vom Mountpunkt für nicht verschlüsselte Datenträger. Weitere Informationen finden Sie im Abschnitt [Neuer Mountpunkt für verschlüsselte {{site.data.keyword.blockstorageshort}}-Datenträger](#mountpoints).
{:important}

## Neue Standorte
{: #new-locations}

Der neue {{site.data.keyword.blockstorageshort}} ist in den folgenden Regionen bzw. Rechenzentren verfügbar.

|US 2|Lateinamerika|Kanada|EU|Asien/Pazifik|Australien|
|-----|-----|-----|-----|-----|------|
| DAL09<br >DAL10<br />DAL12<br />DAL13<br />SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07 | MEX01<br />SAO01 | MON01<br />TOR01  | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />MIL01<br />OSLO1<br />PAR01 | CHE01<br />HKG02<br />SEO01<br />SNG01<br />TOK02<br />TOK04<br />TOK05 | MEL01<br />SYD01<br />SYD04<br />SYD05 |
{: caption="In Tabelle 1 wird die Verfügbarkeit von Rechenzentren aufgeführt. Jede Region weist eine eigene Spalte auf. In manchen Städten, wie zum Beispiel Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London und Sydney, befinden sich mehrere Rechenzentren." caption-side="top"}


## Neue Funktionen und Leistungsmerkmale
{: #features}

- **[Anbietergesteuerte Verschlüsselung ruhender Daten](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)**.
  Alle {{site.data.keyword.blockstorageshort}}-Instanzen werden automatisch ohne Zusatzkosten verschlüsselt bereitgestellt.
- **Option für 10 IOPS/GB-Tier**.
  Für den Endurance-Typ von {{site.data.keyword.blockstorageshort}} ist ein neues Tier verfügbar, das die anspruchsvollsten Workloads unterstützt.
- **Gesamter durch Flashspeicher gestützter Speicher.**
  Gesamter {{site.data.keyword.blockstorageshort}}, der entweder mit dem Endurance- oder Performance-Typ bei 2 IOPS pro GB oder höher bereitgestellt wird, gestützt durch reinen Flashspeicher.
- **Snapshot- und Replikationsunterstützung** mit {{site.data.keyword.blockstorageshort}}
- **Option für stündliche Abrechnung**, verfügbar für Speicher, dessen Verwendung für weniger als einen kompletten Monat geplant ist.
- **Bis zu 48.000 IOPS** für {{site.data.keyword.blockstorageshort}}, der mit Performance bereitgestellt wird.
- **IOPS-Raten können angepasst werden**, um die Leistung bei saisonalen Lastschwankungen zu verbessern. Weitere Informationen zu dieser Funktion finden Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS).
- Erstellen Sie einen Klon Ihrer Daten mit dem **[{{site.data.keyword.blockstorageshort}}-Feature zur Datenträgerduplizierung](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)**.
- **Speichererweiterung** ist in GB-Schritten von bis zu 12 TB möglich, ohne ein Duplikat erstellen zu müssen oder Daten manuell auf einen größeren Datenträger verschieben zu müssen. Weitere Informationen zu dieser Funktion finden Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity).

## Neuer Mountpunkt für verschlüsselten Speicherdatenträger
{: #mountpoints}

Alle erweiterten Speicherdatenträger, die in diesen Rechenzentren bereitgestellt werden, verfügen über einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Überprüfen Sie die Informationen zum Mountpunkt auf der Seite mit den **Datenträger-Details** in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic/storage/block){: external}, um sicherzustellen, dass Sie den korrekten Mountpunkt verwenden. Sie können die richtigen Mountpunktinformationen auch über einen API-Aufruf abrufen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Um auf alle neuen Funktionen zugreifen zu können, wählen Sie `Storage-as-a-Service Package 759` aus, wenn Sie Ihre Bestellung über die API aufgeben. Weitere Informationen zur {{site.data.keyword.blockstorageshort}}-Bestellung über die API finden Sie unter [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
{:important}

Prüfen Sie hier später, ob für weitere Rechenzentren ein Upgrade durchgeführt wurde, und ob und neue Funktionen und Leistungsmerkmale für {{site.data.keyword.blockstorageshort}} hinzugefügt wurden.
{:tip}
