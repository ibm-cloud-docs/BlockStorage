---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# {{site.data.keyword.blockstorageshort}} über die Konsole bestellen
{: #orderingthroughConsole}

Sie können {{site.data.keyword.blockstorageshort}} bereitstellen und entsprechend Ihrer Kapazität und Ihren IOPS-Anforderungen optimieren. Mit zwei Optionen zum Angeben der Leistung können Sie die Nutzung des Speichers optimieren.

- Sie können die Bereitstellung mit **Endurance**-Tiers durchführen, die vordefinierte Leistungsstufen für Workloads enthalten, die nicht über definierte Leistungsanforderungen verfügen.
- Zur Erfüllung bestimmter Leistungsanforderungen und zur Erstellung einer starken **Leistungsumgebung** können Sie Ihren Speicher optimieren, indem Sie die Gesamtzahl der IOPS (E/A-Operationen pro Sekunde) angeben.

## {{site.data.keyword.blockstorageshort}} mit vordefinierten IOPS-Tiers bestellen (Endurance)
{: #orderingthroughConsoleEndurance}

1. Melden Sie sich beim [{{site.data.keyword.cloud_notm}}-Katalog](https://{DomainName}/catalog){: external} an und klicken Sie auf **Speicher**. Wählen Sie anschließend **{{site.data.keyword.blockstorageshort}}** aus und klicken Sie auf **Erstellen**.
2. Wählen Sie Ihre Bereitstellungs**position** (Rechenzentrum) aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position des vorhandenen Rechenhosts bzw. der vorhandenen Rechenhosts hinzugefügt wird.
3. Abrechnung. Wenn Sie ein Rechenzentrum mit verbesserten Leistungsmerkmalen (mit einem Stern gekennzeichnet) ausgewählt haben, haben Sie die Auswahl zwischen monatlicher und stündlicher Abrechnung.
     1. Bei **stündlicher** Abrechnung wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus. Dies hängt davon ab, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist für Speicher verfügbar, der in diesen [Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) bereitgestellt wird.
     2. Bei **monatlicher** Abrechnung wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine Block-LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und verfügbar sein müssen (einen Monat oder länger).

        Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Funktionen aktualisiert werden.
        {:important}
4. Geben Sie die Speichergröße in das Feld **Neue Speichergröße** ein.
5. Wählen Sie **Endurance (Gestaffelte IOPS)** im Abschnitt **IOPS-Optionen für Speicher** aus.
6. Wählen Sie das für die Plattform erforderliche IOPS-Tier aus.
    - **0,25 IOPS pro GB** ist für Workloads mit niedriger Ein-/Ausgabeintensität gedacht. Bei diesen Workloads ist in der Regel zu einem Zeitpunkt ein großer Prozentsatz der Daten inaktiv. Beispielanwendungen sind Speichermailboxen oder gemeinsam genutzte Dateiressourcen auf Abteilungsebene.
    - **2 IOPS pro GB** ist für die meisten allgemeinen Verwendungszwecke gedacht. Beispielanwendungen sind Hostings kleiner Datenbanken, Sicherungen von Webanwendungen oder Plattenimages virtueller Maschinen für einen Hypervisor.
    - **4 IOPS pro GB** ist für Workloads mit höherer Intensität gedacht. Bei diesen Workloads ist in der Regel zu einem Zeitpunkt ein hoher Prozentsatz der Daten aktiv. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
    - **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse. Dieses Tier ist in den [meisten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) für Speicher bis zu 4 TB verfügbar.
7. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie die Snapshotgröße aus der Liste aus. Dieser Speicherplatz wird zusätzlich zum verwendbaren Speicherplatz hinzugefügt. Überlegungen und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Wählen Sie in der Liste Ihren **Betriebssystemtyp** aus.<br/>

   Diese Auswahl basiert auf dem Betriebssystem, auf dem Ihr Host ausgeführt wird, und kann später nicht mehr geändert werden. Läuft Ihr Server zum Beispiel unter Ubuntu oder RHEL, so wählen Sie Linux aus. Wenn Ihr Host ein Windows 2012 oder Windows 2016 Server ist, wählen Sie die Option 'Windows 2008+' aus der Liste aus. Weitere Informationen zu verschiedenen Windows-Optionen finden Sie im Abschnitt mit [FAQs](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).
   {:tip}
9. Überprüfen Sie auf der rechten Seite Ihre Bestellübersicht und wenden Sie gegebenenfalls Ihren Werbeaktionscode an.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
10. Danach markieren Sie das Kontrollkästchen **Die im Folgenden aufgeführten Servicevereinbarungen anderer Anbieter habe ich gelesen und stimme ihnen zu:**.
11. Klicken Sie auf **Erstellen**. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQ)](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}

## {{site.data.keyword.blockstorageshort}} mit angepassten IOPS-Raten bestellen (Performance)
{: #orderingthroughConsolePerformance}

1. Melden Sie sich beim [{{site.data.keyword.cloud_notm}}-Katalog](https://{DomainName}/catalog){: external} an und klicken Sie auf **Speicher**. Wählen Sie anschließend {{site.data.keyword.blockstorageshort}} aus und klicken Sie auf **Erstellen**.
2. Klicken Sie auf die Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position des vorhandenen Rechenhosts bzw. der vorhandenen Rechenhosts hinzugefügt wird.
3. Abrechnung. Wenn Sie ein Rechenzentrum mit verbesserten Leistungsmerkmalen (mit einem Stern gekennzeichnet) ausgewählt haben, haben Sie die Auswahl zwischen monatlicher und stündlicher Abrechnung.
     1. Bei **stündlicher** Abrechnung wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus. Dies hängt davon ab, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist für Speicher verfügbar, der in diesen [Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) bereitgestellt wird.
     2. Bei **monatlicher** Abrechnung wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine Block-LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und verfügbar sein müssen (einen Monat oder länger).

        Der monatliche Abrechnungstyp wird standardmäßig für Speicher verwendet, der in Rechenzentren bereitgestellt wird, die **nicht** mit verbesserten Funktionen aktualisiert werden.
        {:note}
4. Geben Sie die Speichergröße in das Feld **Neue Speichergröße** ein.
5. Wählen Sie **Performance (Zugeordnete IOPS)** im Abschnitt **IOPS-Optionen für Speicher** aus.
6. Geben Sie im Feld **Performance (Zugeordnete IOPS)** die IOPS ein.
7. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie die Snapshotgröße aus der Liste aus. Dieser Speicherplatz wird zusätzlich zum verwendbaren Speicherplatz hinzugefügt. Überlegungen und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Wählen Sie in der Liste Ihren **Betriebssystemtyp** aus.<br/>

   Diese Auswahl basiert auf dem Betriebssystem, auf dem Ihr Host ausgeführt wird, und kann später nicht mehr geändert werden. Läuft Ihr Server zum Beispiel unter Ubuntu oder RHEL, so wählen Sie Linux aus. Wenn Ihr Host ein Windows 2012 oder Windows 2016 Server ist, wählen Sie die Option 'Windows 2008+' aus der Liste aus. Weitere Informationen zu verschiedenen Windows-Optionen finden Sie im Abschnitt mit [FAQs](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).
   {:tip}
9. Überprüfen Sie auf der rechten Seite Ihre Bestellübersicht und wenden Sie gegebenenfalls Ihren Werbeaktionscode an.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
10. Danach markieren Sie das Kontrollkästchen **Die im Folgenden aufgeführten Servicevereinbarungen anderer Anbieter habe ich gelesen und stimme ihnen zu:**.
11. Klicken Sie auf **Erstellen**. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen von Grenzwerten finden Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQ)](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}

## Neuen Speicher verbinden
{: #mountingnewLUN}

Wenn die Bereitstellungsanforderung abgeschlossen ist, autorisieren Sie die Hosts, auf den neuen Speicher zuzugreifen und die Verbindung zu konfigurieren. Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verbindung zu LUNs unter CloudLinux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Verbindung zu LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [LUN in XenServer Shared Storage anhängen](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Hinweise zur Disaster-Recovery

Um Datenverluste zu vermeiden und unterbrechungsfreie Geschäftsabläufe zu gewährleisten, sollten Sie in Betracht ziehen, Ihre Server und Speicher in einem anderen Rechenzentrum zu replizieren. Die Replikation hält Ihre Daten an zwei verschiedenen Positionen je nach Snapshot-Zeitplan synchron. Weitere Informationen hierzu finden Sie im Abschnitt [Daten replizieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).

Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie den Abschnitt [Duplikat des Blockdatenträgers erstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## {{site.data.keyword.blockstorageshort}} auf der Rechnung identifizieren

Alle LUNs werden auf Ihrer Rechnung als eine Artikelposition angegeben. Endurance wird als 'Endurance-Speicherservice' und Performance als 'Performance-Speicherservice' angezeigt; die Rate variiert je nach Ihrer Speicherebene. Sie können Endurance oder Performance erweitern, um anzuzeigen, dass es sich um {{site.data.keyword.blockstorageshort}} handelt.
