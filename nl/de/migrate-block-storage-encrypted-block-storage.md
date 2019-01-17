---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-08"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Upgrade für vorhandenen {{site.data.keyword.blockstorageshort}} auf erweiterten {{site.data.keyword.blockstorageshort}} durchführen

In ausgewählten Rechenzentren ist jetzt erweiterter {{site.data.keyword.blockstoragefull}} verfügbar. Wenn Sie eine Liste der aktualisierten Rechenzentren und verfügbaren Funktionen anzeigen möchten, zum Beispiel konfigurierbare IOPS-Raten und erweiterbare Datenträger, klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html). Weitere Informationen zum vom Provider verwalteten verschlüsselten Speicher finden Sie unter [{{site.data.keyword.blockstorageshort}}-Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html).

Der bevorzugte Migrationspfad ist die gleichzeitige Verbindung zu beiden LUNs und die direkte Übertragung der Daten von einer LUN zur anderen. Die jeweiligen technischen Daten hängen vom Betriebssystem ab und ob erwartet wird, dass die Daten während der Kopieroperation geändert werden.

Es wird davon ausgegangen, dass Sie bereits über eine nicht verschlüsselte LUN verfügen, die mit dem Host verbunden ist. Falls dies nicht der Fall ist, gehen Sie gemäß den Anweisungen vor, die für Ihr Betriebssystem am besten geeignet sind, um die folgende Task auszuführen:

- [Verbindung zu iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html)
- [Verbindung zu iSCSI-LUNs unter CloudLinux herstellen](configure-iscsi-cloudlinux.html)
- [Verbindung zu iSCSI-LUNS unter Microsoft Windows herstellen](accessing-block-storage-windows.html)

Alle erweiterten {{site.data.keyword.blockstorageshort}}-Datenträger, die in diesen Rechenzentren bereitgestellt werden, verfügen über einen anderen Mountpunkt als nicht verschlüsselte Datenträger. Um sicherzustellen, dass Sie für beide Speicherdatenträger den richtigen Mountpunkt verwenden, können Sie die Mountpunktinformationen auf der Seite **Datenträgerdetails** in der Konsole anzeigen. Sie können auf den korrekten Mountpunkt auch über einen API-Aufruf zugreifen: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

## {{site.data.keyword.blockstorageshort}} erstellen

Wenn Sie einen Auftrag mit einer API erteilen, geben Sie das Paket 'Storage as a Service' an, um sicherzustellen, dass Sie die aktualisierten Funktionen mit dem neuen Speicher erhalten.
{:important}

Sie können eine erweiterte LUN über die IBM Cloud-Konsole und das {{site.data.keyword.slportal}} bestellen. Die neue LUN muss dieselbe Größe wie der Originaldatenträger aufweisen oder größer als der Originaldatenträger sein, damit die Migration möglich ist.

- [{{site.data.keyword.blockstorageshort}} mit vordefinierten IOPS-Tiers bestellen (Endurance)](provisioning-block_storage.html#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [{{site.data.keyword.blockstorageshort}} mit angepassten IOPS-Tiers bestellen (Performance)](provisioning-block_storage.html#ordering-block-storage-with-custom-iops-performance-)

Der neue Speicher ist in einigen Minuten zum Anhängen verfügbar. Er kann in der Ressourcenliste und der {{site.data.keyword.blockstorageshort}}-Liste angezeigt werden. 

## Neuen {{site.data.keyword.blockstorageshort}} mit Host verbinden

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf einen Datenträger erteilt wurden. Ohne die Hostberechtigung können Sie über das System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf den Datenträger werden der Benutzername, das Kennwort und der IQN (iSCSI Qualified Name) generiert, die zum Bereitstellen der MPIO-iSCSI-Verbindung (MPIO - Multipath I/O) erforderlich sind.

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und klicken Sie auf den Namen Ihrer LUN.
2. Blättern Sie zu **Autorisierte Hosts**.
3. Klicken Sie auf der rechten Seite auf **Host autorisieren**. Wählen Sie die Hosts aus, die auf den Datenträger zugreifen können.


## Snapshots und Replikation

Haben Sie Snapshots und Replikation für Ihre Original-LUN eingerichtet? Falls ja, müssen Sie für die neue LUN eine Replikation und einen Snapshotbereich konfigurieren und Snapshotpläne erstellen; dabei müssen Sie dieselben Einstellungen verwenden, die auch auf den Originaldatenträger angewendet wurden.

Falls das Rechenzentrum des Replikationsziels noch nicht aktualisiert wurde, können Sie für den neuen Datenträger erst eine Replikation erstellen, wenn für das Rechenzentrum ein Upgrade durchgeführt wurde.


## Daten migrieren

1. Stellen Sie eine Verbindung zu den ursprünglichen und neuen {{site.data.keyword.blockstorageshort}}-LUNs her.

   Wenn Sie Hilfe bei der Verbindung der beiden LUNs zu Ihrem Host benötigen, öffnen Sie einen Support-Fall.
   {:tip}

2. Beachten Sie, welchen Datentyp die ursprüngliche {{site.data.keyword.blockstorageshort}}-LUN aufweist und wie Sie die Daten am besten auf die neue LUN kopieren.
  - Wenn Sie über Sicherungen, statischen Inhalt und andere Daten verfügen, bei denen beim Kopieren keine Änderungen zu erwarten sind, sind keine größeren Überlegungen erforderlich. 
  - Wenn Sie auf dem {{site.data.keyword.blockstorageshort}} eine Datenbank oder virtuelle Maschine ausführen, stellen Sie sicher, dass die Daten während des Kopiervorgangs nicht geändert werden, um eine Beschädigung von Daten zu vermeiden. 
  - Wenn Sie Sorgen wegen der Bandbreite haben, führen Sie die Migration außerhalb der Stoßzeiten durch. 
  - Wenn Sie bei diesen Überlegungen Unterstützung benötigen, öffnen Sie einen Support-Fall. 

3. Kopieren Sie die Daten.
   - Formatieren Sie für **Microsoft Windows** den neuen Speicher und kopieren Sie die Daten aus der {{site.data.keyword.blockstorageshort}}-Original-LUN in Ihre neue LUN, indem Sie Windows Explorer verwenden.
   - Für **Linux** können Sie `rsync` verwenden, um die Daten zu kopieren.
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   Es empfiehlt sich, den obigen Befehl einmal mit dem Flag `--dry-run` zu verwenden, um sicherzustellen, dass die Pfade korrekt ausgerichtet sind. Wenn dieser Prozess unterbrochen wird, können Sie die zuletzt kopierte Zieldatei löschen, um sicherzustellen, dass sie vom Anfang zur neuen Position kopiert wird.<br/>
   Sobald dieser Befehl ohne das Flag `--dry-run` ausgeführt wird, werden die Daten auf die neue {{site.data.keyword.blockstorageshort}}-LUN kopiert. Führen Sie den Befehl noch einmal aus, um sicherzustellen, dass nichts vergessen wurde. Sie können die beiden Positionen auch manuell prüfen, um möglicherweise fehlende Elemente aufzufinden.<br/>
   Wenn die Migration abgeschlossen ist, können Sie die Produktion auf die neue LUN verschieben. Danach können Sie die Original-LUN aus der Konfiguration abhängen und löschen. Durch den Löschvorgang werden auch alle Snapshots oder Replikate auf dem Zielstandort gelöscht, die der Original-LUN zugeordnet waren.
