---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Verbindung zu MPIO iSCSI LUNS unter Microsoft Windows herstellen

Stellen Sie vor dem Start sicher, dass der Host, der auf den {{site.data.keyword.blockstoragefull}}-Datenträger zugreift, über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} autorisiert wurde:

1. Klicken Sie auf der Listenseite von {{site.data.keyword.blockstorageshort}} auf die **Aktionen**, die dem neu bereitgestellten Datenträger zugeordnet sind, und klicken Sie auf **Host autorisieren**.
2. Wählen Sie den bzw. die gewünschten Hosts in der Liste aus und klicken Sie auf **Übergeben**. Damit werden die Hosts zum Zugriff auf den Datenträger autorisiert.

## Vorgehensweise zum Anhängen von {{site.data.keyword.blockstorageshort}}-Datenträgern

Nachfolgend werden die Schritte beschrieben, die zum Herstellen einer Verbindung von einer Windows-basierten {{site.data.keyword.BluSoftlayer_full}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind (MPIO = Multipath Input/Output; iSCSI = Internet Small Computer System Interface; LUN = Logical Unit Number). Das Beispiel basiert auf Windows Server 2012. Bei anderen Windows-Versionen können die Schritte entsprechend der Dokumentation des Anbieters des Betriebssystems angepasst werden. 

### Funktion MPIO konfigurieren

1. Starten Sie den Servermanager und navigieren Sie zu **Verwalten**, **Rollen und Funktionen hinzufügen**..
2. Klicken Sie auf **Weiter**, um das Menü 'Funktionen' zu öffnen.
3. Blättern Sie abwärts und aktivieren Sie das Kontrollkästchen **Multipath I/O**.
4. Klicken Sie auf **Installieren**, um MPIO auf dem Host-Server zu installieren.
![Rollen und Funktionen hinzufügen](/images/Roles_Features.png)

### iSCSI-Unterstützung für MPIO hinzufügen

1. Öffnen Sie die Eigenschaften von MPIO. Klicken Sie zum Öffnen der MPIO-Eigenschaften auf **Start**, ziehen Sie den Mauszeiger auf die Verwaltungstools und klicken Sie auf **MPIO**.
2. Klicken Sie auf die Registerkarte **Multipaths erkennen**.
3. Aktivieren Sie das Kontrollkästchen **Unterstützung für iSCSI-Geräte hinzufügen** und klicken Sie auf **Hinzufügen**. Klicken Sie bei der Aufforderung zum Neustart des Computers auf **Ja**.

**Hinweis**: Bei Windows Server 2008 kann das Microsoft Device Specific Module (MSDSM) durch das Hinzufügen von Unterstützung für iSCSI alle iSCSI-Geräte für MPIO übernehmen, wofür zuerst eine Verbindung zu einem iSCSI-Ziel erforderlich ist.

### iSCSI-Initiator konfigurieren

1. Starten Sie den iSCSI-Initiator über den Servermanager und wählen Sie **Tools**, **iSCSI-Initiator** aus.
2. Klicken Sie auf die Registerkarte **Konfiguration**.
    - Das Feld 'Initiatorname' enthält möglicherweise bereits einen Eintrag, der 'iqn.1991-05.com.microsoft:' ähnelt.
    - Klicken Sie auf **Ändern**, um die vorhandenen Werte durch Ihren qualifizierten iSCSI-Namen (IQN) zu ersetzen. Der IQN kann über die Anzeige '{{site.data.keyword.blockstorageshort}} - Details' im Portal abgerufen werden.
    ![Eigenschaften des iSCSI-Initiators](/images/iSCSI.png)
    - Klicken Sie auf **Erkennung** und anschließend auf **Portal erkennen**.
    - Geben Sie die IP-Adresse Ihres iSCSI-Ziels ein und lassen Sie den Port auf dem Standardwert 3260. 
    - Klicken Sie auf **Erweitert**, um das Fenster 'Erweiterte Einstellungen' zu starten.
    - Wählen Sie **CHAP-Anmeldung aktivieren**, um die CHAP-Authentifizierung zu aktivieren.
    ![CHAP-Anmeldung aktivieren](/images/Advanced_0.png)
    **Hinweis:** Bei den geheimen Schlüsselfeldern 'Name' und 'Ziel' muss die Groß-/Kleinschreibung beachtet werden.
         - Löschen Sie die im Namensfeld vorhandenen Einträge und geben Sie den Benutzernamen aus dem Portal ein.
         - Geben Sie im geheimen Schlüsselfeld 'Ziel' das Kennwort aus dem Portal ein.<br/>
         **Hinweis:** Sie können die Werte für die geheimen Schlüsselfelder 'Name' und 'Ziel' in der Anzeige '{{site.data.keyword.blockstorageshort}} - Details' des Portals jeweils als Benutzername und Kennwort abrufen.
    - Klicken Sie in den Fenstern 'Erweiterte Einstellungen' und 'Zielportal erkennen' auf **OK**, um zur Hauptanzeige 'Eigenschaften des iSCSI-Initiators' zurückzukehren. Überprüfen Sie die Eingaben für den Benutzernamen und das Kennwort noch einmal, wenn Sie Authentifizierungsfehler erhalten.
    ![Inaktives Ziel](/images/Inactive_0.png)
    Beachten Sie, dass der Name Ihres Ziels im Bereich 'Erkannte Ziele' mit dem Status 'Inaktiv' angezeigt wird. 
    
    In den folgenden Schritten wird gezeigt, wie Sie das Ziel aktivieren.
    
### Ziel aktivieren

1. Klicken Sie auf **Verbinden**, um eine Verbindung zu dem Ziel herzustellen.
2. Aktivieren Sie das Kontrollkästchen **Multipath aktivieren**, um Multipath I/O für das Ziel zu aktivieren.
![Multipath aktivieren](/images/Connect_0.png)
3. Klicken Sie auf **Erweitert** und wählen Sie **CHAP-Anmeldung aktivieren** aus.
![CHAP aktivieren](/images/chap_0.png)
4. Geben Sie im Feld 'Name' den Benutzernamen und im geheimen Schlüsselfeld 'Ziel' das Kennwort ein.<br/>
**Hinweis:** Sie können die Werte für die geheimen Schlüsselfelder 'Name' und 'Ziel' in der Anzeige '{{site.data.keyword.blockstorageshort}} - Details' des Portals jeweils als Benutzername und Kennwort abrufen.
5. Klicken Sie auf **OK**, bis das Fenster 'Eigenschaften des iSCSI-Initiators' angezeigt wird. Der Status des Ziels im Bereich 'Erkannte Ziele' wird von 'Inaktiv' in 'Verbunden' geändert.
![Status 'Verbunden'](/images/Connected.png) 


### MPIO im iSCSI-Initiator konfigurieren

1. Starten Sie den iSCSI-Initiator und klicken Sie auf der Registerkarte 'Ziele' auf **Eigenschaften**.
2. Klicken Sie im Fenster 'Eigenschaften' auf **Sitzung hinzufügen**, um das Fenster 'Verbindung mit Ziel herstellen' zu starten.
3. Aktivieren Sie das Kontrollkästchen **Multipath auswählen** und klicken Sie auf **Erweitert...**.
  ![Ziel](/images/Target.png) 
  
4. Gehen Sie im Fenster 'Erweiterte Einstellungen' wie folgt vor:
   - Belassen Sie den Wert 'Standard' in den Feldern 'Lokaler Adapter' und 'Initiator-IP'. Bei Host-Servern mit mehreren Schnittstellen zum iSCSI-Initiator müssen Sie für das Feld 'Initiator-IP' den passenden Wert wählen.
   - Wählen Sie in der Dropdown-Liste 'Zielportal-IP' die IP Ihres iSCSI-Speichers aus.
   - Aktivieren Sie das Kontrollkästchen **CHAP-Anmeldung aktivieren**.
   - Geben Sie die Werte für die geheimen Schlüsselfelder 'Name' und 'Ziel' ein, die Sie aus dem Portal abgerufen haben, und klicken Sie auf **OK**.
   - Klicken Sie im Fenster 'Verbindung mit Ziel herstellen' auf **OK**, um zum Fenster 'Eigenschaften' zurückzukehren. Das Fenster 'Eigenschaften' sollte nun mehrere Sitzungen im Fenster 'ID' anzeigen. Nun haben Sie mehrere Sitzungen zum iSCSI-Initiator.
   ![Einstellungen](/images/Settings.png) 
   
5. Klicken Sie im Fenster 'Eigenschaften' auf **Geräte** und starten Sie das Fenster 'Geräte'. Der Geräteschnittstellenname sollte mit 'mpio' anfangen. <br/>
  ![Geräte](/images/Devices.png) 
  
6. Klicken Sie auf **MPIO**, um das Fenster 'Gerätedetails' zu starten. In diesem Fenster können Sie Lastausgleichsrichtlinien für MPIO wählen und die Pfade zu iSCSI einsehen. In diesem Beispiel werden zwei Pfade mit einer Lastausgleichsrichtlinie 'Umlauf mit Subset' als verfügbar angezeigt.
  ![Gerätedetails](/images/DeviceDetails.png) 
  
7. Klicken Sie mehrmals auf **OK**, um den iSCSI-Initiator zu beenden.



## Korrekte Konfiguration von MPIO auf Windows-Betriebssystemen prüfen

Bei der Prüfung, ob MPIO unter Windows konfiguriert wurde, müssen Sie zuerst sicherstellen, dass das Add-on für MPIO aktiviert und der erforderliche Warmstart durchgeführt wurde:

![Roles_Features_0](/images/Roles_Features_0.png)

Sobald der Warmstart abgeschlossen ist und das Leistungsspeichergerät hinzugefügt wurde, kann geprüft werden, ob MPIO konfiguriert ist und funktioniert. Öffnen Sie dazu das Fenster **Zielgerätdetails** und klicken Sie auf **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Wenn MPIO auf Ihrem Leistungsspeichergerät nicht konfiguriert wurde und die {{site.data.keyword.BluSoftlayer_full}}-Teams eine Wartung durchführen oder ein Netzausfall auftritt, wird die Verbindung des Speichergeräts getrennt und es steht nicht mehr zu Verfügung. MPIO stellt während solcher Ereignisse eine zusätzliche Konnektivitätsebene sicher und behält eine eingerichtete Sitzung mit aktiven Lese-/Schreibvorgängen an die LUN bei.

## Vorgehensweise zum Abhängen von {{site.data.keyword.blockstorageshort}}-Datenträgern

Nachfolgend werden die Schritte beschrieben, die zum Trennen einer Verbindung einer Windows-basierten Bluemix-Recheninstanz von einer MPIO-iSCSI-LUN erforderlich sind. Das Beispiel basiert auf Windows Server 2012. Bei anderen Windows-Versionen
können die Schritte entsprechend der Dokumentation des Anbieters des Betriebssystems angepasst werden.

### Starten Sie den iSCSI-Initiator.

1. Klicken Sie auf die Registerkarte **Ziele**.
2. Wählen Sie die Ziele aus, die Sie entfernen wollen, und klicken Sie auf **Trennen**.

### Gehen Sie wie folgt vor, wenn Sie nicht mehr auf die iSCSI-Ziele zugreifen müssen:

1. Klicken Sie im iSCSI-Initiator auf die Registerkarte **Erkennung**.
2. Heben Sie das Zielportal hervor, das Ihrem Speicherdatenträger zugeordnet ist, und klicken Sie auf **Entfernen**.
