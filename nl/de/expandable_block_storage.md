---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Blockspeicherkapazität erweitern
{: #expandingcapacity}

Mit dieser Funktion können aktuelle {{site.data.keyword.blockstoragefull}}-Benutzer die Größe ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz in GB sofort in Schritten von bis zu 12 TB anpassen. Sie müssen nicht ein Duplikat erstellen oder Daten manuell auf einen größeren Datenträger migrieren. Während der Größenänderung kommt es nicht zu einem Ausfall oder einer Zugriffsbeschränkung.

Die Abrechnung für den Datenträger wird so aktualisiert, dass die anteilige Differenz des neuen Preises zum aktuellen Abrechnungszyklus hinzugefügt wird. Der gesamte neue Betrag wird dann beim nächsten Abrechnungszyklus abgerechnet.

Diese Funktion ist in den [meisten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) verfügbar.

## Vorteile des erweiterbaren Speichers

- **Kostenmanagement** - Sie können erkennen, dass es ein Potenzial für eine Zunahme Ihrer Daten gibt, aber Sie benötigen eine kleinere Speichermenge, um zu beginnen. Die Möglichkeit zur Erweiterung ermöglicht es den Kunden, Kosten für den Speicher zu sparen und ihn zu einem späteren Zeitpunkt zur Anpassung an die jeweiligen Anforderungen zu vergrößern.  

- **Steigender Speicherbedarf** - Kunden mit einem hohen Datenzuwachs benötigen eine Möglichkeit, die Größe Ihres Speichers für die Verwaltung schnell und problemlos zu erhöhen.

## Auswirkungen einer Erweiterung der Speicherkapazität auf die Replikation

Eine Erweiterungsaktion des primären Speichers hat eine automatische Größenänderung des Replikats zur Folge.

## Einschränkungen
{: #limitsofexpandingstorage}

Diese Funktion ist für Speicher verfügbar, der in den [meisten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) bereitgestellt wird.

Speicher, der vor der Freigabe dieser Funktion (**April 2017 - 14. Dezember 2017**) in diesen Rechenzentren bereitgestellt wird, kann maximal auf das 10-fache seiner Originalgröße vergrößert werden. Speicher, der nach dem **14. Dezember 2017** bereitgestellt wird, kann bis zur maximalen Größe von 12 TB erhöht werden.

Die bestehenden Größenbegrenzungen für mit Endurance bereitgestellten {{site.data.keyword.blockstorageshort}} gelten weiterhin (bis zu 4 TB für das 10-IOPS-Tier und bis zu 12 TB für alle anderen Tiers).

## Größe des Speichers ändern
{: #resizingsteps}

1. Klicken Sie in der {{site.data.keyword.cloud}}-Konsole auf das **Menüsymbol**. Klicken Sie anschließend auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie den iSCSI-Datenträger in der Liste aus und klicken Sie auf **...** > **LUN ändern**. 
3. Geben Sie die neue Speichergröße in GB ein.
4. Prüfen Sie Ihre Auswahl und die neue Preisstruktur.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung aufgeben**.
6. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.

Alternativ dazu könne Sie die Größe des Datenträgers auch über die SL-CLI ändern.

```
# slcli block volume-modify --help
Syntax: slcli block volume-modify [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -c, --new-size INTEGER        Neue Größe des Blockspeicherdatenträgers in GB. ***Wird keine Größe angegeben, wird die ursprüngliche
                                Größe des Datenträgers verwendet.***
                                Mögliche Größen: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [ursprüngliche Größe des Datenträgers]
  -i, --new-iops INTEGER        Performance-Speicher-IOPS, zwischen 100 und 6000
                                in Vielfachen von 100 [nur für Performance-
                                Datenträger] ***Wird kein IOPS-Wert angegeben,
                                wird der ursprüngliche IOPS-Wert des Datenträgers
                                verwendet.***
                                Voraussetzungen: [Wenn der ursprüngliche IOPS/GB-
                                Wert für den Datenträger geringer als 0,3 ist, muss
                                der neue IOPS/GB-Wert ebenfalls geringer als 0,3 sein. Wenn der ursprüngliche IOPS/GB-Wert für den
                                Datenträger größer-gleich 0,3 ist, muss der neue
                                IOPS/GB-Wert für den Datenträger ebenfalls
                                größer-gleich 0,3 sein.]
  -t, --new-tier [0.25|2|4|10]  Endurance-Speichertier (IOPS pro GB) [nur
                                für Endurance-Datenträger] ***Wird kein Tier
                                angegeben, wird das ursprüngliche Tier des
                                Datenträgers verwendet.***
                                Voraussetzungen: [Wenn der ursprüngliche IOPS/GB-
                                Wert für den Datenträger 0,25 ist, muss der neue
                                IOPS/GB-Wert für Datenträger ebenfalls 0,25 sein. Wenn der ursprüngliche IOPS/GB-Wert für den
                                Datenträger größer als 0,25 ist, muss der neue
                                IOPS/GB-Wert für den Datenträger ebenfalls
                                größer als 0,25 sein. ]
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

Weitere Informationen zur Erweiterung des Dateisystems (und gegebenenfalls der Partitionen) auf dem Datenträger zur Nutzung des neuen Speicherplatzes  finden Sie in der Dokumentation des Betriebssystems.
{:tip}
