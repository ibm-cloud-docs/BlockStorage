---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-20"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Blockspeicherkapazität erweitern

Mit dieser neuen Funktion können aktuelle {{site.data.keyword.blockstoragefull}}-Benutzer die Größe ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz sofort in Schritten von bis zu 12 GB anpassen. Sie müssen nicht ein Duplikat erstellen oder Daten manuell auf einen größeren Datenträger migrieren. Während der Größenänderung kommt es nicht zu einem Ausfall oder einer Zugriffsbeschränkung.

Die Abrechnung für den Datenträger wird so aktualisiert, dass die anteilige Differenz des neuen Preises zum aktuellen Abrechnungszyklus hinzugefügt wird. Der gesamte neue Betrag wird dann beim nächsten Abrechnungszyklus abgerechnet.

Diese Funktion ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar.

## Vorteile des erweiterbaren Speichers

- **Kostenmanagement** - Sie können erkennen, dass es ein Potenzial für eine Zunahme Ihrer Daten gibt, aber Sie benötigen eine kleinere Speichermenge, um zu beginnen. Die Möglichkeit zur Erweiterung ermöglicht es den Kunden, Kosten für den Speicher zu sparen und ihn anschließend zu erhöhen, um ihn an ihre Anforderungen anzupassen.  

- **Steigender Speicherbedarf** - Kunden mit einem hohen Datenzuwachs benötigen eine Möglichkeit, die Größe Ihres Speichers für die Verwaltung schnell und problemlos zu erhöhen.

## Auswirkungen einer Erweiterung der Speicherkapazität auf die Replikation

Eine Erweiterungsaktion des primären Speichers hat eine automatische Größenänderung des Replikats zur Folge.

## Einschränkungen

Diese Funktion ist für Speicher verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird.

Speicher, der vor der Freigabe dieser Funktion (**April 2017 - 14. Dezember 2017**) in diesen Rechenzentren bereitgestellt wird, kann maximal auf das 10-fache seiner Originalgröße vergrößert werden. Speicher, der nach dem **14. Dezember 2017** bereitgestellt wird, kann bis zur maximalen Größe von 12 TB erhöht werden.

Die bestehenden Größenbegrenzungen für mit Endurance bereitgestellten {{site.data.keyword.blockstorageshort}} gelten weiterhin (bis zu 4 TB für das 10-IOPS-Tier und bis zu 12 TB für alle anderen Tiers).

## Größe des Speichers ändern

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie in der Liste die LUN aus und klicken Sie auf **Aktionen** > **LUN ändern**.
3. Geben Sie die neue Speichergröße in GB ein.
4. Prüfen Sie Ihre Auswahl und die neue Preisstruktur.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung aufgeben**.
6. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.

Weitere Informationen zur Erweiterung des Dateisystems (und gegebenenfalls der Partitionen) auf dem Datenträger zur Nutzung des neuen Speicherplatzes  finden Sie in der Dokumentation des Betriebssystems.
{:tip}
