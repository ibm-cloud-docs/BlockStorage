---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots bestellen

Um automatisch oder manuell Snapshots Ihres Speicherdatenträgers zu erstellen, müssen Sie Speicherbereich kaufen, der diese Snapshots aufnehmen kann. Sie können Kapazität bis zur Menge Ihres Speicherdatenträgers kaufen (beim Kauf des ursprünglichen Datenträgers oder später mit den nachfolgenden Schritten).

1. Greifen Sie über **Speicher**, Registerkarte **{{site.data.keyword.blockstorageshort}}** des [{{site.data.keyword.slportal}}s ](https://control.softlayer.com/){:new_window} auf Ihre Speicher-LUN zu.
2. Klicken Sie im Rahmen 'Snapshots' auf den Link **Snapshotbereich hinzufügen**.
3. Wählen Sie den benötigten Speicherplatz aus, indem Sie auf das Optionsfeld neben der entsprechenden Menge klicken.
4. Klicken Sie auf **Weiter**.
5. Geben Sie gegebenenfalls den Werbeaktionscode ein und klicken Sie auf **Neu berechnen**. In den Feldern für die Gebühren dieser Bestellung und für die Bestellungsprüfung stehen die Standardwerte.
6. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Auftrag erteilen**. Ihr Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Vorgehensweise zum Ermitteln der Menge des zu bestellenden Snaphostbereichs

Leider gibt es kein bewährtes Verfahren, das als anwendungsbasierte Empfehlung dienen könnte. Im Allgemeinen wird ein Snapshotbereich von Snapshots auf der Basis von zwei Informationen verbraucht:
- Menge der Änderungen Ihres aktiven Dateisystems 
- Geplante Dauer der Beibehaltung der Snapshots  

Prinzipiell lässt sich der benötigte Speicherplatz wie folgt berechnen: **(Änderungsrate)** x **(Anzahl der beibehaltenen Stunden/Tage/Wochen/Monate)**.  
**Hinweis**: Der erste Snapshot verbraucht nur sehr wenig Speicherplatz, weil er lediglich eine Kopie der Metadaten (Verweise) ist, die die Blöcke des aktiven Dateisystems angeben. 

Ein Datenträger mit vielen Datenänderungen (beispielsweise eine Datenbank mit hoher Änderungsrate) und mit einer langen Aufbewahrungsdauer der Snapshots braucht mehr Speicherplatz für Snapshots als ein Datenträger mit moderaten Änderungen (beispielsweise ein VM-Datenspeicher) und einer moderateren Aufbewahrungsfrist für Snapshots. 

Wenn Sie bei einem Datenträger mit 500 GB Daten 12 stündliche Snapshots machen und zwischen den einzelnen Snapshots 1% Änderung haben, ergibt das (5 GB Änderungsrate) x (12 stündliche Snapshots) = 60 GB für Snapshots.

Wenn sich andererseits bei diesen 500 GB Daten mit 12 stündlichen Snapshots jede Stunde 10% ändern, ergibt das (50 GB Änderungsrate) x (12 stündliche Snapshots) = 600 GB.

Berücksichtigen Sie also bei der Bestimmung des benötigten Snapshotbereichs die Änderungsrate in angemessener Weise. Sie hat erheblichen Einfluss auf die Menge des erforderlichen Snapshotbereichs. Auch wenn die Größe eines Datenträgers tendenziell eine größere Änderungsmenge bedeutet, belegen ein 500-GB-Datenträger mit 5 GB Änderung und ein 10-TB-Datenträger mit 5 GB Änderung denselben Snapshotspeicher.

Außerdem gilt bei den meisten Workloads, das umso weniger Speicherplatz anfangs für Snapshots reserviert werden muss, je größer ein Datenträger ist. Dies liegt in erster Linie an der zugrunde liegenden Datenleistung Ihrer Plattform sowie an der Funktionsweise von Snapshots in Ihrer Umgebung.



