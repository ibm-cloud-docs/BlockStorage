---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} auf verschlüsselten {{site.data.keyword.blockstorageshort}} migrieren

In ausgewählten Rechenzentren ist nun verschlüsselter {{site.data.keyword.blockstoragefull}} für Endurance oder Performance verfügbar. Nachstehend finden Sie Informationen zum Migrieren von nicht verschlüsseltem auf verschlüsselten {{site.data.keyword.blockstorageshort}}. Weitere Informationen zum anbietergesteuerten verschlüsselten Speicher finden Sie im Artikel [{{site.data.keyword.blockstorageshort}} - Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html). Klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html), um eine Liste der aktualisierten Rechenzentren und der verfügbaren Funktionen anzuzeigen.

Das bevorzugte Migrationsverfahren besteht darin, gleichzeitig eine Verbindung zu beiden LUNs herzustellen und Daten direkt von einer Datei-LUN an eine andere zu übertragen. Die konkreten Schritte hängen von Ihrem Betriebssystem sowie davon ab, ob zu erwarten ist, dass die Daten während der Kopieroperation geändert werden.

Die häufigeren Szenarios werden zur Veranschaulichung skizziert. Es wird angenommen, dass Sie Ihre nicht verschlüsselte Datei-LUN bereits an Ihren Host angehängt haben. Wenn nicht, folgen Sie den nachstehenden Anweisungen, die am besten zu Ihrem Betriebssystem passen, um diese Task auszuführen.

- [Unter Linux auf {{site.data.keyword.blockstorageshort}} zugreifen](accessing_block_storage_linux.html)

- [Unter Windows auf {{site.data.keyword.blockstorageshort}} zugreifen](accessing-block-storage-windows.html)

 
## Verschlüsselte LUN erstellen

Erstellen Sie mithilfe der folgenden Schritte eine LUN derselben Größe oder größer, die verschlüsselt ist, um den Migrationsprozess zu erleichtern.
Verschlüsselte Endurance-Speicher-LUN bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf der auf der Homepage auf **Speicher** > **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.

2. Klicken Sie auf der Seite '{{site.data.keyword.blockstorageshort}}' auf den Link **{{site.data.keyword.blockstorageshort}} bestellen**.

3. Wählen Sie **Endurance** aus.

4. Wählen Sie das Rechenzentrum aus, in dem sich Ihre Original-LUN befindet. <br/> **Hinweis**: Die Verschlüsselung ist nur in ausgewählten Rechenzentren verfügbar.

5. Wählen Sie das gewünschte IOPS-Tier aus.

6. Wählen Sie die gewünschte Speichermenge in GB aus. Bei TB entspricht 1 TB 1.000 GB und 12 TB entspricht 12.000 GB.

7. Geben Sie die gewünschte Speichermenge für Snapshots in GB ein. 

8. Wählen Sie in der Dropdown-Liste das VMware-Betriebssystem aus.

9. Übergeben Sie die Bestellung.

## Verschlüsselte Performance-Speicher-LUN bestellen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf der auf der Homepage auf **Speicher** > **{{site.data.keyword.blockstorageshort}}** ODER klicken Sie im {{site.data.keyword.BluSoftlayer_full}}-Katalog auf **Infrastruktur** > **Speicher** > **{{site.data.keyword.blockstorageshort}}**.

2. Klicken Sie auf **{{site.data.keyword.blockstorageshort}} bestellen**.

3. Wählen Sie **Performance** aus.

4. Wählen Sie das Rechenzentrum aus, in dem sich Ihre Original-LUN befindet. Beachten Sie, dass die Verschlüsselung nur in den mit einem Stern (`*`) gekennzeichneten Rechenzentren verfügbar ist.

5. Wählen Sie die gewünschte Speichermenge in GB aus. Bei TB entspricht 1 TB 1.000 GB und 12 TB entspricht 12.000 GB.

6. Geben Sie die gewünschte IOPS-Menge in Intervallen von 100 an.

7. Wählen Sie in der Dropdown-Liste das VMware-Betriebssystem aus.

8. Übergeben Sie die Bestellung.

Der Speicher wird in weniger als einer Minute bereitgestellt und im {{site.data.keyword.slportal}} auf der Seite '{{site.data.keyword.blockstorageshort}}' angezeigt.

 
## Neuen Datenträger mit Host verbinden

