---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}

# {{site.data.keyword.blockstorageshort}} verwalten

Sie können Ihre {{site.data.keyword.blockstoragefull}}-Datenträger über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwalten.

## Details zu {{site.data.keyword.blockstorageshort}}-LUN anzeigen

Sie können eine Zusammenfassung der wichtigsten Informationen für die ausgewählte Speicher-LUN einschließlich zusätzlicher Snapshot- und Replikationsfunktionen anzeigen, die zum Speicher hinzugefügt wurden.

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie in der Liste auf den entsprechenden LUN-Namen.

## Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} autorisieren

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf eine bestimmte LUN erteilt wurden. Ohne die Hostberechtigung können Sie über das System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf die LUN werden der Benutzername, das Kennwort und der IQN (iSCSI Qualified Name) generiert, die zum Bereitstellen der MPIO-iSCSI-Verbindung (MPIO - Multipath I/O) erforderlich sind.

**Hinweis:** Sie können Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Wenn Sie über mehrere Konten verfügen, können Sie einen Host nicht über das Konto für den Zugriff auf Ihren Speicher auf einem anderen Konto berechtigen.

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und klicken Sie auf den Namen Ihrer LUN.
2. Blättern Sie zum Bereich **Autorisierte Hosts** der Seite.
3. Klicken Sie auf der rechten Seite auf **Host autorisieren**. Wählen Sie die Hosts aus, die auf die entsprechende LUN zugreifen können.

 

## Liste der Hosts anzeigen, die für Zugriff auf {{site.data.keyword.blockstorageshort}}-LUN autorisiert sind

1. Klicken Sie auf **Speicher** -> **{{site.data.keyword.blockstorageshort}}** und klicken Sie auf den Namen Ihrer LUN.
2. Blättern Sie abwärts zum Abschnitt **Autorisierte Hosts**.

Dort wird eine Liste der Hosts angezeigt, die derzeit für den Zugriff auf die LUN berechtigt sind. Außerdem werden die Authentifizierungsdaten aufgeführt, die zum Herstellen einer Verbindung erforderlich sind - Benutzername, Kennwort und IQN-Host. Die Zieladresse wird auf der Seite **Speicherdetails** aufgelistet. Die Zieladresse wird für NFS als DNS-Name beschrieben, für iSCSI ist es die IP-Adresse von 'Zielportal ermitteln'.

 

## {{site.data.keyword.blockstorageshort}} anzeigen, für den ein Host autorisiert ist

Sie können die LUNs anzeigen, auf die ein Host Zugriff hat, sowie die Informationen, die zum Herstellen einer Verbindung erforderlich sind – LUN-Name, Speichertyp, Zieladresse, Kapazität und Position:

1. Klicken Sie im [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} auf **Geräte** -> **Geräteliste** und klicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.

Es wird eine Liste der Speicher-LUNs angezeigt, auf die dieser Host zugreifen kann. Die Liste ist nach Speichertypen gruppiert (Blockspeicher, Dateispeicher, etc.). Durch Klicken auf **Aktionen** können Sie weiteren Speicher autorisieren oder Zugriff widerrufen.

 

## {{site.data.keyword.blockstorageshort}} anhängen und abhängen

Details hierzu finden Sie in den folgenden Artikeln:

- [{{site.data.keyword.blockstorageshort}} unter Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} unter Microsoft Windows](accessing-block-storage-windows.html)

 

## Zugriff eines Hosts auf {{site.data.keyword.blockstorageshort}} widerrufen

Wenn Sie den Zugriff eines Hosts auf eine bestimmte Speicher-LUN stoppen möchten, können Sie den Zugriff widerrufen. Wenn der Zugriff widerrufen wird, wird die Hostverbindung zur LUN gelöscht. Das Betriebssystem und die Anwendungen auf diesem Host können nicht mehr mit der LUN kommunizieren.

**Hinweis:** Hängen Sie zur Vermeidung von Problemen die Speicher-LUN von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, um fehlende Laufwerke oder eine Datenbeschädigung zu vermeiden.

Sie können den Zugriff über die **Geräteliste** oder die **Speicheransicht** widerrufen.

### Zugriff über Geräteliste widerrufen

1. Klicken Sie im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} auf **Geräte** - **Geräteliste** und doppelklicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.
3. Es wird eine Liste der Speicher-LUNs angezeigt, auf die dieser Host zugreifen kann. Die Liste ist nach Speichertypen gruppiert (Blockspeicher, Dateispeicher, etc.). Wählen Sie neben dem Namen der LUN **Aktion** aus und klicken Sie auf 'Zugriff widerrufen'**.
4. Bestätigen Sie, dass Sie den Zugriff für eine LUN widerrufen möchten, weil die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den LUN-Zugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Hinweis:** Falls Sie die Verbindungen eines bestimmten Hosts zu mehreren LUNs trennen möchten, müssen Sie die Aktion 'Zugriff widerrufen' für jede einzelne LUN wiederholen.


### Zugriff über Speicheransicht widerrufen

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}** und wählen Sie die LUN aus, von der Sie den Zugriff widerrufen wollen.
2. Blättern Sie zu **Autorisierte Hosts**.
3. Klicken Sie auf **Aktionen** neben dem Host, dessen Zugriff Sie widerrufen möchten, und wählen Sie **Zugriff widerrufen** aus.
4. Bestätigen Sie, dass Sie den Zugriff für eine LUN widerrufen möchten, weil die Aktion nicht rückgängig gemacht werden kann. Klicken Sie auf **Ja**, um den LUN-Zugriff zu widerrufen, oder auf **Nein**, um die Aktion abzubrechen.

**Hinweis:** Falls Sie die Verbindungen einer LUN zu mehreren Hosts trennen möchten, müssen Sie die Aktion 'Zugriff widerrufen' für jeden einzelnen Host wiederholen.

 

## Speicher-LUN abbrechen

Wenn Sie eine bestimmte LUN nicht mehr brauchen, können Sie sie abbrechen. Zum Abbrechen einer Speicher-LUN müssen Sie zunächst den Zugriff von allen Hosts widerrufen.

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie für die LUN, die storniert werden soll, auf **Aktionen**, und wählen Sie **{{site.data.keyword.blockstorageshort}} stornieren** aus.
3. Geben Sie an, ob die LUN sofort storniert werden soll oder am Stichtag der Bereitstellung der LUN. 
**Hinweis:** Wenn Sie die Option zum Abbrechen der LUN an ihrem Stichtag auswählen, können Sie die Abbruchanforderung vor ihrem Stichtag annullieren.
4. Klicken Sie auf **Weiter** oder **Schließen**. 
5. Klicken Sie auf das Kontrollkästchen **Bestätigung** und klicken Sie auf **Bestätigen**.
