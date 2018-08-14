---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Verbindung zu MPIO-iSCSI-LUNS unter Microsoft Windows herstellen

Stellen Sie vor dem Start sicher, dass der Host, von dem auf das {{site.data.keyword.blockstoragefull}}-Laufwerk zugegriffen wird, im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} autorisiert wurde.

1. Suchen Sie auf der Seite mit der {{site.data.keyword.blockstorageshort}}-Liste den neuen Datenträger und klicken Sie auf **Aktionen**.Klicken Sie auf **Host autorisieren**.
2. Wählen Sie in der Liste den Host oder die Hosts aus, der bzw. die auf den Datenträger zugreifen soll(en), und klicken Sie auf **Abschicken**.

## {{site.data.keyword.blockstorageshort}}-Datenträger anhängen

Nachfolgend werden die Schritte beschrieben, die zum Herstellen einer Verbindung von einer Windows-basierten {{site.data.keyword.BluSoftlayer_full}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind (MPIO = Multipath Input/Output; iSCSI = internet Small Computer System Interface; LUN = Logical Unit Number).Das Beispiel basiert auf Windows Server 2012. Die Schritte können für andere Windows-Versionen gemäß der Dokumentation des Anbieters für das Betriebssystem angepasst werden.

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

**Hinweis:** Bei Windows Server 2008 kann das Microsoft Device Specific Module (MSDSM) durch das Hinzufügen von Unterstützung für iSCSI alle iSCSI-Geräte für MPIO übernehmen, wofür zuerst eine Verbindung zu einem iSCSI-Ziel erforderlich ist.

### iSCSI-Initiator konfigurieren

1. Starten Sie den iSCSI-Initiator über den Servermanager und wählen Sie **Tools**, **iSCSI-Initiator** aus.
2. Klicken Sie auf die Registerkarte **Konfiguration**.
    - Das Feld 'Initiatorname' enthält möglicherweise bereits einen Eintrag, der `iqn.1991-05.com.microsoft:` ähnelt.
    - Klicken Sie auf **Ändern**, um die vorhandenen Werte durch Ihren qualifizierten iSCSI-Namen (IQN) zu ersetzen. Der IQN (iSCSI Qualified Name) kann aus der Anzeige '{{site.data.keyword.blockstorageshort}} - Details' im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} abgerufen werden.
    ![Eigenschaften des iSCSI-Initiators](/images/iSCSI.png)
    - Klicken Sie auf **Erkennung** und anschließend auf **Portal erkennen**.
    - Geben Sie die IP-Adresse Ihres iSCSI-Ziels ein und lassen Sie den Port auf dem Standardwert 3260. 
    - Klicken Sie auf **Erweitert**, um das Fenster 'Erweiterte Einstellungen' zu öffnen.
    - Wählen Sie **CHAP-Anmeldung aktivieren**, um die CHAP-Authentifizierung zu aktivieren.
    ![CHAP-Anmeldung aktivieren](/images/Advanced_0.png)
    **Hinweis:** In den Feldern 'Name' und 'Zielschlüssel' muss die Groß-/Kleinschreibung beachtet werden.
         - Löschen Sie im Feld **Name** alle vorhandenen Einträge und geben Sie den Benutzernamen aus dem [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} ein.
         - Geben Sie im Feld **Zielschlüssel** das Kennwort aus dem [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} ein.
    - Klicken Sie auf **OK** in den Fenstern **Erweiterte Einstellungen** und **Zielportal ermitteln**, um zur Hauptanzeige für die Eigenschaften des iSCSI-Initiators zurückzukehren. Überprüfen Sie die Eingaben für den Benutzernamen und das Kennwort, wenn Sie Authentifizierungsfehler erhalten.
    ![Inaktives Ziel](/images/Inactive_0.png)
    **Hinweis:** Der Name des Ziels im Bereich 'Erkannte Ziele' wird mit dem Status 'Inaktiv' angezeigt. 

    
### Ziel aktivieren

1. Klicken Sie auf **Verbinden**, um eine Verbindung zu dem Ziel herzustellen.
2. Wählen Sie das Kontrollkästchen **Multipfad aktivieren** aus, um Multipath IO für das Ziel zu aktivieren.
![Multipfad aktivieren](/images/Connect_0.png)
3. Klicken Sie auf **Erweitert** und wählen Sie **CHAP-Anmeldung aktivieren** aus.
![CHAP aktivieren](/images/chap_0.png)
4. Geben Sie in das Feld 'Name' den Benutzernamen und in das Feld 'Zielschlüssel' das Kennwort ein.<br/>
**Hinweis:** Die Werte der Felder 'Name' und 'Zielschlüssel' können aus der Anzeige '{{site.data.keyword.blockstorageshort}} - Details' abgerufen werden.
5. Klicken Sie auf **OK**, bis das Fenster **Eigenschaften des iSCSI-Initiators** angezeigt wird. Der Status des Ziels im Abschnitt **Erkannte Ziele** wechselt von **Inaktiv** zu **Verbunden**.
![Verbundener Status](/images/Connected.png) 


