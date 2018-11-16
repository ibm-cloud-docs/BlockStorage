---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Snapshots bestellen

Um automatisch oder manuell Snapshots Ihres Speicherdatenträgers zu erstellen, müssen Sie Speicherbereich kaufen, der diese Snapshots aufnehmen kann. Sie können Kapazität bis zur Menge Ihres Speicherdatenträgers kaufen (beim Kauf des ursprünglichen Datenträgers oder später mit den hier beschriebenen Schritten).

1. Melden Sie sich an der [{{site.data.keyword.cloud_notm}}-Konsole](https://console.bluemix.net/catalog/){:new_window} an und klicken Sie oben links auf das Symbol **Menü**. Wählen Sie **Klassische Infrastruktur** aus. 

   Alternativ können Sie sich beim [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} anmelden.
2. Greifen Sie über **Speicher** >**{{site.data.keyword.blockstorageshort}}** auf Ihre Speicher-LUN zu.
2. Klicken Sie im Rahmen 'Snapshots' auf die Option zum Ändern des Snapshotbereichs. 
3. Wählen Sie die Menge an benötigtem Speicherplatz und die Zahlungsmethode aus. 
4. Klicken Sie auf **Weiter**.
5. Geben Sie gegebenenfalls den **Werbeaktionscode** ein und klicken Sie auf **Neu berechnen**. In den Feldern für die Gebühren dieser Bestellung und für die Bestellungsprüfung stehen die Standardwerte.
6. Markieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen und bin mit den darin genannten Bedingungen einverstanden.** und klicken Sie auf **Bestellung aufgeben**. Der Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Menge des zu bestellenden Snapshotbereichs ermitteln

Im Allgemeinen wird ein Snapshotbereich von Snapshots auf der Basis von zwei Faktoren verbraucht:
- Menge der Änderungen im aktiven Dateisystem im zeitlichen Verlauf
- Geplante Dauer der Beibehaltung der Snapshots  

Der benötigte Speicherplatz kann wie folgt berechnet werden: **(Änderungsrate)** x **(Anzahl der beibehaltenen Stunden/Tage/Wochen/Monate)**.

Der erste Snapshot verwendet nur sehr wenig Speicherplatz, da es sich bei ihm um eine Kopie der Metadaten (Verweise) handelt, mit denen die Blöcke des aktiven Dateisystems angegeben werden.
{:note}

Ein Datenträger mit vielen Änderungen und einer langen Aufbewahrungsdauer braucht mehr Speicherplatz als ein Datenträger mit moderaten Änderungen und einer moderateren Aufbewahrungsfrist. Ein Beispiel für den ersten Typ ist eine Datenbank mit einer hohen Änderungsrate. Ein Beispiel für den zweiten Typ ist ein VMware-Datenspeicher.

Wenn Sie bei einem Datenträger mit 500 GB Daten stündlich 12 Snapshots erstellen und die Änderung zwischen den einzelnen Snapshots 1 Prozent beträgt, ergibt dies 60 GB für Snapshots.

*(5 GB Änderungsrate) x (12 stündliche Snapshots) = (60 GB belegter Speicherplatz)*

Wenn sich andererseits bei diesen 500 GB Daten mit 12 stündlichen Snapshots jede Stunde 10 Prozent ändern, weist der verwendete Snapshotbereich 600 GB auf.

*(50 GB Änderungsrate) x (12 stündliche Snapshots) = (600 GB belegter Speicherplatz)*

Berücksichtigen Sie also bei der Bestimmung des benötigten Snapshotbereichs die Änderungsrate in angemessener Weise. Sie hat erheblichen Einfluss auf die Menge des erforderlichen Snapshotbereichs. Auf einem größeren Datenträger werden wahrscheinlich häufiger Änderungen auftreten. Ein 500-GB-Datenträger mit Änderungen im Umfang von 5 GB und ein 10-TB-Datenträger mit Änderungen im Umfang von 5 GB belegen dieselbe Menge an Snapshotspeicherplatz.

Außerdem gilt bei den meisten Workloads, dass anfangs umso weniger Speicherplatz reserviert werden muss, je größer ein Datenträger ist. Dies liegt in erster Linie an der zugrunde liegenden Datenleistung sowie an der Funktionsweise von Snapshots in der Umgebung.
