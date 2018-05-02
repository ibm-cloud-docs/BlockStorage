---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Snapshots verwalten

## Wie erstelle ich einen Snapshotplan?

Mithilfe von Snapshotplänen können Sie entscheiden, wie oft und wann Sie einen zeitpunktgesteuerten Verweis auf Ihren Speicherdatenträger erstellen wollen. Auf einem Speicherdatenträger können maximal 50 Snapshots vorhanden sein. Die Verwaltung der Pläne erfolgt über **Speicher**, Registerkarte **{{site.data.keyword.blockstorageshort}}** des [{{site.data.keyword.slportal}}s](https://control.softlayer.com/){:new_window}.

Damit Sie Ihren ersten Plan einrichten können, müssen Sie zuerst Snapshotbereich kaufen, falls Sie dies nicht bereits bei der Erstbereitstellung des Speicherdatenträgers getan haben.

### Wie füge ich einen Snapshotplan hinzu?

Snapshotpläne können für stündliche, tägliche und wöchentliche Intervalle und einen bestimmten Aufbewahrungszyklus eingerichtet werden. Es können maximal 50 geplante Snapshots (eine Mischung aus stündlichen, täglichen und wöchentlichen Plänen) und manuelle Snapshots gemacht werden.

1. Klicken Sie auf Ihren Speicherdatenträger, klicken Sie auf die Dropdown-Liste **Aktionen** und klicken Sie auf **Snapshot planen**.
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
3. Klicken Sie auf **Speichern** und erstellen Sie einen anderen Plan mit einer anderen Häufigkeit. Beachten Sie, dass Sie eine Warnung erhalten und nicht speichern können, wenn die Gesamtzahl der geplanten Snapshots 50 überschreitet.

Im Bereich 'Snapshots' der Seite 'Details' wird eine Liste der ausgeführten Snapshots angezeigt.

## Wie mache ich einen manuellen Snapshot?

Manuelle Snapshots können an verschiedenen Punkten während der Aktualisierung oder Wartung einer Anwendung gemacht werden. Sie können auf Anwendungsebene auch Snapshots auf mehreren Maschinen machen, die vorübergehend inaktiviert wurden.

Auf einem Speicherdatenträger können maximal 50 manuelle Snapshots vorhanden sein.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Dropdown-Liste 'Aktionen'.
3. Klicken Sie auf **Manuellen Snapshot machen**.
Der Snapshot wird gemacht und im Bereich 'Snapshots' der Seite 'Details' angezeigt. Für seinen Plan ist 'Manuell' festgelegt.

## Wie zeige ich eine Liste der Snapshots mit Speicherverbrauch und Managementfunktionen an?

Eine Liste mit den aufbewahrten Snapshots und dem verbrauchten Speicherplatz können Sie auf der Seite **Detail** (Speicher, {{site.data.keyword.blockstorageshort}}) anzeigen. Die Managementfunktionen (Pläne bearbeiten und mehr Speicherplatz hinzufügen) werden auf der Seite 'Details' mithilfe des Dropdown-Menüs **Aktionen** oder mit Links in den verschiedenen Seitenbereichen gesteuert.

## Wie zeige ich eine Liste der aufbewahrten Snapshots an?

Die aufbewahrten Snapshots basieren auf der Zahl, die Sie beim Einrichten Ihrer Pläne im Feld 'Letzten aufbewahren' eingegeben haben. Im Bereich 'Snapshots' können Sie die ausgeführten Snapshots anzeigen. Die Snapshots werden entsprechend dem Plan aufgelistet.

## Wie kann ich sehen, wie viel Snapshotbereich verwendet wurde?

Das Kreisdiagramm oben auf der Seite 'Details' zeigt an, wie viel Speicherplatz verwendet wurde und wie viel Speicherplatz übrig ist. Sie erhalten Benachrichtigungen, wenn Sie sich Schwellenwerten für den Speicherplatz nähern – 75%, 90% und 95%.

## Wie ändere ich die Menge des Snapshotbereichs für meinen Datenträger?

Möglicherweise müssen Sie einen Snapshotbereich zu einem Datenträger hinzufügen, der zuvor keinen hatte oder der zusätzlichen Snapshotbereich benötigt. Je nach Ihren Anforderungen können Sie zwischen 5 GB und 4.000 GB hinzufügen. 

**Hinweis**: Der Snapshotbereich kann nur erhöht und nicht verringert werden. Es empfiehlt sich, eine geringere Speichermenge auszuwählen, bis Sie ermittelt haben, wie viel Speicherplatz Sie tatsächlich brauchen. Denken Sie daran, dass automatisierte und manuelle Snapshots denselben Speicherplatz gemeinsam nutzen.

Der Snapshotbereich wird über **Speicher, {{site.data.keyword.blockstorageshort}}** geändert.

1. Klicken Sie auf Ihre Speicherdatenträger, klicken Sie auf die Dropdown-Liste **Aktionen** und klicken Sie auf **Mehr Snapshotbereich hinzufügen**.
2. Wählen Sie an der Eingabeaufforderung einen Wert aus einem Größenbereich aus. In der Regel reichen die Größen von 0 bis zur Größe Ihres Datenträgers.
3. Klicken Sie auf **Weiter**, um den zusätzlichen Speicherplatz bereitzustellen.
4. Geben Sie gegebenenfalls den Werbeaktionscode ein und klicken Sie auf **Neu berechnen**. In den Feldern für die Gebühren dieser Bestellung und für die Bestellungsprüfung stehen die Standardwerte.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Auftrag erteilen**. Ihr zusätzlicher Snapshotbereich wird in wenigen Minuten bereitgestellt.

## Wie erhalte ich Benachrichtigungen, wenn ich mich dem Grenzwert meines Snapshotbereichs nähere und Snapshots gelöscht werden?

Benachrichtigungen werden mithilfe von Support-Tickets vom Support an den Masterbenutzer Ihres Kontos gesendet, wenn Sie drei verschiedene Schwellenwerte für den Speicherplatz erreichen – 75%, 90% und 95%.

- **75% Kapazität**: Es wird eine Warnung gesendet, dass die Belegung des Snapshotbereichs 75% überschritten hat. Wenn Sie die Warnung beachten und manuell Speicherplatz hinzufügen oder aufbewahrte und überflüssige Snapshots löschen, wird die Aktion registriert und das Ticket geschlossen. Wenn Sie nichts tun, müssen Sie das Ticket manuell bestätigen, damit es geschlossen wird.
- **90% Kapazität**: Es wird eine zweite Warnung gesendet, wenn die Belegung des Snapshotbereichs 90% überschritten hat. Wie beim Erreichen der Kapazität von 75%, wird die Aktion registriert und das Ticket geschlossen, wenn Sie die notwendigen Aktionen zur Verringerung des belegten Speicherplatzes ausführen. Wenn Sie nichts tun, müssen Sie das Ticket manuell bestätigen, damit es geschlossen wird.
- **95% Kapazität**: Es wird eine letzte Warnung gesendet. Wenn Sie nichts unternehmen, um Ihren Speicherplatz unter den Schwellenwert zu senken, wird eine Benachrichtigung generiert und eine automatische Löschung durchgeführt, damit künftige Snapshots erstellt werden können. Geplante Snapshots werden gelöscht, beginnend mit dem ältesten, bis die Belegung unter 95% fällt, und die Löschung wird jedes Mal fortgesetzt, wenn die Belegung 95% übersteigt, bis sie unter den Schwellenwert fällt. Der der Speicherplatz manuell erhöht wird oder Snapshots gelöscht werden, wird die Warnung zurückgesetzt und erneut abgesetzt, sobald der Schwellenwert wieder überschritten wird. Wenn Sie nichts unternehmen, ist dies die einzige Warnung, die Sie erhalten.

## Wie lösche ich einen Snapshotplan?

Snapshotpläne können über **Speicher, {{site.data.keyword.blockstorageshort}}** abgebrochen werden.

1. Klicken Sie auf der Seite **Details** im Rahmen **Snapshotpläne** auf den Plan, der gelöscht werden soll.
2. Aktivieren Sie das Kontrollkästchen neben dem Plan, der gelöscht werden soll, und klicken Sie auf **Speichern**.<br />
**Vorsicht**: Achten Sie bei Verwendung der Replikationsfunktion darauf, dass der Plan, den Sie gerade löschen, nicht der von der Replikation verwendete Plan ist. Weitere Informationen zum Löschen eines Replikationsplans finden Sie [hier](replication.html).

## Wie lösche ich einen Snapshot?

Snapshots, die nicht mehr benötigt werden, können manuell entfernt werden, um Speicherplatz für künftige Snapshots freizugeben. Die Löschung erfolgt über **Speicher, {{site.data.keyword.blockstorageshort}}**.

1. Klicken Sie auf Ihren Speicherdatenträger und blättern Sie abwärts zum Bereich 'Snapshot', um eine Liste der vorhandenen Snapshots anzuzeigen.
2. Klicken Sie neben einem bestimmten Snapshot auf die Dropdown-Liste **Aktionen** und klicken Sie auf **Löschen**, um den Snapshot zu löschen. Diese Aktion hat keine Auswirkungen auf künftige oder frühere Snapshot in demselben Plan, weil es keine Abhängigkeit zwischen Snapshots gibt.

Manuelle Snapshots, die nicht in der oben beschriebenen Weise gelöscht werden, werden automatisch gelöscht (der älteste zuerst), wenn Sie die Speichergrenzen erreichen.

## Wenn kann ich meinen Speicherdatenträger mit einem Snapshot auf einen bestimmten Zeitpunkt wiederherstellen?

Möglicherweise müssen Sie Ihren Speicherdatenträger aufgrund eines Benutzerfehlers oder von Datenverlust auf einen bestimmten Zeitpunkt zurücksetzen.

1. Hängen Sie Ihren Speicherdatenträger vom Host ab.
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Linux finden Sie [hier](accessing_block_storage_linux.html).
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Microsoft Windows finden Sie [hier](accessing-block-storage-windows.html).
2. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
3. Blättern Sie abwärts und klicken Sie auf Ihren wiederherzustellenden Datenträger. Im Bereich **Snapshots** der Seite **Detail** wird eine Liste aller gespeicherten Snapshots mit ihrer Größe und dem Erstellungsdatum angezeigt.
4. Klicken Sie auf die Schaltfläche **Aktionen** des Snapshots, den Sie verwenden wollen, und klicken Sie auf **Wiederherstellen**. <br/>
  **Hinweis**: Bei einer Wiederherstellung gehen die Daten verloren, die seit dem Moment, an dem der betreffende Snapshot gemacht wurde, erstellt oder geändert wurden. Sobald die Aktion abgeschlossen ist, wird Ihr Speicherdatenträger in denselben Zustand versetzt, in dem er zum Zeitpunkt seiner Erstellung war. An einer Eingabeaufforderung wird eine entsprechende Benachrichtigung darüber angezeigt.
5. Klicken Sie auf **Ja**, um die Wiederherstellung einzuleiten. Oben auf der Seite wird eine Nachricht angezeigt, dass der Datenträger mithilfe des ausgewählten Snapshots wiederhergestellt wurde. Außerdem wird neben Ihrem Datenträger auf dem {{site.data.keyword.blockstorageshort}} ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie die Maus über das Symbol bewegen, wird ein Dialogfeld mit Angaben zu der Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet.
6. Hängen Sie Ihren Speicherdatenträger wieder an den Host an.
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Linux finden Sie [hier](accessing_block_storage_linux.html).
   - Die Anweisungen zu {{site.data.keyword.blockstorageshort}} unter Microsoft Windows finden Sie [hier](accessing-block-storage-windows.html).
   
**Hinweis**: Bei der Wiederherstellung eines Datenträgers werden alle Snapshots gelöscht, die vor dem wiederhergestellten Snapshot gemacht wurden.
