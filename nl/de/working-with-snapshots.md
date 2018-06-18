---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots verwalten

## Wie erstelle ich einen Snapshotplan?

Sie entscheiden, wie oft und wann eine Referenz mit Zeitangabe Ihres Speicherdatenträgers mit Snapshotplänen erstellt werden soll. Auf einem Speicherdatenträger können maximal 50 Snapshots vorhanden sein. Die Pläne werden über **Speicher** in der Registerkarte **{{site.data.keyword.blockstorageshort}}** des [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}s verwaltet. 

Damit Sie einen ersten Plan konfigurieren können, müssen Sie vorher einen Snapshotbereich kaufen, sofern Sie noch keinen während der Ersteinrichtung des Speicherdatenträgers gekauft haben.

### Wie füge ich einen Snapshotplan hinzu?

Snapshotpläne können für stündliche, tägliche und wöchentliche Intervalle und einen bestimmten Aufbewahrungszyklus eingerichtet werden. Es können maximal 50 geplante Snapshots (eine Mischung aus stündlichen, täglichen und wöchentlichen Plänen) und manuelle Snapshots gemacht werden.

1. Klicken Sie auf den Speicherdatenträger, klicken Sie auf **Aktionen** und klicken Sie auf **Snapshot planen**.
2. Im Dialogfeld 'Neuer Snapshotplan' können Sie aus drei verschiedenen Snapshothäufigkeiten auswählen. Verwenden Sie eine beliebige Kombination dieser drei Optionen, um einen umfassenden Snapshotplan zu erstellen
   - Stündlich
      - Geben Sie die Minute jeder Stunde an, zu der der Snapshot durchgeführt werden soll. Der Standardwert ist die aktuelle Minute.
      - Geben Sie die Anzahl der stündlichen Snapshots an, die aufbewahrt werden sollen, bevor der älteste gelöscht wird.
   - Täglich
      - Geben Sie die Stunde und Minute an, zu der der Snapshot durchgeführt werden soll. Der Standardwert ist die aktuelle Stunde und Minute.
      - Wählen Sie die Anzahl der täglichen Snapshots aus, die aufbewahrt werden sollen, bevor der älteste gelöscht wird.
   - Wöchentlich
      - Geben Sie den Tag der Woche, die Stunde und Minute an, zu der der Snapshot durchgeführt werden soll. Der Standardwert ist der aktuelle Tag, die aktuelle Stunde und die aktuelle Minute.
      - Wählen Sie die Anzahl der wöchentlichen Snapshots aus, die aufbewahrt werden sollen, bevor der älteste gelöscht wird.
3. Klicken Sie auf **Speichern** und erstellen Sie einen anderen Plan mit einer anderen Häufigkeit. Falls die Gesamtzahl der geplanten Snapshots 50 übersteigt, wird eine Warnung angezeigt und Sie können keine Speicherung durchführen.

Die Liste der Snapshots wird so angezeigt, wie sie im Abschnitt **Snapshots** der Seite **Details** dargestellt werden.

## Wie erstelle ich einen manuellen Snapshot?

Manuelle Snapshots können an verschiedenen Punkten während der Aktualisierung oder Wartung einer Anwendung gemacht werden. Sie können auch Snapshots für mehrere Maschinen erstellen, die temporär auf der Anwendungsebene inaktiviert wurden.

Auf einem Speicherdatenträger sind maximal 50 Snapshots möglich.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf **Aktionen**.
3. Klicken Sie auf **Manuellen Snapshot machen**.
Der Snapshot wird erstellt und im Abschnitt **Snapshots** der Seite **Details** angezeigt. Für seinen Plan ist 'Manuell' festgelegt.

## Wie zeige ich eine Liste der Snapshots mit verwendetem Bereich und Verwaltungsfunktionen an?

Eine Liste mit den aufbewahrten Snapshots und dem verbrauchten Speicherplatz können Sie auf der Seite **Detail** (Speicher, {{site.data.keyword.blockstorageshort}}) anzeigen. Verwaltungsfunktionen (Bearbeiten von Plänen und Hinzufügen von zusätzlichem Speicherplatz) können auf der Seite 'Details' mithilfe des Menüs **Aktionen** oder Links im unterschiedlichen Abschnitten auf der Seite ausgeführt werden.

## Wie zeige ich eine Liste der aufbewahrten Snapshots an?

