---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# IOPS anpassen

Mit dieser neuen Funktion können die {{site.data.keyword.blockstoragefull}}-Speicherbenutzer die IOPS ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz spontan anpassen, ohne ein Duplikat erstellen oder Daten manuell in einen neuen Speicher migrieren zu müssen. Während der Anpassung erleben die Benutzer keinerlei Ausfall oder Zugriffsbeschränkung. 

Die Abrechnung für den Speicher wird so aktualisiert, dass die anteilige Differenz des neuen Preises zum aktuellen Abrechnungszyklus hinzugefügt und beim nächsten Abrechnungszyklus der gesamte neue Betrag abgerechnet wird.

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar. 

## Warum sollten IOPS genutzt werden?

- Kostenmanagement - einige unserer Kunden brauchen hohe IOPS möglicherweise nur in Zeiten hoher Auslastung. Ein großes Einzelhandelsgeschäft beispielsweise hat in den Ferien eine hohe Auslastung und braucht dann möglicherweise höhere IOPS für seinen Speicher als im Hochsommer. Dank dieser Funktion kann es seine Kosten steuern und zahlt für die höheren IOPS nur dann, wenn es sie wirklich braucht.

## Gibt es Einschränkungen?

Diese Funktion ist nur für Speicher verfügbar, der in [Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) mit erweiterten Leistungsmerkmalen bereitgestellt wird. 

Kunden können beim Anpassen ihrer IOPS nicht zwischen Endurance und Performance wechseln. Benutzer können auf Basis der folgenden Bedingungen/Einschränkungen ein neues IOPS-Tier oder eine neue IOPS-Ebene für ihren Speicher angeben: 

- Wenn der Originaldatenträger ein Endurance-Tier mit 0,25 IOPS/GB ist, kann das IOPS-Tier nicht aktualisiert werden.
- Wenn der Originaldatenträger ein Performance-Tier mit < 0,30 IOPS/GB ist, sollten nur Größen- und IOPS-Kombinationen verfügbar sein, deren Ergebnis < 0,30 IOPS/GB ist. 
- Wenn der Originaldatenträger ein Performance-Tier mit >= 0,30 IOPS/GB ist, sollten nur Größen- und IOPS-Kombinationen verfügbar sein, deren Ergebnis >= 0,30 IOPS/GB ist. Größe (größer-gleich dem Originaldatenträger)



## Wie wirkt sich die Anpassung der IOPS auf die Replikation aus?

Wenn die Replikation auf dem Datenträger eingerichtet wurde, wird das Replikat automatisch so aktualisiert, dass es mit der IOPS-Auswahl des Primärdatenträgers übereinstimmt. 

## Wie können die IOPS in meinem Speicher angepasst werden?

1. Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie in der Liste die LUN aus und klicken Sie auf **Aktionen** > **LUN ändern**.
3. Treffen Sie im Bereich **IOPS-Optionen für Speicher** eine neue Auswahl:
    - Endurance (Gestaffelte IOPS): Wählen Sie ein IOPS-Tier aus, das größer als 0,25 IOPS/GB Ihres Speichers ist. Sie können das IOPS-Tier jederzeit erhöhen. Seine Verringerung ist jedoch nur einmal pro Monat möglich.
    - Performance (Zugeordnete IOPS): Geben Sie für Ihren Speicher eine neue IOPS-Option an, indem Sie einen Wert zwischen 100 und 48.000 IOPS eingeben. (Achten Sie darauf, ob im Bestellformular bestimmte Grenzwerte aufgrund der Größe erforderlich sind.)
4. Prüfen Sie Ihre Auswahl und die neue Preisstruktur.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen...** und klicken Sie auf **Auftrag erteilen**.
6. Ihre neue Speicherzuordnung sollte in wenigen Minuten verfügbar sein.
