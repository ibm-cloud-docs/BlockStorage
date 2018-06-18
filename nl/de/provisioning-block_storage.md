---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} bestellen

Abhängig von Ihren Anforderungen und Vorgaben gibt es zwei unterschiedliche Arten zur Bereitstellung von {{site.data.keyword.blockstorageshort}}. Hierbei handelt es sich um die beiden folgenden Möglichkeiten: 

- **Endurance**: Stellen Sie Endurance-Tiers mit vordefinierten Leistungsstufen und Funktionen wie Snapshots und Replikation bereit. 
- **Performance**: Erstellen Sie eine leistungsfähige Performance-Umgebung, in der Sie die gewünschten E/A-Operationen pro Sekunde (IOPS) zuordnen können.

## Endurance für {{site.data.keyword.blockstorageshort}} bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur > Speicher > {{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.blockstorageshort}} bestellen**.
3. Wählen Sie **Endurance** in der Liste **Speichertyp auswählen** aus.
4. Wählen Sie Ihre Bereitstellungs**position** (Rechenzentrum) aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie die früher bestellten Rechenhosts hinzugefügt wird.
   - Wenn Sie ein Rechenzentrum mit verbesserten Leistungsmerkmalen (mit einem Stern gekennzeichnet) ausgewählt haben, haben Sie die Auswahl zwischen monatlicher und stündlicher Abrechnung. 
     1. Bei **stündlicher** Abrechnung erfolgt die Abrechnung der Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus, je nachdem, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird. 
     2. Bei **monatlicher** Abrechnung wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine Block-LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und im Zugriff sein müssen (einen Monat oder länger).
     **HINWEIS**: Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Leistungsmerkmalen aktualisiert werden.
5. Wählen Sie das für Ihre Plattform erforderliche Tier aus.
    - **0,25 IOPS pro GB** ist für Workloads mit niedriger Ein-/Ausgabeintensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein großer Prozentsatz der Daten inaktiv. Beispielanwendungen sind Speichermailboxen oder Dateifreigaben auf Abteilungsebene.
    - **2 IOPS pro GB** ist für die meisten allgemeinen Verwendungszwecke gedacht. Beispielanwendungen sind Hostings kleiner Datenbanken, Sicherungen von Webanwendungen oder Plattenimages virtueller Maschinen für einen Hypervisor.
    - **4 IOPS pro GB** ist für Workloads mit höherer Intensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein hoher Prozentsatz der Daten aktiv. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse. Dieses Tier ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) für Speicher bis zu 4 TB verfügbar.
6. Klicken Sie auf *Speichergröße auswählen** und wählen Sie die Speichergröße aus der Liste aus.
7. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie die Snapshotgröße aus der Liste aus. Dieser Speicherplatz wird zusätzlich zu Ihrem verwendbaren Speicherplatz hinzugefügt. Überlegungen und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](ordering-snapshots.html).
8. Wählen Sie in der Liste Ihren **Betriebssystemtyp** aus.
9. Klicken Sie auf **Weiter**. Ihnen werden die monatlichen und die anteiligen Gebühren angezeigt, sodass Sie die Details der Bestellung prüfen können.
10. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Auftrag erteilen**.
11. Ihre neue Speicherzuordnung sollte in wenigen Minuten verfügbar sein.

**Hinweis**: Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen wollen. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](managing-storage-limits.html).

Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](BlockStorageFAQ.html).
 
## Performance für Block Storage bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** und danach auf **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur > Speicher > {{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.blockstorageshort}} bestellen**.
3. Wählen Sie in der Dropdown-Liste **Speichertyp auswählen** den Eintrag **Performance** aus.
4. Klicken Sie auf die Dropdown-Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie die früher bestellten Hosts hinzugefügt wird.
   - Wenn Sie ein Rechenzentrum mit verbesserten Leistungsmerkmalen (in der Dropdown-Liste mit * gekennzeichnet) ausgewählt haben, haben Sie die Auswahl zwischen monatlicher und stündlicher Abrechnung. 
     1. Bei **stündlicher** Abrechnung erfolgt die Abrechnung der Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus, je nachdem, was zuerst eintritt.  Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in diesen [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird. 
     2. Bei **monatlicher** Abrechnung wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine Block-LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und im Zugriff sein müssen (einen Monat oder länger).
     **HINWEIS**: Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Leistungsmerkmalen aktualisiert werden.
5. Wählen Sie das Optionsfeld neben der entsprechenden **Speichergröße** aus.
6. Geben Sie im Feld **IOPS angeben** die IOPS ein.
7. Klicken Sie auf **Weiter**. Ihnen werden die monatlichen und die anteiligen Gebühren angezeigt, sodass Sie die Details der Bestellung prüfen können. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern wollen.
8. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf die Schaltfläche **'Auftrag erteilen'.
9. Ihre neue Speicherzuordnung sollte in wenigen Minuten verfügbar sein.

**Hinweis**: Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen wollen. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](managing-storage-limits.html).

Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](BlockStorageFAQ.html).

## Meinen {{site.data.keyword.blockstorageshort}} auf meiner Rechnung ermitteln

Alle LUNs werden auf Ihrer Rechnung als Artikelposition angezeigt. Endurance wird als 'Endurance-Speicherservice' und Performance als 'Performance-Speicherservice' angezeigt. Die Rate variiert je nach Ihrer Speicherebene. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um {{site.data.keyword.blockstorageshort}} handelt.
