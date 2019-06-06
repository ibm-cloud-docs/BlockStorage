---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: MPIO iSCSI LUNS, iSCSI Target, MPIO, multipath, block storage, LUN, mounting, mapping secondary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# Verbindung zu iSCSI-LUNS unter Microsoft Windows herstellen
{: #mountingWindows}

Stellen Sie vor Beginn sicher, dass der Host, von dem auf den {{site.data.keyword.blockstoragefull}}-Datenträger zugegriffen wird, im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} autorisiert wurde.

1. Suchen Sie auf der Seite mit der {{site.data.keyword.blockstorageshort}}-Liste den neuen Datenträger und klicken Sie auf **Aktionen**. Klicken Sie auf **Host autorisieren**.
2. Wählen Sie in der Liste den Host oder die Hosts aus, der bzw. die auf den Datenträger zugreifen soll(en), und klicken Sie auf **Abschicken**.

Alternativ dazu können Sie den Host auch über die SL-CLI berechtigen.
```
# slcli block access-authorize --help
Syntax: slcli block access-authorize [OPTIONEN] DATENTRÄGER_ID

Optionen:
  -h, --hardware-id TEXT    ID einer SoftLayer-Hardware zur Berechtigung
  -v, --virtual-id TEXT     ID eines virtuellen SoftLayer-Gastsystems zur Berechtigung
  -i, --ip-address-id TEXT  ID der Teilnetz-IP-Adresse eines SoftLayer-Netzes
                            zur Berechtigung
  --ip-address TEXT         IP-Adresse zur Berechtigung
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

## {{site.data.keyword.blockstorageshort}}-Datenträger anhängen
{: #mountWin}

Nachfolgend werden die Schritte beschrieben, die zum Herstellen einer Verbindung von einer Windows-basierten {{site.data.keyword.cloud}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind (MPIO = Multipath Input/Output; iSCSI = internet Small Computer System Interface; LUN = Logical Unit Number). Das Beispiel basiert auf Windows Server 2012. Die Schritte können für andere Windows-Versionen gemäß der Dokumentation des Anbieters für das Betriebssystem angepasst werden.

### Funktion MPIO konfigurieren

1. Starten Sie den Servermanager und navigieren Sie zu **Verwalten**, **Rollen und Features hinzufügen**.
2. Klicken Sie auf **Weiter**, um das Menü 'Features' zu öffnen.
3. Blättern Sie nach unten und markieren Sie **Multipfad-E/A**.
4. Klicken Sie auf **Installieren**, um MPIO auf dem Host-Server zu installieren.
![Rollen und Funktionen im Servermanager hinzufügen](/images/Roles_Features.png)

### iSCSI-Unterstützung für MPIO hinzufügen

1. Öffnen Sie das Fenster für die MPIO-Eigenschaften, indem Sie auf **Start** klicken, auf **Verwaltung** zeigen und auf **MPIO** klicken.
2. Klicken Sie auf **Multipfade suchen**.
3. Markieren Sie **Unterstützung für iSCSI-Geräte hinzufügen** und klicken Sie auf **Hinzufügen**. Klicken Sie bei der Aufforderung zum Neustart des Computers auf **Ja**.

Bei Windows Server 2008 kann das Microsoft Device Specific Module (MSDSM) durch das Hinzufügen von Unterstützung für iSCSI alle iSCSI-Geräte für MPIO übernehmen, wofür zuerst eine Verbindung zu einem iSCSI-Ziel erforderlich ist.
{:note}

### iSCSI-Initiator konfigurieren

1. Starten Sie den iSCSI-Initiator über den Servermanager und wählen Sie **Tools**, **iSCSI-Initiator** aus.
2. Klicken Sie auf die Registerkarte **Konfiguration**.
    - Das Feld 'Initiatorname' enthält möglicherweise bereits einen Eintrag, der `iqn.1991-05.com.microsoft:` ähnelt.
    - Klicken Sie auf **Ändern**, um die vorhandenen Werte durch Ihren qualifizierten iSCSI-Namen (IQN) zu ersetzen.
    ![Eigenschaften des iSCSI-Initiators](/images/iSCSI.png)

      Der IQN-Name kann aus der Detailanzeige zu {{site.data.keyword.blockstorageshort}} im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} abgerufen werden.
      {: tip}

    - Klicken Sie auf **Erkennung** und anschließend auf **Portal erkennen**.
    - Geben Sie die IP-Adresse Ihres iSCSI-Ziels ein und lassen Sie den Port auf dem Standardwert 3260.
    - Klicken Sie auf **Erweitert**, um das Fenster 'Erweiterte Einstellungen' zu öffnen.
    - Wählen Sie **CHAP-Anmeldung aktivieren**, um die CHAP-Authentifizierung zu aktivieren.
    ![CHAP-Anmeldung aktivieren](/images/Advanced_0.png)

    In den Feldern 'Name' und 'Zielschlüssel' muss die Groß-/Kleinschreibung beachtet werden.
    {:important}
         - Löschen Sie im Feld **Name** alle vorhandenen Einträge und geben Sie den Benutzernamen aus dem [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} ein.
         - Geben Sie im Feld **Geheimer Zielschlüssel** das Kennwort aus dem [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} ein.
    - Klicken Sie auf **OK** in den Fenstern **Erweiterte Einstellungen** und **Zielportal ermitteln**, um zur Hauptanzeige für die Eigenschaften des iSCSI-Initiators zurückzukehren. Überprüfen Sie die Eingaben für den Benutzernamen und das Kennwort, wenn Sie Authentifizierungsfehler erhalten.
    ![Inaktives Ziel](/images/Inactive_0.png)

    Der Name des Ziels im Bereich 'Erkannte Ziele' wird mit dem Status `Inaktiv` angezeigt.
    {:note}


### Ziel aktivieren

1. Klicken Sie auf **Verbinden**, um eine Verbindung zu dem Ziel herzustellen.
2. Wählen Sie das Kontrollkästchen **Multipfad aktivieren** aus, um Multipath IO für das Ziel zu aktivieren.
<br/>
   ![Multipfad aktivieren](/images/Connect_0.png)
3. Klicken Sie auf **Erweitert** und wählen Sie **CHAP-Anmeldung aktivieren** aus.
</br>
   ![CHAP aktivieren](/images/chap_0.png)
4. Geben Sie in das Feld 'Name' den Benutzernamen und in das Feld 'Zielschlüssel' das Kennwort ein.

   Die Werte der Felder 'Name' und 'Zielschlüssel' können aus der Detailanzeige zu {{site.data.keyword.blockstorageshort}} abgerufen werden.
   {:tip}
5. Klicken Sie auf **OK**, bis das Fenster **Eigenschaften des iSCSI-Initiators** angezeigt wird. Der Status des Ziels im Abschnitt **Erkannte Ziele** wechselt von **Inaktiv** zu **Verbunden**.
![Verbundener Status](/images/Connected.png)


### MPIO im iSCSI-Initiator konfigurieren

1. Starten Sie den iSCSI-Initiator und klicken Sie auf der Registerkarte 'Ziele' auf **Eigenschaften**.
2. Klicken Sie im Fenster 'Eigenschaften' auf **Sitzung hinzufügen**, um das Fenster 'Verbindung mit Ziel herstellen' zu öffnen.
3. Wählen Sie im Dialogfenster 'Verbindung mit Ziel herstellen' das Kontrollkästchen **Multipfad aktivieren** aus und klicken Sie auf **Erweitert**.
  ![Ziel](/images/Target.png)

4. Im Fenster 'Erweiterte Einstellungen' ![Einstellungen](/images/Settings.png)
   - Wählen Sie in der Liste lokaler Adapter den Eintrag 'Microsoft iSCSI Initiator' aus.
   - Wählen Sie in der Liste mit Initiator-IPs die IP-Adresse des lokalen Hosts aus.
   - Wählen Sie in der Liste mit Zielportal-IPs die IP der Geräteschnittstelle aus.
   - Klicken Sie auf das Kontrollkästchen **CHAP-Anmeldung aktivieren**.
   - Geben Sie die Werte für die geheimen Schlüsselfelder 'Name' und 'Ziel' ein, die Sie aus dem Portal abgerufen haben, und klicken Sie auf **OK**.
   - Klicken Sie im Fenster 'Verbindung mit Ziel herstellen' auf **OK**, um zum Fenster 'Eigenschaften' zurückzukehren.

5. Klicken Sie auf **Eigenschaften**. Klicken Sie im Dialog 'Eigenschaften' erneut auf **Sitzung hinzufügen**, um den zweiten Pfad hinzuzufügen.
6. Wählen Sie im Fenster 'Verbindung mit Ziel herstellen' das Kontrollkästchen **Multipfad aktivieren** aus. Klicken Sie auf **Erweitert**.
7. Im Fenster "Erweiterte Einstellungen"
   - Wählen Sie in der Liste lokaler Adapter den Eintrag 'Microsoft iSCSI Initiator' aus.
   - Wählen Sie in der Liste mit Initiator-IPs die IP-Adresse aus, die dem Host entspricht. In diesem Fall verbinden Sie zwei Netzschnittstellen auf dem Speichermedium mit einer einzigen Netzschnittstelle auf dem Host. Daher ist diese Schnittstelle mit der für die erste Sitzung bereitgestellten Schnittstelle identisch.
   - Wählen Sie in der Liste mit den Zielportal-IPs die IP-Adresse für die zweite Datenschnittstelle aus, die auf dem Speichermedium aktiviert ist.

     Die zweite IP-Adresse finden Sie in der Detailanzeige zu {{site.data.keyword.blockstorageshort}} im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
      {: tip}
   - Klicken Sie auf das Kontrollkästchen **CHAP-Anmeldung aktivieren**.
   - Geben Sie die Werte für die geheimen Schlüsselfelder 'Name' und 'Ziel' ein, die Sie aus dem Portal abgerufen haben, und klicken Sie auf **OK**.
   - Klicken Sie im Fenster 'Verbindung mit Ziel herstellen' auf **OK**, um zum Fenster 'Eigenschaften' zurückzukehren.
8. Jetzt werden im Fenster 'Eigenschaften' mehrere Sitzungen im Teilfenster 'ID' angezeigt. Sie haben nun mehr als eine Sitzung im iSCSI-Speicher.

   Wenn Ihr Host über mehrere Schnittstellen verfügt, die Sie an den ISCSI-Speicher anschließen möchten, können Sie eine andere Verbindung konfigurieren und dabei die IP-Adresse des anderen NIC im Feld für die Initiator-IP verwenden. Stellen Sie jedoch sicher, dass Sie die zweite Initiator-IP-Adresse im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} autorisieren, bevor Sie versuchen, die Verbindung herzustellen.
   {:note}
9. Klicken Sie im Fenster 'Eigenschaften' auf **Geräte**, um das Fenster 'Geräte' zu öffnen. Der Name der Geräteschnittstelle beginnt mit `mpio`. <br/>
  ![Geräte](/images/Devices.png)

10. Klicken Sie auf **MPIO**, um das Fenster **Gerätedetails** zu öffnen. In diesem Fenster können Sie die Lastausgleichsrichtlinien für MPIO auswählen; außerdem werden die Pfade zu iSCSI angezeigt. Im folgenden Beispiel werden zwei Pfade als für MPIO mit der Lastausgleichsrichtlinie 'Round Robin mit Teilmenge' verfügbar dargestellt.
  ![Fenster 'Gerätedetails' mit zwei Pfaden, die für MPIO mit der Lastausgleichsrichtlinie 'Round Robin mit Teilmenge' verfügbar sind](/images/DeviceDetails.png)

11. Klicken Sie mehrmals auf **OK**, um den iSCSI-Initiator zu beenden.



## Korrekte Konfiguration von MPIO auf Windows-Betriebssystemen prüfen
{: #verifyMPIOWindows}

Wenn Sie überprüfen möchten, ob MPIO für Windows konfiguriert ist, müssen Sie zuerst sicherstellen, dass das MPIO-Add-on aktiviert ist und den Server erneut starten.

![Roles_Features_0](/images/Roles_Features_0.png)

Wenn der Neustart abgeschlossen ist und das Speichermedium hinzugefügt wurde, können Sie überprüfen, ob MPIO konfiguriert ist und ordnungsgemäß funktioniert. Öffnen Sie dazu das Fenster **Zielgerätdetails** und klicken Sie auf **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Falls MPIO nicht ordnungsgemäß konfiguriert ist, wird die Verbindung zum Speichermedium getrennt und ist nicht inaktiviert, sobald ein Netzausfall auftritt oder ein {{site.data.keyword.cloud}}-Team eine Wartung durchführt. Von MPIO wird für solche Ereignisse eine zusätzliche Verbindungsebene bereitgestellt, sodass eine vorhandene Sitzung mit aktiven Lese- und Schreibvorgängen auf einer LUN aufrecht erhalten wird.

## {{site.data.keyword.blockstorageshort}}-Datenträger abhängen
{: #unmountingWin}

Nachfolgend werden die Schritte aufgeführt, die zum Trennen der Verbindung von einer Windows-basierten {{site.data.keyword.Bluemix_short}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind. Das Beispiel basiert auf Windows Server 2012. Die Schritte können für andere Windows-Versionen gemäß der Dokumentation des Anbieters für das Betriebssystem angepasst werden.

### iSCSI-Initiator starten

1. Klicken Sie auf die Registerkarte **Ziele**.
2. Wählen Sie die Ziele aus, die Sie entfernen möchten, und klicken Sie auf **Trennen**.

### Ziele entfernen
Falls Sie nicht mehr auf die iSCSI-Ziele zugreifen müssen, können Sie optional wie folgt vorgehen.

1. Klicken Sie im iSCSI-Initiator **Ermittlung**.
2. Heben Sie das Zielportal hervor, das Ihrem Speicherdatenträger zugeordnet ist, und klicken Sie auf **Entfernen**.
