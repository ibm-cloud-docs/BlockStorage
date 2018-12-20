---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} verwalten

Sie können Ihre {{site.data.keyword.blockstoragefull}}-Datenträger über das [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} verwalten.

## Details zu {{site.data.keyword.blockstorageshort}}-LUN anzeigen

Sie können eine Zusammenfassung der wichtigsten Informationen für die ausgewählte Speicher-LUN einschließlich zusätzlicher Snapshot- und Replikationsfunktionen anzeigen, die zum Speicher hinzugefügt wurden.

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der Liste auf den entsprechenden LUN-Namen.

## Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} autorisieren

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf eine bestimmte LUN erteilt wurden. Ohne die Hostberechtigung können Sie über das System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf die LUN werden der Benutzername, das Kennwort und der IQN (iSCSI Qualified Name) generiert, die zum Bereitstellen der MPIO-iSCSI-Verbindung (MPIO - Multipath I/O) erforderlich sind.

Sie können Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Sie können mehrere Konten haben, Sie können jedoch nicht einen Host über das eine Konto für den Zugriff auf Ihren Speicher in einem anderen Konto berechtigen.
{:important}

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und klicken Sie auf den Namen Ihrer LUN.
2. Blättern Sie zum Bereich **Autorisierte Hosts** der Seite.
3. Klicken Sie auf der rechten Seite auf **Host autorisieren**. Wählen Sie die Hosts aus, die auf die entsprechende LUN zugreifen können.



## Liste der Hosts anzeigen, die für Zugriff auf {{site.data.keyword.blockstorageshort}}-LUN autorisiert sind

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und klicken Sie auf den Namen Ihrer LUN.
2. Blättern Sie abwärts zum Abschnitt **Autorisierte Hosts**.

Dort wird eine Liste der Hosts angezeigt, die derzeit für den Zugriff auf die LUN berechtigt sind. Außerdem werden die Authentifizierungsdaten aufgeführt, die zum Herstellen einer Verbindung erforderlich sind - Benutzername, Kennwort und IQN-Host. Die Zieladresse wird auf der Seite **Speicherdetails** aufgelistet. Die Zieladresse wird für NFS als DNS-Name beschrieben, für iSCSI ist es die IP-Adresse von 'Zielportal ermitteln'.



## {{site.data.keyword.blockstorageshort}} anzeigen, für den ein Host autorisiert ist

Sie können die LUNs anzeigen, auf die ein Host Zugriff hat, sowie die Informationen, die zum Herstellen einer Verbindung erforderlich sind – LUN-Name, Speichertyp, Zieladresse, Kapazität und Position:

1. Klicken Sie auf **Geräte** -> **Geräteliste** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](http://control.softlayer.com/){:new_window} und klicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.

Es wird eine Liste der Speicher-LUNs angezeigt, auf die dieser Host zugreifen kann. Die Liste ist nach Speichertypen gruppiert (Blockspeicher, Dateispeicher, etc.). Durch Klicken auf **Aktionen** können Sie weiteren Speicher autorisieren oder Zugriff widerrufen.



## {{site.data.keyword.blockstorageshort}} anhängen und abhängen

Befolgen Sie auf der Basis des Betriebssystems Ihres Hosts die entsprechenden Anweisungen.

- [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html)
- [Verbindung zu MPIO-iSCSI-LUNs unter CloudLinux herstellen](configure-iscsi-cloudlinux.html)
- [Verbindung zu MPIO-iSCSI-LUNS unter Microsoft Windows herstellen](accessing-block-storage-windows.html)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](configure-backup-cpanel.html)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](configure-backup-plesk.html)


## Zugriff eines Hosts auf {{site.data.keyword.blockstorageshort}} widerrufen

Wenn Sie den Zugriff eines Hosts auf eine bestimmte Speicher-LUN stoppen möchten, können Sie den Zugriff widerrufen. Wenn der Zugriff widerrufen wird, wird die Hostverbindung zur LUN gelöscht. Das Betriebssystem und die Anwendungen auf diesem Host können nicht mehr mit der LUN kommunizieren.

Um hostseitige Probleme zu vermeiden, hängen Sie die Speicher-LUN von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, sodass keine Laufwerke fehlen oder Daten beschädigt werden.
{:important}

Sie können den Zugriff über die **Geräteliste** oder die **Speicheransicht** widerrufen.

### Zugriff über Geräteliste widerrufen

1. Klicken Sie auf **Geräte** > **Geräteliste** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} und doppelklicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.
3. Es wird eine Liste der Speicher-LUNs angezeigt, auf die dieser Host zugreifen kann. Die Liste ist nach Speichertypen gruppiert (Blockspeicher, Dateispeicher, etc.). Wählen Sie neben dem Namen der LUN **Aktion** aus und klicken Sie auf 'Zugriff widerrufen'**.
4. Bestätigen Sie, dass Sie den Zugriff für eine LUN widerrufen möchten, weil die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den LUN-Zugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

Wenn Sie mehrere LUNS von einem bestimmten Host trennen möchten, müssen Sie die Aktion 'Zugriff widerrufen' für jede LUN wiederholen.
{:tip}


### Zugriff über Speicheransicht widerrufen

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}** und wählen Sie die LUN aus, von der Sie den Zugriff widerrufen wollen.
2. Blättern Sie zu **Autorisierte Hosts**.
3. Klicken Sie auf **Aktionen** neben dem Host, dessen Zugriff Sie widerrufen möchten, und wählen Sie **Zugriff widerrufen** aus.
4. Bestätigen Sie, dass Sie den Zugriff für eine LUN widerrufen möchten, weil die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den LUN-Zugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

Wenn Sie mehrere Host von einer bestimmten LUN trennen möchten, müssen Sie die Aktion 'Zugriff widerrufen' für jeden Host wiederholen.
{:tip}



## Speicher-LUN abbrechen

Wenn Sie eine bestimmte LUN nicht mehr brauchen, können Sie sie jederzeit abbrechen.


Zum Abbrechen einer Speicher-LUN müssen Sie zunächst den Zugriff von allen Hosts widerrufen.
{:important}

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie für die LUN, die storniert werden soll, auf **Aktionen**, und wählen Sie **{{site.data.keyword.blockstorageshort}} stornieren** aus.
3. Geben Sie an, ob die LUN sofort storniert werden soll oder am Stichtag der Bereitstellung der LUN.

   Wenn Sie die Option auswählen, um die LUN an ihrem Stichtag zu stornieren, können Sie die Stornierungsanforderung vor ihrem Stichtag annullieren.
   {:tip}
4. Klicken Sie auf **Weiter** oder **Schließen**.
5. Klicken Sie auf das Kontrollkästchen **Bestätigung** und klicken Sie auf **Bestätigen**.