Aufbewahrte Snapshot basieren auf der Nummer, die Sie in das Feld **Letzte aufbewahren** beim Konfigurieren der Pläne eingegeben haben. Sie können die Snapshots anzeigen, die im Abschnitt **Snapshot** erstellt wurden. Die Snapshots werden entsprechend dem Plan aufgelistet.

## Wie kann ich erkennen, wie viel Snapshotbereich belegt ist?

Im Kreisdiagramm oben auf der Seite **Details** wird angezeigt, wie viel Speicherplatz belegt ist und wie viel Speicherplatz frei ist. Sie erhalten Benachrichtigungen, wenn die Schwellenwerte für den Speicherplatz erreicht werden - 75 Prozent, 90 Prozent und 95 Prozent.

## Wie ändere ich die Menge des Snapshotbereichs für meinen Datenträger?

Es kann vorkommen, dass Sie Snapshotbereich zu einem Datenträger hinzufügen möchten, auf dem vorher keiner vorhanden war oder dass Sie zusätzlichen Snapshotbereich benötigen. Sie können je nach Bedarf zwischen 5 GB und 4.000 GB hinzufügen. 

**Hinweis**: Der Snapshotbereich kann nur erhöht und nicht verringert werden. Es empfiehlt sich, eine geringere Speichermenge auszuwählen, bis Sie ermittelt haben, wie viel Speicherplatz Sie tatsächlich brauchen. Bedenken Sie dabei, dass der Speicherplatz gemeinsam von automatisierten und manuellen Snapshots verwendet wird.

Der Snapshotbereich kann über **Speicher** > **{{site.data.keyword.blockstorageshort}}** geändert werden.

1. Klicken Sie auf die Speicherdatenträger, klicken Sie auf **Aktionen** und klicken Sie auf **Mehr Snapshotbereich hinzufügen**.
2. Wählen Sie an der Eingabeaufforderung einen Wert aus einem Größenbereich aus. In der Regel reichen die Größen von 0 bis zur Größe Ihres Datenträgers.
3. Klicken Sie auf **Weiter**.
4. Geben Sie gegebenenfalls den Werbeaktionscode ein und klicken Sie auf **Neu berechnen**. In den Feldern für die Gebühren dieser Bestellung und für die Bestellungsprüfung stehen die Standardwerte.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Auftrag erteilen**. Ihr zusätzlicher Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Wie empfange ich Benachrichtigungen, wenn der Grenzwert für den Snapshotbereich fast erreicht ist und Snapshots gelöscht werden?

Benachrichtigungen werden über Support-Tickets an den Masterbenutzer eines Kontos gesendet, wenn Sie drei unterschiedliche Schwellenwerte für den Speicherplatz erreichen - 75 Prozent, 90 Prozent und 95 Prozent.

- **75 Prozent der Kapazität**: Eine Warnung wird gesendet, die besagt, dass 75 Prozent Speicherbelegung überschritten wurden. Wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen, wird die Aktion notiert und das Ticket geschlossen. Wenn Sie nichts tun, müssen Sie das Ticket manuell bestätigen, damit es geschlossen wird.
- **90 Prozent der Kapazität**: Eine Warnung wird gesendet, wenn die Verwendung durch den Snapshotbereich 90 Prozent überschreitet. Wie bei Erreichen von 75 Prozent der Kapazität gilt, dass die Aktion notiert und das Ticket geschlossen wird, wenn Sie auf die Warnung reagieren und Speicherbereich manuell hinzufügen oder aufbewahrte und unnötige Snapshots löschen. Wenn Sie nichts tun, müssen Sie das Ticket manuell bestätigen, damit es geschlossen wird.
- **95 Prozent der Kapazität**: Eine letzte Warnung wird gesendet. Wenn Sie nichts unternehmen, um Ihren Speicherplatz unter den Schwellenwert zu senken, wird eine Benachrichtigung generiert und eine automatische Löschung durchgeführt, damit künftige Snapshots erstellt werden können. Geplante Snapshots werden gelöscht, beginnend mit dem ältesten, bis die Belegung unter 95 Prozent liegt. Snapshots werden weiterhin jedes mal gelöscht, wenn die Belegung 95 Prozent überschreitet, bis die Belegung unter dem Schwellenwert liegt. Wenn der Speicherbereich manuell vergrößert oder Snapshots gelöscht werden, wird die Warnung zurückgesetzt und erneut ausgegeben, wenn der Schwellenwert wieder überschritten wird. Wenn Sie nichts unternehmen, ist dies die einzige Warnung, die Sie erhalten.

