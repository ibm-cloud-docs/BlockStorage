---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# {{site.data.keyword.blockstorageshort}} für Sicherung mit cPanel konfigurieren

In diesem Artikel werden Anweisungen zum Konfigurieren Ihrer Sicherungen in cPanel zum Speichern in {{site.data.keyword.blockstoragefull}} bereitgestellt. Dabei wird angenommen, dass root- oder sudo SSH- sowie ein vollständiger WHM-Zugriff (WHM = WebHost Manager) verfügbar ist. Die Anweisungen basieren auf einem **CentOS 7**-Host.

**Hinweis**: Die Dokumentation zu cPanel zum **Konfigurieren eines Sicherungsverzeichnisses** finden Sie [hier](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Stellen Sie über SSH eine Verbindung zu dem Host her.

2. Stellen Sie sicher, dass ein Mountpunktziel vorhanden ist. <br />
   **Hinweis**: Standardmäßig speichert das cPanel-System Sicherungsdateien lokal im Verzeichnis `/backup`. Im vorliegenden Dokument wird angenommen, dass `/backup` vorhanden ist und Sicherungen enthält. Verwenden Sie daher `/backup2` als neuen Mountpunkt.
   
3. Konfigurieren Sie Ihre {{site.data.keyword.blockstorageshort}}-Instanz entsprechend der Beschreibung im Abschnitt [Verbindung zu MPIO iSCSI LUNs unter Linux herstellen](accessing_block_storage_linux.html). Achten Sie darauf, sie an `/backup2` anzuhängen und in `/etc/fstab` zu konfigurieren, um das Anhängen beim Booten zu ermöglichen.

4. **Optional**: Kopieren Sie die vorhandenen Sicherungen in den neuen Speicher. Sie können `rsync` verwenden:
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Hinweis**: Dieser Befehl überträgt Ihre Daten in komprimierter Form, wobei möglichst viel (außer festen Verbindungen) beibehalten wird. Es werden Informationen zu den Dateien, die übertragen werden, und am Ende eine kurze Zusammenfassung bereitgestellt.
    
5. Melden Sie sich bei WebHost Manager an und navigieren Sie über **Home** > **Sicherung** > **Sicherungskonfiguration** zur Sicherungskonfiguration.

6. Bearbeiten Sie die Konfiguration so, dass die Sicherungen an dem neuen Mountpunkt gespeichert werden. 
    - Ändern Sie das standardmäßige Sicherungsverzeichnis, indem Sie anstelle des Verzeichnisses /backup/ den absoluten Pfad zu der neuen Position eingeben. 
    - Wählen Sie die Option **Anhängen eines Sicherungslaufwerks aktivieren**. Diese Einstellung veranlasst den Sicherungskonfigurationsprozess, in der Datei `/etc/fstab` einen Sicherungsmount (`/backup2`) zu suchen. <br /> **Hinweis**: Wenn ein Mount mit demselben Namen wie das Staging-Verzeichnis vorhanden ist, hängt der Sicherungskonfigurationsprozess das Laufwerk an und sichert die Informationen auf diesem Laufwerk. Nach Abschluss des Sicherungsprozesses wird das Laufwerk abgehängt. 

7. Wenden Sie die Änderungen an, indem Sie unten in der Schnittstelle **Sicherungskonfiguration** auf **Konfiguration speichern** klicken.

8. **Optional**: Entfernen Sie je nach Ihrem konkreten Anwendungsfall und Ihren Geschäftsanforderungen den alten Speicher vom Server und brechen Sie das Konto ab.

