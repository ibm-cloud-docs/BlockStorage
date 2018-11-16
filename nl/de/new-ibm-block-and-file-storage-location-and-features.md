---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Neue Positionen und Funktionen von {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.BluSoftlayer_full}} führt eine neue Version von {{site.data.keyword.blockstoragefull}} ein!

Der neue Speicher ist in ausgewählten Rechenzentren verfügbar und wird durch Flashspeicher auf höherem IOPS-Niveau mit Verschlüsselung ruhender Daten auf Datenträgerebene gesichert. Der gesamte in den aktualisierten Rechenzentren bereitgestellte Speicher wird automatisch mit der neuen Version erstellt.

Der NFS-Mountpunkt für neue Datenträger unterscheidet sich vom Mountpunkt für nicht verschlüsselte Datenträger. Weitere Informationen finden Sie im Abschnitt [Neuer Mountpunkt für verschlüsselte {{site.data.keyword.filestorage_short}}-Datenträger](#new-mount-point-for-encrypted-storage-volumes).
{:important}

## Neue Standorte

Der neue {{site.data.keyword.blockstorageshort}} ist in den folgenden Regionen bzw. Rechenzentren verfügbar.
<table role="presentation">
  <tr>
    <td><strong>US 2</strong></td>
    <td><strong>EU</strong></td>
    <td><strong>Australien</strong></td>
    <td><strong>Kanada</strong></td>
    <td><strong>Lateinamerika</strong></td>
    <td><strong>Asien/Pazifik</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*In Tabelle 1 wird die Verfügbarkeit von Rechenzentren aufgeführt. Jede Region weist eine eigene Spalte auf. In manchen Städten, wie zum Beispiel Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London und Sydney, befinden sich mehrere Rechenzentren.*

## Neue Funktionen und Leistungsmerkmale

- **[Anbietergesteuerte Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html)**.
  Alle {{site.data.keyword.blockstorageshort}}-Instanzen werden automatisch ohne Zusatzkosten verschlüsselt bereitgestellt.
- **Option für 10 IOPS/GB-Tier**.
  Für den Endurance-Typ von {{site.data.keyword.blockstorageshort}} ist ein neues Tier verfügbar, das die anspruchsvollsten Workloads unterstützt.
- **Gesamter durch Flashspeicher gestützter Speicher.**
  Gesamter {{site.data.keyword.blockstorageshort}}, der entweder mit dem Endurance- oder Performance-Typ bei 2 IOPS pro GB oder höher bereitgestellt wird, gestützt durch reinen Flashspeicher.
- **Snapshot- und Replikationsunterstützung** mit {{site.data.keyword.blockstorageshort}}
- **Option für stündliche Abrechnung**, verfügbar für Speicher, dessen Verwendung für weniger als einen kompletten Monat geplant ist.
- **Bis zu 48.000 IOPS** für {{site.data.keyword.blockstorageshort}}, der mit Performance bereitgestellt wird.
- **IOPS-Raten können angepasst werden**, um die Leistung bei saisonalen Lastschwankungen zu verbessern. Weitere Informationen zu dieser Funktion finden Sie [hier](adjustable-iops.html).
- Erstellen Sie mit der **[Funktion von {{site.data.keyword.blockstorageshort}} zur Datenträgerduplizierung](how-to-create-duplicate-volume.html)** einen Klon Ihrer Daten.
- **Speichererweiterung** ist in GB-Schritten von bis zu 12 TB möglich, ohne ein Duplikat erstellen zu müssen oder Daten manuell auf einen größeren Datenträger verschieben zu müssen. Weitere Informationen zu dieser Funktion finden Sie [hier](expandable_block_storage.html).

## Neuer Mountpunkt für verschlüsselten Speicherdatenträger

Alle erweiterten Speicherdatenträger, die in diesen Rechenzentren bereitgestellt werden, verfügen über einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Wenn Sie sicherstellen möchten, dass Sie den korrekten Mountpunkt für die Speicherdatenträger verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} anzeigen. Sie können auf den korrekten Mountpunkt auch über einen API-Aufruf zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Prüfen Sie hier später, ob für weitere Rechenzentren ein Upgrade durchgeführt wurde, und ob und neue Funktionen und Leistungsmerkmale für {{site.data.keyword.blockstorageshort}} hinzugefügt wurden.
{:tip}
