---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} bestellen

Sie können {{site.data.keyword.blockstorageshort}}-Speicher bereitstellen und so optimieren, dass Ihre Anforderungen an Kapazität und IOPS-Bedarf erfüllt werden. Mit zwei Optionen zum Angeben der Leistung können Sie die Nutzung des Speichers optimieren.

- Sie können Endurance-IOPS-Tiers auswählen, von denen die vordefinierten Leistungsstufen für die Anpassung an die Workloads bereitgestellt werden, die nicht über gut definierte Leistungsanforderungen verfügen. 
- Sie können den Speicher so optimieren, dass bestimmte Speicheranforderungen erfüllt werden; hierfür geben Sie die Gesamtanzahl der IOPS mit Performance an.

## {{site.data.keyword.blockstorageshort}} mit vordefinierten IOPS-Tiers bestellen (Endurance)

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur > Speicher > {{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie rechts oben auf **{{site.data.keyword.blockstorageshort}} bestellen**.
3. Wählen Sie Ihre Bereitstellungs**position** (Rechenzentrum) aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position des vorhandenen Rechenhosts bzw. der vorhandenen Rechenhosts hinzugefügt wird.
4. Abrechnung. Wenn Sie ein Rechenzentrum mit verbesserten Leistungsmerkmalen (mit einem Stern gekennzeichnet) ausgewählt haben, haben Sie die Auswahl zwischen monatlicher und stündlicher Abrechnung. 
     1. Bei **stündlicher** Abrechnung wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus. Dies hängt davon ab, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird. 
     2. Bei **monatlicher** Abrechnung wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine Block-LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und verfügbar sein müssen (einen Monat oder länger).>**HINWEIS:** Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Leistungsmerkmalen aktualisiert werden.
5. Geben Sie die Speichergröße in das Feld **Neue Speichergröße** ein.
6. Wählen Sie **Endurance (Gestaffelte IOPS)** im Abschnitt **IOPS-Optionen für Speicher** aus.
7. Wählen Sie das für die Plattform erforderliche IOPS-Tier aus.
    - **0,25 IOPS pro GB** ist für Workloads mit niedriger Ein-/Ausgabeintensität gedacht. Bei diesen Workloads ist in der Regel zu einem Zeitpunkt ein großer Prozentsatz der Daten inaktiv. Beispielanwendungen sind Speichermailboxen oder Dateifreigaben auf Abteilungsebene.
    - **2 IOPS pro GB** ist für die meisten allgemeinen Verwendungszwecke gedacht. Beispielanwendungen sind Hostings kleiner Datenbanken, Sicherungen von Webanwendungen oder Plattenimages virtueller Maschinen für einen Hypervisor.
    - **4 IOPS pro GB** ist für Workloads mit höherer Intensität gedacht. Bei diesen Workloads ist in der Regel zu einem Zeitpunkt ein hoher Prozentsatz der Daten aktiv. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse. Dieses Tier ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) für Speicher bis zu 4 TB verfügbar.
8. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie die Snapshotgröße aus der Liste aus. Dieser Speicherplatz wird zusätzlich zum verwendbaren Speicherplatz hinzugefügt. Überlegungen und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](ordering-snapshots.html).
9. Wählen Sie in der Liste Ihren **Betriebssystemtyp** aus.
10. Wählen Sie die Kontrollkästchen für **Bedingungen** aus und klicken Sie auf **Bestellung aufgeben**.
11. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.

>**Hinweis:** Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](managing-storage-limits.html).<br/><br/>Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](BlockStorageFAQ.html).
 
## {{site.data.keyword.blockstorageshort}} mit angepassten IOPS-Raten bestellen (Performance)

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** und danach auf **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur > Speicher > {{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie rechts oben auf **{{site.data.keyword.blockstorageshort}} bestellen**.
3. Klicken Sie auf die Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position des vorhandenen Rechenhosts bzw. der vorhandenen Rechenhosts hinzugefügt wird.
4. Abrechnung. Wenn Sie ein Rechenzentrum mit verbesserten Leistungsmerkmalen (mit einem Stern gekennzeichnet) ausgewählt haben, haben Sie die Auswahl zwischen monatlicher und stündlicher Abrechnung.
     1. Bei **stündlicher** Abrechnung wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus. Dies hängt davon ab, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird. 
     2. Bei **monatlicher** Abrechnung wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine Block-LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und verfügbar sein müssen (einen Monat oder länger).>**HINWEIS:** Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Leistungsmerkmalen aktualisiert werden.
5. Geben Sie die Speichergröße in das Feld **Neue Speichergröße** ein.
6. Wählen Sie **Performance (Zugeordnete IOPS)** im Abschnitt **IOPS-Optionen für Speicher** aus.
7. Geben Sie im Feld **Performance (Zugeordnete IOPS)** die IOPS ein.
8. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie die Snapshotgröße aus der Liste aus. Dieser Speicherplatz wird zusätzlich zum verwendbaren Speicherplatz hinzugefügt. Überlegungen und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](ordering-snapshots.html).
9. Wählen Sie in der Liste Ihren **Betriebssystemtyp** aus.
10. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.

>**Hinweis:** Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](managing-storage-limits.html).<br/><br/>Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](BlockStorageFAQ.html).

## Neuen Speicher verbinden

Wenn die Bereitstellungsanforderung abgeschlossen ist, autorisieren Sie die Hosts, auf den neuen Speicher zuzugreifen und die Verbindung zu konfigurieren. Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html)
- [Verbindung zu MPIO-iSCSI-LUNS unter Microsoft Windows herstellen](accessing-block-storage-windows.html)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](configure-backup-cpanel.html)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](configure-backup-plesk.html)

## {{site.data.keyword.blockstorageshort}} auf der Rechnung identifizieren

Alle LUNs werden auf Ihrer Rechnung als eine Artikelposition angegeben. Endurance wird als 'Endurance-Speicherservice' und Performance als 'Performance-Speicherservice' angezeigt; die Rate variiert je nach Ihrer Speicherebene. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um {{site.data.keyword.blockstorageshort}} handelt.