## Wie lösche ich einen Snapshotplan?

Snapshotpläne können über **Speicher** > **{{site.data.keyword.blockstorageshort}}** gelöscht werden.

1. Klicken Sie im Rahmen **Snapshotpläne** auf der Seite **Details** auf den Plan, der gelöscht werden soll.
2. Klicken Sie auf das Kontrollkästchen neben dem zu löschenden Plan und klicken Sie auf **Speichern**.<br />
**Achtung**: Wenn Sie die Replikationsfunktion verwenden, stellen Sie sicher, dass der Zeitplan, den Sie löschen, nicht der von der Replikation verwendete Zeitplan ist. Weitere Informationen zum Löschen eines Replikationsplans finden Sie [hier](replication.html).

## Wie wird ein Snapshot gelöscht?

Snapshots, die nicht mehr benötigt werden, können manuell entfernt werden, um Speicherplatz für künftige Snapshots freizugeben. Das Löschen erfolgt über **Speicher** > **{{site.data.keyword.blockstorageshort}}**.

1. Klicken Sie auf den Speicherdatenträger und blättern Sie bis zum Abschnitt **Snapshot**, um die Liste der vorhandenen Snapshots anzuzeigen.
2. Klicken Sie auf **Aktionen** neben dem gewünschten Snapshot und klicken Sie auf **Löschen**, um den Snapshot zu löschen. Diese Aktion hat keine Auswirkung auf zukünftige oder frühere Snapshots im selben Zeitplan, da zwischen Snapshots keine Abhängigkeit besteht.

Manuelle Snapshots, die nicht mit der oben beschriebenen Methode gelöscht werden, werden automatisch (älteste zuerst) gelöscht, wenn Sie die Speicherbereichsbegrenzung erreichen.

## Wie wird ein Speicherdatenträger mit einem Snapshot auf dem Stand eines bestimmten Zeitpunkts wiederhergestellt?

Es kann vorkommen, dass Sie Ihren Speicherdatenträger aufgrund eines Benutzerfehlers oder einer Datenbeschädigung auf einen bestimmten Zeitpunkt zurücksetzen müssen.

1. Hängen Sie Ihren Speicherdatenträger vom Host ab.
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Linux finden Sie [hier](accessing_block_storage_linux.html).
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Microsoft Windows finden Sie [hier](accessing-block-storage-windows.html).
2. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
3. Blättern Sie nach unten und klicken Sie auf den Datenträger, der wiederhergestellt werden soll. Im Abschnitt **Snapshots** der Seite **Details** wird die Liste aller gespeicherten Snapshots mit Angabe ihrer Größe und ihres Erstellungsdatums angezeigt.
4. Klicken Sie auf **Aktionen** neben dem Snapshot, der verwendet werden soll, und klicken Sie auf **Wiederherstellen**. <br/>
  **Hinweis**: Bei einer Wiederherstellung gehen die Daten verloren, die seit dem Moment, an dem der betreffende Snapshot gemacht wurde, erstellt oder geändert wurden. Nach Abschluss der Wiederherstellung befindet sich Ihr Speicherdatenträger in dem Status des Zeitpunkts, zu dem der Snapshot erfasst wurde. 
5. Klicken Sie auf **Ja**, um die Wiederherstellung einzuleiten. Sie empfangen eine Nachricht im oberen Bereich der Seite, die besagt, dass der Datenträger mit dem ausgewählten Snapshot wiederhergestellt wurde. Außerdem wird neben Ihrem Datenträger auf dem {{site.data.keyword.blockstorageshort}} ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie die Maus über das Symbol bewegen, wird ein Dialogfeld mit Angaben zu der Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. 
6. Hängen Sie Ihren Speicherdatenträger wieder an den Host an und verbinden Sie ihn erneut. 
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Linux finden Sie [hier](accessing_block_storage_linux.html).
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Microsoft Windows finden Sie [hier](accessing-block-storage-windows.html).
   
**Hinweis**: Die Wiederherstellung eines Datenträgers hat zur Folge, dass alle Snapshots vor dem wiederhergestellten Snapshot gelöscht werden.