'Autorisierte' Hosts sind Hosts, denen Zugriffsrechte für einen Datenträger erteilt wurden. Ohne eine Hostberechtigung können Sie nicht auf Speicher Ihres Systems zugreifen oder diesen verwenden. Durch das Autorisieren eines Hosts für den Zugriff auf Ihren Datenträger werden der Benutzername, das Kennwort und der qualifizierte iSCSI-Name (IQN) generiert, die zum Anhängen der MPIO-iSCSI-Verbindung erforderlich sind.

1. Klicken Sie auf **Speicher**  > **{{site.data.keyword.blockstorageshort}}** und anschließend auf Ihren LUN-Namen.

2. Blättern Sie zum Bereich **Autorisierte Hosts** der Seite.

3. Klicken Sie rechts auf der Seite auf den Link **Host autorisieren**. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können.

 
## Snapshots und Replikation

Haben Sie Snapshots und Replikation für Ihre Original-LUN eingerichtet? Wenn ja, müssen Sie die Replikation und den Snapshotbereich einrichten sowie Snapshotpläne für die neu verschlüsselte LUN mit denselben Einstellungen wie der Originaldatenträger erstellen. 

Beachten Sie Folgendes: Wenn das Zielrechenzentrum Ihrer Replikation nicht für die Verschlüsselung aktualisiert wurde, können Sie die Replikation für den verschlüsselten Datenträger erst einrichten, nachdem dieses Rechenzentrum aktualisiert wurde.

 
## Daten migrieren

Sie sollten sowohl mit Ihren Original-LUNs als auch mit den verschlüsselten {{site.data.keyword.blockstorageshort}}-LUNs verbunden sein. 
- Wenn nicht, müssen Sie sicherstellen, dass Sie sowohl die oben aufgeführten als auch die in den anderen Beiträgen beschriebenen Schritte ordnungsgemäß ausgeführt haben. Öffnen Sie ein Support-Ticket zur Unterstützung beim Verbinden der beiden LUNs.

### Überlegungen zu den Daten

Hier empfiehlt es sich zu überlegen, welche Art von Daten sich auf Ihrer Original-{{site.data.keyword.blockstorageshort}}-LUN befinden und wie diese am besten auf Ihre verschlüsselte LUN zu kopieren sind. Wenn Sie Sicherungen, statische Inhalte und Dinge haben, bei denen beim Kopieren keine Änderungen zu erwarten sind, brauchen Sie keine großen Überlegungen anzustellen.

Wenn Sie eine Datenbank oder eine virtuelle Maschine auf Ihrem {{site.data.keyword.blockstorageshort}} ausführen, müssen Sie sicherstellen, dass die Daten auf der Original-LUN beim Kopieren nicht geändert werden, damit keine Beschädigung auftritt. Wenn Sie Sorgen wegen der Bandbreite haben, sollten Sie die Migration außerhalb der Stoßzeiten durchführen. Wenn Sie bei diesen Überlegungen Unterstützung benötigen, öffnen Sie ein Support-Ticket.
 
### Microsoft Windows

Formatieren Sie zum Kopieren von Daten von Ihrer Original-{{site.data.keyword.blockstorageshort}}-LUN auf Ihre verschlüsselte LUN den neuen Speicher und kopieren Sie die Dateien mit Windows Explorer.

 
### Linux

Sie können die Daten mit rsync kopieren. Nachstehend finden Sie einen Beispielbefehl:

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

Es empfiehlt sich, den obigen Befehl einmal mit dem Flag --dry-run zu verwenden, um sicherzustellen, dass die Pfade korrekt ausgerichtet sind. Wenn dieser Prozess unterbrochen wird, ist es ratsam, die zuletzt kopierte Zieldatei zu löschen, um sicherzustellen, dass sie vom Anfang der neuen Position kopiert wird.

Sobald dieser Befehl ohne das Flag --dry-run ausgeführt wurde, sollten Ihre Daten auf die verschlüsselte {{site.data.keyword.blockstorageshort}}-LUN kopiert werden. Sie sollten aufwärts blättern und den Befehl noch einmal ausführen, um sicherzustellen, dass nichts vergessen wurde. Sie können die beiden Positionen auch manuell prüfen, um möglicherweise fehlende Elemente aufzufinden.

Wenn Ihre Migration abgeschlossen ist, können Sie die Produktion auf die verschlüsselte LUN verschieben und Ihre Original-LUN aus Ihrer Konfiguration abhängen und löschen. Beachten Sie, dass bei der Löschung auch alle Snapshots oder Replikate von der Zielsite gelöscht werden, die der Original-LUN zugeordnet waren.
