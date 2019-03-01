---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Snapshots bestellen
{: #orderingsnapshots}

Um automatisch oder manuell Snapshots Ihres Speicherdatenträgers zu erstellen, müssen Sie Speicherbereich kaufen, der diese Snapshots aufnehmen kann. Sie können Kapazität bis zur Menge Ihres Speicherdatenträgers kaufen (beim Kauf des ursprünglichen Datenträgers oder später mit den hier beschriebenen Schritten).

## Menge des zu bestellenden Snapshotbereichs ermitteln

Im Allgemeinen wird ein Snapshotbereich von Snapshots auf der Basis von zwei Faktoren verbraucht:
- Menge der Änderungen im aktiven Dateisystem im zeitlichen Verlauf
- Geplante Dauer der Beibehaltung der Snapshots  

Der benötigte Speicherplatz kann wie folgt berechnet werden: **(Änderungsrate)** x **(Anzahl der beibehaltenen Stunden/Tage/Wochen/Monate)**.

Der erste Snapshot verwendet nur sehr wenig Speicherplatz, da es sich bei ihm um eine Kopie der Metadaten (Verweise) handelt, mit denen die Blöcke des aktiven Dateisystems angegeben werden.
{:note}

Ein Datenträger mit vielen Änderungen und einer langen Aufbewahrungsdauer braucht mehr Speicherplatz als ein Datenträger mit moderaten Änderungen und einer moderateren Aufbewahrungsfrist. Ein Beispiel für den ersten Typ ist eine Datenbank mit einer hohen Änderungsrate. Ein Beispiel für den zweiten Typ ist ein VMware-Datenspeicher.

Wenn Sie bei einem Datenträger mit 500 GB Daten stündlich 12 Snapshots erstellen und die Änderung zwischen den einzelnen Snapshots 1 Prozent beträgt, ergibt dies 60 GB für Snapshots.

*(50 GB Änderungsrate) x (12 stündliche Snapshots) = (60 GB belegter Speicherplatz)*

Wenn sich andererseits bei diesen 500 GB Daten mit 12 stündlichen Snapshots jede Stunde 10 Prozent ändern, weist der verwendete Snapshotbereich 600 GB auf.

*(50 GB Änderungsrate) x (12 stündliche Snapshots) = (600 GB belegter Speicherplatz)*

Berücksichtigen Sie also bei der Bestimmung des benötigten Snapshotbereichs die Änderungsrate in angemessener Weise. Sie hat erheblichen Einfluss auf die Menge des erforderlichen Snapshotbereichs. Auf einem größeren Datenträger werden wahrscheinlich häufiger Änderungen auftreten. Ein 500-GB-Datenträger mit Änderungen im Umfang von 5 GB und ein 10-TB-Datenträger mit Änderungen im Umfang von 5 GB belegen dieselbe Menge an Snapshotspeicherplatz.

Außerdem gilt bei den meisten Workloads, dass anfangs umso weniger Speicherplatz reserviert werden muss, je größer ein Datenträger ist. Dies liegt in erster Linie an der zugrunde liegenden Datenleistung sowie an der Funktionsweise von Snapshots in der Umgebung.

## Snapshotbereich über die {{site.data.keyword.cloud_notm}}-Konsole bestellen

1. Melden Sie sich an der [{{site.data.keyword.cloud_notm}}-Konsole ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/catalog/){:new_window} an und klicken Sie auf das Menüsymbol oben links. Wählen Sie **Klassische Infrastruktur** aus.

   Alternativ können Sie sich beim [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} anmelden.
2. Greifen Sie über **Speicher** >**{{site.data.keyword.blockstorageshort}}** auf Ihre Speicher-LUN zu.
2. Klicken Sie im Rahmen 'Snapshots' auf die Option zum Ändern des Snapshotbereichs.
3. Wählen Sie die Menge an benötigtem Speicherplatz und die Zahlungsmethode aus.
4. Klicken Sie auf **Weiter**.
5. Geben Sie gegebenenfalls den **Werbeaktionscode** ein und klicken Sie auf **Neu berechnen**. In den Feldern für die Gebühren dieser Bestellung und für die Bestellungsprüfung stehen die Standardwerte.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
6. Wählen Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen und bin mit den darin genannten Bedingungen einverstanden** aus und klicken Sie auf **Bestellung aufgeben**. Der Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Snapshotbereich über die SL-CLI bestellen

```
# slcli block snapshot-order --help
Syntax: slcli block snapshot-order [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --capacity INTEGER    Größe des zu erstellenden Snapshots in GB  [erforderlich]
  --tier [0.25|2|4|10]  Endurance-Speichertier (IOPS per GB) des Blockspeicher-
                        datenträgers, für den Speicherbereich bestellt wird [optional,
                        nur gültig für Endurance-Speicherdatenträger]
  --upgrade             Flag zur Angabe, dass es sich um ein Upgrade handelt
  -h, --help            Diese Nachricht anzeigen und Ausführung beenden.
```
{codeblock}
