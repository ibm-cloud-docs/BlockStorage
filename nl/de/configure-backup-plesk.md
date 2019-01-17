---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} für Sicherung mit Plesk konfigurieren

Sie können die folgenden Anweisungen zum Konfigurieren von {{site.data.keyword.blockstoragefull}} für Sicherungen in Plesk verwenden. Dabei wird angenommen, dass root- oder sudo SSH- sowie ein vollständiger Plesk-Zugriff auf Administratorebene verfügbar ist. Diese Anweisungen basieren auf einem CentOS 7-Host.

Weitere Informationen finden Sie in der Dokumentation von Plesk zum Thema Sicherheit und Wiederherstellung unter [Plesk's documentation for backing up and restoration ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Stellen Sie über SSH eine Verbindung zu dem Host her.
2. Stellen Sie sicher, dass ein Mountpunktziel vorhanden ist.

   Plesk bietet zwei Optionen zum Speichern von Sicherungen. Die eine ist der interne Plesk-Speicher (ein Sicherungsspeicher auf dem Plesk-Server). Die andere ist ein externer FTP-Speicher (ein Sicherungsspeicher, der sich auf einem externen Server im Web oder im lokalen Netz befindet). In der Regel werden interne Sicherungen in Plesk-Fenstern im Pfad `/var/lib/psa/dumps` gespeichert und verwenden `/tmp` als temporäres Verzeichnis. Im vorliegenden Beispiel wird das temporäre Verzeichnis lokal beibehalten, das Speicherauszugsverzeichnis jedoch auf das {{site.data.keyword.blockstorageshort}}-Ziel (`/backup/psa/dumps`) verschoben. Es sind keine FTP-Benutzerberechtigungsnachweise erforderlich.
   {:note}   
3. Konfigurieren Sie Ihre {{site.data.keyword.blockstorageshort}}-Instanz entsprechend der Beschreibung im Abschnitt [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html). Hängen Sie {{site.data.keyword.blockstorageshort}} an `/backup` an und konfigurieren Sie `/etc/fstab`, um das Anhängen beim Starten zu ermöglichen.
4. **Optional**: Kopieren Sie die vorhandenen Sicherungen in den neuen Speicher. Sie können `rsync` verwenden.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    Dieser Befehl komprimiert und überträgt die Daten, wobei möglichst viel (außer festen Verbindungen) beibehalten wird. Es werden Informationen zu den Dateien, die übertragen werden, und am Ende eine kurze Zusammenfassung bereitgestellt.
    {:tip}    
5. Bearbeiten Sie `/etc/psa/psa.conf` so, dass der Wert für `DUMP_D` auf das neue Ziel verweist.
    - Es wird wie folgt angezeigt: `DUMP_D /backup/psa/dumps`.
6. **Optional**: Entfernen Sie je nach Ihrem konkreten Anwendungsfall und Ihren Geschäftsanforderungen den alten Speicher vom Server und brechen Sie das Konto ab.
