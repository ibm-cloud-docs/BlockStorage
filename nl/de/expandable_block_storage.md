---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Erweiterbare Blockspeicherkapazität

Mit dieser neuen Funktion können die aktuellen {{site.data.keyword.blockstoragefull}}-Benutzer die Größe ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz spontan in GB-Schritten bis auf 12 TB erhöhen, ohne ein Duplikat erstellen oder Daten manuell auf einen größeren Datenträger migrieren zu müssen.  Während der Größenänderung gibt keinen keinerlei Ausfall oder Zugriffsbeschränkung. 

Die Abrechnung für den Datenträger wird automatisch so aktualisiert, dass die anteilige Differenz des neuen Preises zum aktuellen Abrechnungszyklus hinzugefügt und beim nächsten Abrechnungszyklus der gesamte neue Betrag abgerechnet wird.

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar. 

## Warum sollte erweiterbarer Speicher genutzt werden?

- **Kostenmanagement** – Sie wissen vielleicht, dass es Wachstumspotenzial für Ihre Daten gibt, benötigen aber am Anfang eine geringere Speicherkapazität. Dank der Möglichkeit zur Erweiterung können die Kunden Kosten für Speicherplatz sparen und später entsprechend Ihren Anforderungen wachsen.  

- **Wachsender Speicherbedarf** - Kunden, die starkes Wachstum erleben, brauchen ein Verfahren, mit dem Sie die Größe ihres Speichers schnell und einfach erhöhen können, um mit diesem Wachstum umzugehen.

## Wie wirkt sich die Erweiterung der Speicherkapazität auf die Replikation aus?

Eine Erweiterungsaktion für den primären Speicher bewirkt eine automatische Größenänderung des Replikats. 

## Gibt es Einschränkungen?

Diese Funktion ist nur für Speicher verfügbar, der in [Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) mit erweiterten Leistungsmerkmalen bereitgestellt wird. 

Speicher, der vor der Freigabe dieser Funktion (14. Dezember 2017) in diesen Rechenzentren für aktualisierten Speicher bereitgestellt wird, kann nur auf das 10-fache seiner Originalgröße vergrößert werden.  Der gesamte übrige Speicher, der nach diesem Datum bereitgestellt wird, kann bis zur maximalen Größe von 12 TB erhöht werden. 

Die bestehenden Größenbegrenzungen für mit Endurance bereitgestellten {{site.data.keyword.blockstorageshort}} gelten weiterhin (bis zu 4 TB für das 10-IOPS-Tier und bis zu 12 TB für alle anderen Tiers).

## Wie kann ich erkennen, ob mein bereitgestellter Speicher erweiterbar ist?

Speicher, der mit erweiterten Leistungsmerkmalen bereitgestellt wird, wird stets ruhend verschlüsselt.  Sie können leicht erkennen, dass Ihr Speicher infrage kommt, wenn in der Portalbenutzerschnittstelle ein 'Schlosssymbol' neben seinem Eintrag angezeigt wird. 

## Wie kann ich die Größe meines Speichers ändern?

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie in der Liste die LUN aus und klicken Sie auf **Aktionen** > **LUN ändern**.
3. Geben Sie die neue Speichergröße in GB ein.
4. Prüfen Sie Ihre Auswahl und die neue Preisstruktur.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen...** und klicken Sie auf **Auftrag erteilen**.
6. Ihre neue Speicherzuordnung sollte in wenigen Minuten verfügbar sein.
  
