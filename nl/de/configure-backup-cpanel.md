/---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

Schlüsselwörter: Blockspeicher, cPanel, backups, Mountpunkt, ISCSI

Untergeordnete Sammlung: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} für Sicherung mit cPanel konfigurieren
{: #cPanelBackups}

Sie können die folgenden Anweisungen zum Konfigurieren der Sicherungen in cPanel für das Speichern im {{site.data.keyword.blockstoragefull}}. Dabei wird angenommen, dass root- oder sudo SSH- sowie ein vollständiger WHM-Zugriff (WHM = WebHost Manager) verfügbar ist. Diese Anweisungen basieren auf einem **CentOS 7**-Host.

Weitere Informationen finden Sie in [cPanel - Configuring backup directory](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external}.
{:tip}

1. Stellen Sie über SSH eine Verbindung zu dem Host her.

2. Stellen Sie sicher, dass ein Mountpunktziel vorhanden ist. <br />
   Standardmäßig speichert das cPanel-System Sicherungsdateien lokal im Verzeichnis `/backup`. Im vorliegenden Dokument wird angenommen, dass `/backup` vorhanden ist und Sicherungen enthält. Verwenden Sie daher `/backup2` als neuen Mountpunkt.
   {:note}

3. Konfigurieren Sie Ihre {{site.data.keyword.blockstorageshort}}-Instanz entsprechend der Beschreibung im Abschnitt [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Achten Sie darauf, sie an `/backup2` anzuhängen und in `/etc/fstab` zu konfigurieren, um das Anhängen beim Booten zu ermöglichen.

4. **Optional**: Kopieren Sie die vorhandenen Sicherungen in den neuen Speicher. Sie können `rsync` verwenden.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Dieser Befehl komprimiert und überträgt Ihre Daten, während er (abgesehen von festen Verbindungen) so viel wie möglich beibehält. Es werden Informationen zu den Dateien, die übertragen werden, und am Ende eine kurze Zusammenfassung bereitgestellt.
    {:tip}

5. Melden Sie sich an WHM an und navigieren Sie über **Home** > **Sicherung** > **Sicherungskonfiguration** zur Sicherungskonfiguration.

6. Bearbeiten Sie die Konfiguration so, dass die Sicherungen an dem neuen Mountpunkt gespeichert werden.
    - Ändern Sie das standardmäßige Sicherungsverzeichnis, indem Sie anstelle des Verzeichnisses /backup/ den absoluten Pfad zu der neuen Position eingeben.
    - Wählen Sie die Option **Anhängen eines Sicherungslaufwerks aktivieren**. Diese Einstellung veranlasst den Sicherungskonfigurationsprozess, in der Datei `/etc/fstab` einen Sicherungsmount (`/backup2`) zu suchen. <br />

    Falls ein Mount mit demselben Namen wie das Staging-Verzeichnis vorhanden ist, hängt der Sicherungskonfigurationsprozess das Laufwerk an und sichert die Informationen auf dem Laufwerk. Nach Abschluss des Sicherungsprozesses wird das Laufwerk abgehängt.
    {:note}

7. Wenden Sie die Änderungen durch Klicken auf **Konfiguration speichern** an.

8. **Optional**: Entfernen Sie je nach Ihrem konkreten Anwendungsfall und Ihren Geschäftsanforderungen den alten Speicher vom Server und brechen Sie das Konto ab.
