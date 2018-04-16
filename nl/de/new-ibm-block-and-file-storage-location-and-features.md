---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Neue Positionen und Funktionen von {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} führt eine neue Version von {{site.data.keyword.blockstoragefull}} ein! 

Der neue Speicher ist in ausgewählten Rechenzentren verfügbar und wird durch Flashspeicher auf höherem IOPS-Niveau mit Verschlüsselung ruhender Daten auf Datenträgerebene gesichert. Der gesamte in den ausgewählten Rechenzentren bereitgestellte Speicher wird automatisch mit der neuen Version von {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_full}} bereitgestellt.

**Hinweis** Der NFS-Mountpunkt für neue Datenträger hat sich geändert. Details dazu finden Sie unten im Abschnitt **Neuer Mountpunkt für verschlüsselte {{site.data.keyword.filestorage_short}}-Datenträger**.

Der neue {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}} ist derzeit in den folgenden Regionen/Datenzentren verfügbar, wobei bald weitere Datenzentren verfügbar werden!
<table style="width:100%;">
	<caption>Verfügbarkeit von Rechenzentren</caption>
	<tbody>
		<tr>
			<td><strong>US 2</strong></td>
			<td><strong>EU</strong></td>
			<td><strong>Australien</strong></td>
			<td><strong>Kanada</strong></td>
			<td><strong>Lateinamerika</strong></td>
			<td><strong>Asien/Pazifik</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOK02<br />
				HKG02<br />
			        SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


Der neue Speicher hat die folgenden Funktionen und Leistungsmerkmale:

- [Anbietergesteuerte Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html). Der gesamte {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}} wird automatisch ohne Aufpreis verschlüsselt bereitgestellt.
- Option für 10 IOPS/GB-Tier. Zum Endurance-Typ von {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}} wurde ein neues Tier hinzugefügt, um die anspruchsvollsten Workloads zu unterstützen.
- Gesamter durch Flashspeicher gestützter Speicher. {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}, bereitgestellt mit Endurance oder Performance bei 2 IOPS pro GB oder höher, gestützt durch Flashspeicher.
- Snapshot- und Replikationsunterstützung mit {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}, bereitgestellt mit Endurance oder Performance.
- Option für stündliche Abrechnung, hinzugefügt für Speicher, dessen Verwendung für weniger als einen kompletten Monat geplant ist. 
- Bis zu 48.000 IOPS für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}, bereitgestellt mit Performance.
- Die IOPS-Raten können angepasst werden, um die Leistung bei saisonalen Lastschwankungen zu verbessern. Weitere Informationen zu dieser Funktion finden Sie [hier](adjustable-iops.html).
- Erstellen Sie mit der [Funktion von {{site.data.keyword.blockstorageshort}} zur Datenträgerduplizierung](how-to-create-duplicate-volume.html) einen neuen Klon Ihrer Daten.
- Der Speicher kann spontan in GB-Schritten bis auf 12 TB erhöht werden, ohne ein Duplikat erstellen oder Daten manuell auf einen größeren Datenträger migrieren zu müssen. Weitere Informationen zu dieser Funktion finden Sie [hier](expandable_block_storage.html).

## Neuer Mountpunkt für verschlüsselte Storage-Datenträger

Alle in diesen Rechenzentren bereitgestellten verschlüsselten Speicherdatenträger haben einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie sowohl für Ihre verschlüsselten als auch für Ihre nicht verschlüsselten Speicherdatenträger den korrekten Mountpunkt verwenden, können Sie auf der Seite 'Datenträgerdetails' die Mountpunktinformationen in der Benutzerschnittstelle anzeigen sowie per API-Aufruf auf den korrekten Mountpunkt zugreifen:  `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Hier können Sie prüfen, wenn weitere Rechenzentren aktualisiert wurden und ob neue Funktionen und Leistungsmerkmale für {{site.data.keyword.blockstorageshort}} hinzugefügt wurden.
