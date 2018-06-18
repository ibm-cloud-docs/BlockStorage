---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Upgrade für vorhandenen {{site.data.keyword.blockstorageshort}} auf erweiterten {{site.data.keyword.blockstorageshort}} durchführen

In ausgewählten Rechenzentren ist jetzt erweiterter {{site.data.keyword.blockstoragefull}} verfügbar. Wenn Sie eine Liste der aktualisierten Rechenzentren und verfügbaren Funktionen anzeigen möchten, zum Beispiel konfigurierbare IOPS-Raten und erweiterbare Datenträger, klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html). Weitere Informationen zu anbietergesteuertem verschlüsseltem Speicher finden Sie im Artikel [{{site.data.keyword.blockstorageshort}} - Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html).

Der bevorzugte Migrationspfad ist die gleichzeitige Verbindung zu beiden LUNs und die direkte Übertragung der Daten von einer LUN zur anderen. Die jeweiligen technischen Daten hängen vom Betriebssystem ab und ob erwartet wird, dass die Daten während der Kopieroperation geändert werden. 

Es wird davon ausgegangen, dass Sie bereits über eine nicht verschlüsselte LUN verfügen, die mit dem Host verbunden ist. Falls dies nicht der Fall ist, gehen Sie gemäß den Anweisungen vor, die für Ihr Betriebssystem am besten geeignet sind, um die folgende Task auszuführen:

- [Unter Linux auf {{site.data.keyword.blockstorageshort}} zugreifen](accessing_block_storage_linux.html)
- [Unter Windows auf {{site.data.keyword.blockstorageshort}} zugreifen](accessing-block-storage-windows.html)

 
## Erstellen Sie eine neue {{site.data.keyword.blockstorageshort}}-Instanz.

**WICHTIG**: Wenn Sie einen Auftrag mit einer API erteilen, geben Sie das Paket 'Storage as a Service' an, um sicherzustellen, dass Sie die aktualisierten Funktionen mit dem neuen Speicher erhalten.

Die folgenden Anweisungen sind für die Bestellung einer erweiterten LUN über die Benutzerschnittstelle vorgesehen. Die neue LUN muss dieselbe Größe wie der Originaldatenträger aufweisen oder größer als der Originaldatenträger sein, damit die Migration möglich ist.

### Endurance für LUN bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur > Speicher > {{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.blockstorageshort}} bestellen**.
3. Wählen Sie **Endurance** in der Liste **Speichertyp auswählen** aus.
4. Wählen Sie Ihre Bereitstellungs**position** (Rechenzentrum) aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie der frühere Datenträger hinzugefügt wird.
5. Wählen Sie eine Abrechnungsoption aus. Sie können eine stündlich oder monatliche Abrechnung auswählen.
6. Wählen Sie das IOPS-Tier aus.
7. Klicken Sie auf *Speichergröße auswählen** und wählen Sie die Speichergröße aus der Liste aus.
8. Klicken Sie auf **Größe des Snapshotbereichs angeben** und wählen Sie die Snapshotgröße aus der Liste aus. Dieser Speicherplatz wird zusätzlich zu Ihrem verwendbaren Speicherplatz hinzugefügt. Überlegungen und Empfehlungen zum Snapshotbereich finden Sie im Abschnitt [Snapshots bestellen](ordering-snapshots.html).
9. Wählen Sie in der Liste Ihren **Betriebssystemtyp** aus.
10. Klicken Sie auf **Weiter**. Ihnen werden die monatlichen und die anteiligen Gebühren angezeigt, sodass Sie die Details der Bestellung prüfen können.
11. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Auftrag erteilen**.

### Performance für LUN bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Speicher** und danach auf **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur > Speicher > {{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der rechten oberen Ecke auf **{{site.data.keyword.blockstorageshort}} bestellen**.
3. Wählen Sie in der Dropdown-Liste **Speichertyp auswählen** den Eintrag **Performance** aus.
4. Klicken Sie auf die Dropdown-Liste **Position** und wählen Sie Ihr Rechenzentrum aus.
   - Stellen Sie sicher, dass der neue Speicher an derselben Position wie die früher bestellten Hosts hinzugefügt wird.
5. Wählen Sie eine Abrechnungsoption aus. Sie können eine stündlich oder monatliche Abrechnung auswählen.
6. Wählen Sie das Optionsfeld neben der entsprechenden **Speichergröße** aus.
7. Geben Sie im Feld **IOPS angeben** die IOPS ein.
8. Klicken Sie auf **Weiter**. Ihnen werden die monatlichen und die anteiligen Gebühren angezeigt, sodass Sie die Details der Bestellung prüfen können. Klicken Sie auf **Zurück**, wenn Sie Ihre Bestellung ändern wollen.
9. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf die Schaltfläche **'Auftrag erteilen'.


