---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}

# IOPS anpassen

Mit dieser neuen Funktion können {{site.data.keyword.blockstoragefull}}-Speicherbenutzer die IOPS ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz sofort anpassen. Sie müssen nicht ein Duplikat erstellen oder Daten manuell in einen neuen Speicher migrieren. Während der Anpassung erleben die Benutzer keinerlei Ausfall oder Zugriffsbeschränkung. 

Die Abrechnung für den Speicher wird so aktualisiert, dass die anteilige Differenz des neuen Preises zum aktuellen Abrechnungszyklus hinzugefügt wird. Der gesamte neue Betrag wird beim nächsten Abrechnungszyklus abgerechnet.


## Vorteile konfigurierbarer IOPS

- Kostenmanagement - Einige Kunden benötigen ein hohes IOPS-Kontingent möglicherweise nur in Zeiten mit maximaler Nutzung. Ein großes Einzelhandelsgeschäft beispielsweise hat in den Ferien eine hohe Auslastung und braucht dann möglicherweise höhere IOPS-Raten für seinen Speicher. In der Mitte des Sommers sind die höheren IOPS-Raten dagegen nicht erforderlich. Dank dieser Funktion kann es seine Kosten steuern und zahlt für die höheren IOPS-Raten, wenn es sie braucht.

## Einschränkungen

Diese Funktion ist nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar.

Kunden können beim Anpassen ihrer IOPS nicht zwischen Endurance und Performance wechseln. Sie können jedoch auf Basis der folgenden Bedingungen/Einschränkungen ein neues IOPS-Tier oder eine neue IOPS-Ebene für ihren Speicher angeben: 

- Wenn der Originaldatenträger ein Endurance-Tier mit 0,25 IOPS/GB ist, kann das IOPS-Tier nicht aktualisiert werden.
- Wenn der Originaldatenträger ein Performance-Tier mit weniger als 0,30 IOPS/GB ist, schließen Sie nur Größen- und IOPS-Kombinationen ein, deren Ergebnis niedriger als 0,30 IOPS/GB ist. 
- Wenn der Originaldatenträger ein Performance-Tier mit größer-gleich 0,30 IOPS/GB ist, schließen Sie nur Größen- und IOPS-Kombinationen ein, deren Ergebnis größer-gleich 0,30 IOPS/GB ist. 

## Auswirkung der IOPS-Anpassung auf die Replikation

Wenn die Replikation auf dem Datenträger eingerichtet wurde, wird das Replikat automatisch so aktualisiert, dass es mit der IOPS-Auswahl des Primärdatenträgers übereinstimmt. 

## IOPS im Speicher anpassen

1. Navigieren Sie zur {{site.data.keyword.blockstorageshort}}-Liste:
   - Klicken Sie im {{site.data.keyword.slportal}} auf **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
   - Klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie in der Liste die LUN aus und klicken Sie auf **Aktionen** > **LUN ändern**.
3. Treffen Sie im Bereich **IOPS-Optionen für Speicher** eine neue Auswahl:
    - Endurance (Gestaffelte IOPS): Wählen Sie ein IOPS-Tier aus, das größer als 0,25 IOPS/GB Ihres Speichers ist. Sie können den Wert für das IOPS-Tier jederzeit erhöhen. Seine Verringerung ist jedoch nur einmal pro Monat möglich.
    - Performance (Zugeordnete IOPS): Geben Sie für Ihren Speicher eine neue IOPS-Option an, indem Sie einen Wert zwischen 100 und 48.000 IOPS eingeben. (Achten Sie darauf, ob im Bestellformular bestimmte Grenzwerte aufgrund der Größe erforderlich sind.)
4. Prüfen Sie Ihre Auswahl und die neue Preisstruktur.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Auftrag erteilen**.
6. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.
