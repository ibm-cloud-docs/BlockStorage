---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-22"

keywords: Block storage, new feature, adjusting IOPS, modify IOPS, increase IOPS, decrease IOPS,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# IOPS anpassen
{: #adjustingIOPS}

Mit dieser Funktion können {{site.data.keyword.blockstoragefull}}-Speicherbenutzer die IOPS ihrer vorhandenen {{site.data.keyword.blockstorageshort}}-Instanz sofort anpassen. Sie müssen nicht ein Duplikat erstellen oder Daten manuell in einen neuen Speicher migrieren. Während der Anpassung erleben die Benutzer keinerlei Ausfall oder Zugriffsbeschränkung.

Die Abrechnung für den Speicher wird so aktualisiert, dass die anteilige Differenz des neuen Preises zum aktuellen Abrechnungszyklus hinzugefügt wird. Der gesamte neue Betrag wird beim nächsten Abrechnungszyklus abgerechnet.


## Vorteile konfigurierbarer IOPS

- Kostenmanagement - Einige Kunden benötigen ein hohes IOPS-Kontingent möglicherweise nur in Zeiten mit maximaler Nutzung. Ein großes Einzelhandelsgeschäft beispielsweise hat in den Ferien eine hohe Auslastung und braucht dann möglicherweise höhere IOPS-Raten für seinen Speicher. In der Mitte des Sommers sind die höheren IOPS-Raten dagegen nicht erforderlich. Dank dieser Funktion kann es seine Kosten steuern und zahlt für die höheren IOPS-Raten, wenn es sie braucht.

## Einschränkungen
{: #limitsofIOPSadjustment}

Diese Funktion ist in den [meisten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) verfügbar.

Kunden können beim Anpassen ihrer IOPS nicht zwischen Endurance und Performance wechseln. Sie können jedoch auf Basis der folgenden Bedingungen/Einschränkungen ein neues IOPS-Tier oder eine neue IOPS-Ebene für ihren Speicher angeben:

- Wenn der Originaldatenträger ein Endurance-Tier mit 0,25 IOPS/GB ist, kann das IOPS-Tier nicht aktualisiert werden.
- Wenn der Originaldatenträger ein Performance-Tier mit kleiner-gleich 0,30 IOPS/GB ist, schließen Sie nur Größen- und IOPS-Kombinationen ein, deren Ergebnis kleiner-gleich 0,30 IOPS/GB ist.
- Wenn der Originaldatenträger ein Performance-Tier mit mehr als 0,30 IOPS/GB ist, schließen Sie nur Größen- und IOPS-Kombinationen ein, deren Ergebnis höher als 0,30 IOPS/GB ist.

## Auswirkung der IOPS-Anpassung auf die Replikation

Wenn die Replikation auf dem Datenträger eingerichtet wurde, wird das Replikat automatisch so aktualisiert, dass es mit der IOPS-Auswahl des Primärdatenträgers übereinstimmt.

## IOPS im Speicher anpassen
{: #adjustingsteps}

1. Navigieren Sie zur {{site.data.keyword.blockstorageshort}}-Liste. Klicken Sie in der {{site.data.keyword.cloud}}-Konsole auf das **Menüsymbol** und anschließend auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie den iSCSI-Datenträger in der Liste aus und klicken Sie auf **...** > **LUN ändern**. 
3. Nehmen Sie unter **Speicher-IOPS anpassen** eine neue Auswahl vor: 
    - Wählen Sie für 'Endurance (Gestaffelte IOPS)' Sie ein IOPS-Tier aus, das größer als 0,25 IOPS/GB Ihres Speichers ist. Sie können den Wert für das IOPS-Tier jederzeit erhöhen. Seine Verringerung ist jedoch nur einmal pro Monat möglich.
    - Geben Sie für 'Performance (zugeordnete E/A-Operationen pro Sekunde)' die neue IOPS-Option für Ihren Speicher an, indem Sie einen Wert im Bereich von 100 bis 48.000 IOPS eingeben.

    Stellen Sie sicher, dass Sie alle spezifischen Grenzen beachten, die durch die Größe im Bestellformular erforderlich sind.
    {:tip}
4. Prüfen Sie Ihre Auswahl und die Details zur Preisstruktur.
5. Klicken Sie auf das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung aufgeben**.
6. Ihre neue Speicherzuordnung ist in wenigen Minuten verfügbar.


Alternativ dazu können Sie die IOPS über die SL-CLI anpassen.
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
  -h, --help                    Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}