Der Speicher wird in weniger als einer Minute bereitgestellt und im {{site.data.keyword.slportal}} auf der Seite '{{site.data.keyword.blockstorageshort}}' angezeigt.


 
## Neuen {{site.data.keyword.blockstorageshort}} mit Host verbinden

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriffsrechte auf einen Datenträger erteilt wurden. Ohne die Hostberechtigung können Sie über Ihr System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf den Datenträger werden der Benutzername, das Kennwort und der IQN (iSCSI Qualified Name) generiert, die zum Bereitstellen der MPIO-iSCSI-Verbindung (MPIO - Multipath I/O) erforderlich sind.

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und klicken Sie auf den Namen Ihrer LUN.

2. Blättern Sie zu **Autorisierte Hosts**.

3. Klicken Sie auf der rechten Seite auf **Host autorisieren**. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können.

 
## Snapshots und Replikation

Haben Sie Snapshots und Replikation für Ihre Original-LUN eingerichtet? Falls ja, müssen Sie für die neue LUN eine Replikation und einen Snapshotbereich konfigurieren und Snapshotpläne erstellen; dabei müssen Sie dieselben Einstellungen verwenden, die auch auf den Originaldatenträger angewendet wurden. 

Beachten Sie Folgendes: Falls das Rechenzentrum des Replikationsziels nicht für die Verschlüsselung aktualisiert wurde, können Sie für den neuen Datenträger erst dann eine Replikation erstellen, wenn für das Rechenzentrum ein Upgrade durchgeführt wurde.

 
## Daten migrieren

Sie müssen über eine Verbindung zum Originaldatenträger und zu den neuen {{site.data.keyword.blockstorageshort}}-LUNs verfügen. 
- Wenn Sie Unterstützung bei der Herstellung der Verbindung der beiden LUNs zu Ihrem Host benötigen, öffnen Sie ein Support-Ticket.

### Überlegungen zu den Daten

An diesem Zeitpunkt müssen Sie berücksichtigen, welchen Datentyp die {{site.data.keyword.blockstorageshort}}-Original-LUN aufweist und wie Sie die Daten am besten auf die neue LUN kopieren. Wenn Sie Sicherungen, statische Inhalte und Dinge haben, bei denen beim Kopieren keine Änderungen zu erwarten sind, brauchen Sie keine großen Überlegungen anzustellen.

Wenn Sie auf dem {{site.data.keyword.blockstorageshort}} eine Datenbank oder virtuelle Maschine ausführen, stellen Sie sicher, dass die Daten während des Kopiervorgangs nicht geändert werden, um eine Beschädigung von Daten zu vermeiden. Wenn Sie Sorgen wegen der Bandbreite haben, sollten Sie die Migration außerhalb der Stoßzeiten durchführen. Wenn Sie bei diesen Überlegungen Unterstützung benötigen, öffnen Sie ein Support-Ticket.
 
### Microsoft Windows

Formatieren Sie zum Kopieren der Daten von Ihrer {{site.data.keyword.blockstorageshort}}-Original-LUN zur neuen LUN den neuen Speicher und kopieren Sie die Dateien mit Windows Explorer.

 
### Linux

Sie können die Daten mit rsync kopieren. Nachstehend finden Sie einen Beispielbefehl:

```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

Es empfiehlt sich, den obigen Befehl einmal mit dem Flag `--dry-run` zu verwenden, um sicherzustellen, dass die Pfade korrekt ausgerichtet sind. Wenn dieser Prozess unterbrochen wird, ist es ratsam, die zuletzt kopierte Zieldatei zu löschen, um sicherzustellen, dass sie vom Anfang zur neuen Position kopiert wird.

Sobald dieser Befehl ohne das Flag `--dry-run` ausgeführt wird, müssen Ihre Daten auf die neue {{site.data.keyword.blockstorageshort}}-LUN kopiert werden. Blättern Sie aufwärts und führen Sie den Befehl noch einmal aus, um sicherzustellen, dass nichts vergessen wurde. Sie können die beiden Positionen auch manuell prüfen, um möglicherweise fehlende Elemente aufzufinden.

Wenn Ihre Migration abgeschlossen ist, können Sie die Produktion auf die neue LUN verschieben. Danach können Sie die Original-LUN aus der Konfiguration abhängen und löschen. Beachten Sie, dass bei der Löschung auch alle Snapshots oder Replikate von der Zielsite gelöscht werden, die der Original-LUN zugeordnet waren.