### MPIO im iSCSI-Initiator konfigurieren

1. Starten Sie den iSCSI-Initiator und klicken Sie auf der Registerkarte 'Ziele' auf **Eigenschaften**.
2. Klicken Sie im Fenster 'Eigenschaften' auf **Sitzung hinzufügen**, um das Fenster 'Verbindung mit Ziel herstellen' zu öffnen.
3. Wählen Sie das Kontrollkästchen **Multipfad aktivieren** aus und klicken Sie auf **Erweitert...**.
  ![Ziel](/images/Target.png) 
  
4. Im Fenster 'Erweiterte Einstellungen'
   - Belassen Sie den Wert 'Standard' in den Feldern 'Lokaler Adapter' und 'Initiator-IP'. Bei Host-Servern mit mehreren Schnittstellen zum iSCSI-Initiator müssen Sie für das Feld 'Initiator-IP' den passenden Wert wählen.
   - Wählen Sie die IP des iSCSI-Speichers aus der Drop-down-Liste **Zielportal-IP** aus.
   - Klicken Sie auf das Kontrollkästchen **CHAP-Anmeldung aktivieren**.
   - Geben Sie die Werte für die geheimen Schlüsselfelder 'Name' und 'Ziel' ein, die Sie aus dem Portal abgerufen haben, und klicken Sie auf **OK**.
   - Klicken Sie im Fenster 'Verbindung mit Ziel herstellen' auf **OK**, um zum Fenster 'Eigenschaften' zurückzukehren. Jetzt werden im Fenster 'Eigenschaften' mehrere Sitzungen im Teilfenster 'ID' angezeigt. Nun haben Sie mehrere Sitzungen zum iSCSI-Initiator.
   ![Einstellungen](/images/Settings.png) 
   
5. Klicken Sie im Fenster 'Eigenschaften' auf **Geräte**, um das Fenster 'Geräte' zu öffnen. Der Name der Geräteschnittstelle beginnt mit `mpio`.<br/>
  ![Geräte](/images/Devices.png) 
  
6. Klicken Sie auf **MPIO**, um das Fenster **Gerätedetails** zu öffnen. In diesem Fenster können Sie die Lastausgleichsrichtlinien für MPIO auswählen; außerdem werden die Pfade zu iSCSI angezeigt. Im folgenden Beispiel werden zwei Pfade als für MPIO mit der Lastausgleichsrichtlinie 'Round Robin mit Teilmenge' verfügbar dargestellt.
  ![Fenster 'Gerätedetails' mit zwei Pfaden, die für MPIO mit der Lastausgleichsrichtlinie 'Round Robin mit Teilmenge' verfügbar sind](/images/DeviceDetails.png) 
  
7. Klicken Sie mehrmals auf **OK**, um den iSCSI-Initiator zu beenden.



## Korrekte Konfiguration von MPIO auf Windows-Betriebssystemen prüfen

Wenn Sie überprüfen möchten, ob MPIO für Windows konfiguriert ist, müssen Sie zuerst sicherstellen, dass das MPIO-Add-on aktiviert ist und den Server erneut starten.

![Roles_Features_0](/images/Roles_Features_0.png)

Wenn der Neustart abgeschlossen ist und die Speichereinheit hinzugefügt wurde, können Sie überprüfen, ob MPIO konfiguriert ist und ordnungsgemäß funktioniert. Öffnen Sie dazu das Fenster **Zielgerätdetails** und klicken Sie auf **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Falls MPIO nicht ordnungsgemäß konfiguriert ist, wird die Verbindung zur Speichereinheit getrennt und ist nicht mehr verfügbar, sobald ein Netzausfall auftritt oder ein {{site.data.keyword.BluSoftlayer_full}}-Team eine Wartung durchführt. Von MPIO wird für solche Ereignisse eine zusätzliche Verbindungsebene bereitgestellt, sodass eine vorhandene Sitzung mit aktiven Lese- und Schreibvorgängen auf einer LUN aufrecht erhalten wird.

## {{site.data.keyword.blockstorageshort}}-Datenträger abhängen

Nachfolgend werden die Schritte aufgeführt, die zum Trennen der Verbindung von einer Windows-basierten {{site.data.keyword.Bluemix_short}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind. Das Beispiel basiert auf Windows Server 2012. Die Schritte können für andere Windows-Versionen gemäß der Dokumentation des Anbieters für das Betriebssystem angepasst werden.

### iSCSI-Initiator starten

1. Klicken Sie auf die Registerkarte **Ziele**.
2. Wählen Sie die Ziele aus, die Sie entfernen möchten, und klicken Sie auf **Trennen**.

### Ziele entfernen
Falls Sie nicht mehr auf die iSCSI-Ziele zugreifen müssen, können Sie optional wie folgt vorgehen.

1. Klicken Sie im iSCSI-Initiator **Ermittlung**.
2. Heben Sie das Zielportal hervor, das Ihrem Speicherdatenträger zugeordnet ist, und klicken Sie auf **Entfernen**.
