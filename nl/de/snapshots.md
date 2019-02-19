---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Snapshots
{: #snapshots}

Snapshots sind eine Funktion von {{site.data.keyword.blockstoragefull}}. Ein Snapshot stellt den Inhalt eines Datenträgers zu einem bestimmten Zeitpunkt dar. Mit Snapshots können Sie Ihre Daten ohne Leistungseinflüsse und minimalem Platzbedarf schützen. Snapshots gelten als erste Verteidigungslinie beim Datenschutz. Mit der Snapshotfunktion können Daten schnell und bequem von einer Snapshotkopie wiederhergestellt werden, wenn ein Benutzer wichtige Daten auf einem Datenträger versehentlich ändert oder löscht.

{{site.data.keyword.blockstorageshort}} bietet Ihnen zwei Möglichkeiten, Snapshots zu erstellen.

* Zum einen über einen konfigurierbaren Snapshotplan, der Snapshotkopien automatisch für die einzelnen Speicherdatenträger erstellt und löscht. Je nach Ihren Anforderungen können Sie auch weitere Snapshotpläne erstellen, Kopien manuell löschen und Pläne verwalten.
* Die zweite Möglichkeit besteht in einer manuellen Snapshotaufnahme.

Eine Snapshotkopie ist eine schreibgeschützte Abbildung einer {{site.data.keyword.blockstorageshort}}-LUN, die den Zustand des Datenträgers zu einem bestimmten Zeitpunkt erfasst. Snapshotkopien sind effizient, was die benötigte Zeit zur Erstellung und den Speicherplatz anbelangt. Die Erstellung einer {{site.data.keyword.blockstorageshort}}-Snapshotkopie dauert nur wenige Sekunden. In der Regel dauert es sogar weniger als 1 Sekunde, unabhängig von der Größe des Datenträgers und der Aktivitätsstufe auf dem Datenträger. Nachdem eine Snapshotkopie erstellt wurde, werden Änderungen an Datenobjekten in Aktualisierungen der aktuellen Version der Objekte abgebildet, so als wären keine Snapshotkopien vorhanden. Gleichzeitig bleibt die Kopie der Daten stabil.

Eine Snapshotkopie verursacht keine Leistungseinbußen. Benutzer können ohne großen Aufwand bis zu 50 geplante Snapshots und 50 manuelle Snapshots pro {{site.data.keyword.blockstorageshort}}-Datenträger speichern, die alle als schreibgeschützte und Onlineversionen der Daten zugänglich sind.

Snapshots bieten Ihnen folgende Möglichkeiten:

- Zeitpunktgesteuerte Wiederherstellungspunkte ohne Betriebsunterbrechung erstellen
- Datenträger auf einen früheren Zeitpunkt zurücksetzen

Sie müssen zunächst für Ihren Datenträger einen bestimmten Snapshotbereich kaufen, um Snapshots davon machen zu können. Der Snapshotbereich kann bei der Erstbestellung oder danach über **Datenträgerdetails** hinzugefügt werden. Geplante und manuelle Snapshots nutzen den Snapshotbereich gemeinsam; stellen Sie deswegen sicher, dass Sie ausreichend Speicherplatz bestellen. Weitere Informationen finden Sie unter [Snapshots bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).

## Bewährte Verfahren für Snapshots

Die Snapshotgestaltung richtet sich nach der Umgebung des Kunden. Die folgenden konzeptionellen Überlegungen können Ihnen helfen, Snapshotkopien zu planen und zu implementieren:
- Bis zu 50 Snapshots können über einen Plan und bis zu 50 können manuell auf den einzelnen Datenträgern oder LUNs erstellt werden.
- Verwenden Sie nicht zu viele Snapshots. Stellen Sie sicher, dass Ihre geplante Snapshothäufigkeit Ihre RTO- und RPO-Bedürfnisse sowie Ihre Anwendungsgeschäftsanforderungen erfüllt, indem Sie stündliche, tägliche oder wöchentliche Snapshots planen.
- Mit der automatischen Löschung von Snapshots (AutoDelete) kann die Zunahme der Speicherbelegung gesteuert werden. <br/>

  Der Schwellenwert AutoDelete ist auf 95 Prozent festgelegt.
  {:note}

Snapshots stellen keinen Ersatz für eine tatsächliche ausgelagerte Disaster-Recovery-Replikation oder eine lange Aufbewahrungssicherung dar.
{:important}

## Sicherheit

Auch alle Screenshots und Replikate von verschlüsseltem {{site.data.keyword.blockstorageshort}} werden standardmäßig verschlüsselt. Diese Funktion kann nicht auf einzelnen Datenträgern inaktiviert werden. Weitere Informationen zum vom Provider verwalteten verschlüsselten Speicher finden Sie unter [Daten schützen](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

## Auswirkungen von Snapshots auf den Plattenspeicher

Snapshotkopien minimieren die Plattennutzung, da sie einzelne Blöcke anstelle von ganzen Dateien beibehalten. Snapshotkopien verbrauchen nur dann zusätzlichen Speicherplatz, wenn Dateien im aktiven Dateisystem geändert oder gelöscht werden.

Die geänderten Blöcke werden im aktiven Dateisystem an verschiedenen Positionen auf dem Datenträger neu erstellt oder als aktive Dateiblöcke komplett entfernt. Wenn Dateien geändert oder gelöscht werden, werden die ursprünglichen Dateiblöcke in einer oder mehreren Snapshotkopien beibehalten. Somit wird der von den Originalblöcken verwendete Plattenspeicher weiter reserviert, um den Status des aktiven Dateisystems vor der Änderung abzubilden. Dieser Speicherplatz wird zusätzlich zu dem Plattenspeicher reserviert, der von Blöcken im geänderten aktiven Dateisystem verwendet wird.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Plattenspeicherplatzbelegung vor und nach der Snapshotkopie</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Vor der Snapshotkopie"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Nach der Snapshotkopie"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Änderungen nach der Snapshotkopie"></td>
     </tr><tr>
        <td style="border: 0.0px;">Bevor eine Snapshotkopie erstellt wird, wird Plattenspeicher nur vom aktiven Dateisystem verbraucht.</td>
        <td style="border: 0.0px;">Nachdem eine Snapshotkopie erstellt wurde, verweisen das aktive Dateisystem und die Snapshotkopie auf dieselben Plattenblöcke. Die Snapshotkopie verbraucht keinen zusätzlichen Plattenspeicher.</td>
        <td style="border: 0.0px;">Nachdem die Datei <i>myfile.txt</i> aus dem aktiven Dateisystem gelöscht wurde, umfasst die Snapshotkopie weiterhin die Datei und verweist auf ihre Plattenblöcke. Deswegen bedeutet das Löschen von Daten des aktiven Dateisystems nicht immer eine Freigabe von Plattenspeicher.</td>
      </tr>
</table>

Weitere Informationen zur Speichernutzung von Snapshots finden Sie in [Snapshots verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots).
