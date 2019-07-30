---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} verwalten
{: #managingstorage}

Sie können Ihre {{site.data.keyword.blockstoragefull}}-Datenträger über die [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} verwalten. Wählen Sie im **Menü** die **Klassische Infrastruktur** aus, um mit klassischen Services zu interagieren.

## Details zu {{site.data.keyword.blockstorageshort}}-LUN anzeigen

Sie können eine Zusammenfassung der wichtigsten Informationen für die ausgewählte Speicher-LUN einschließlich zusätzlicher Snapshot- und Replikationsfunktionen anzeigen, die zum Speicher hinzugefügt wurden.

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Klicken Sie auf den entsprechenden Datenträgernamen in der Liste. 

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli block volume-detail --help
Syntax: slcli block volume-detail [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -h, --help  Diese Nachricht anzeigen und Ausführung beenden.
```

## Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} autorisieren

Berechtigte ("autorisierte") Hosts sind Hosts, denen Zugriff auf eine bestimmte LUN erteilt wurden. Ohne die Hostberechtigung können Sie über das System nicht auf den Speicher zugreifen und ihn nicht verwenden. Durch das Berechtigen eines Hosts für den Zugriff auf die LUN werden der Benutzername, das Kennwort und der IQN (iSCSI Qualified Name) generiert, die zum Bereitstellen der MPIO-iSCSI-Verbindung (MPIO - Multipath I/O) erforderlich sind.

Sie können Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Sie können mehrere Konten haben, Sie können jedoch nicht einen Host über das eine Konto für den Zugriff auf Ihren Speicher in einem anderen Konto berechtigen.
{:important}

2. Klicken Sie auf **Speicher** > **{{site.data.keyword.blockstorageshort}}**.
3. Suchen Sie den Datenträger und klicken Sie auf **...**.
4. Klicken Sie auf **Host autorisieren**.
5. Zum Aufrufen einer Liste mit verfügbaren Geräten oder IP-Adressen wählen Sie zuerst aus, ob Zugriffsberechtigungen auf der Basis des Gerätetyps oder auf der Basis von Teilnetzen erteilt werden sollen. 
   - Wenn Sie 'Geräte' auswählen, können Sie Bare Metal Server- oder Virtual Server-Instanzen auswählen. 
   - Wenn Sie 'IP-Adressen' auswählen, wählen Sie zuerst das Teilnetz aus, in dem sich der Host befindet. 
6. Wählen Sie in der gefilterten Liste einen oder mehrere Hosts aus, die über Zugriff auf den Datenträger verfügen, und klicken Sie auf **Speichern**.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli block access-authorize --help
Syntax: slcli block access-authorize [OPTIONEN] DATENTRÄGER_ID

Optionen:
  -h, --hardware-id TEXT    Die ID eines Servers zur Autorisierung.
  -v, --virtual-id TEXT     Die ID eines virtuellen Servers zur Autorisierung.
  -i, --ip-address-id TEXT  Die ID einer IP-Adresse zur Autorisierung.
  -p, --ip-address TEXT     Eine IP-Adresse zur Autorisierung.
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

## Liste der Hosts anzeigen, die für Zugriff auf {{site.data.keyword.blockstorageshort}}-LUN autorisiert sind

1. Klicken Sie auf **Speicher** > **{{site.data.keyword.blockstorageshort}}** und dann auf den Namen Ihres Datenträgers. 
2. Blättern Sie abwärts zum Abschnitt **Autorisierte Hosts**.

Dort wird eine Liste der Hosts angezeigt, die derzeit für den Zugriff auf die LUN berechtigt sind. Außerdem werden die Authentifizierungsdaten aufgeführt, die zum Herstellen einer Verbindung erforderlich sind - Benutzername, Kennwort und IQN-Host. Die Zieladresse wird auf der Seite **Speicherdetails** aufgelistet. Die Zieladresse wird für NFS als DNS-Name beschrieben, für iSCSI ist es die IP-Adresse von 'Zielportal ermitteln'.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli block access-list --help
Syntax: slcli block access-list [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, Name, Typ,
                  private IP-Adresse, Quellenteilnetz, Host-IQN,
                  Benutzername, Kennwort, zulässige Host-ID
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
```

## {{site.data.keyword.blockstorageshort}} anzeigen, für den ein Host autorisiert ist

Sie können die LUNs anzeigen, auf die ein Host Zugriff hat, sowie die Informationen, die zum Herstellen einer Verbindung erforderlich sind – LUN-Name, Speichertyp, Zieladresse, Kapazität und Position:

1. Klicken Sie in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} auf **Geräte** -> **Geräteliste** und klicken Sie auf das entsprechende Gerät.
2. Wählen Sie die Registerkarte **Speicher** aus.

Es wird eine Liste der Speicher-LUNs angezeigt, auf die dieser Host zugreifen kann. Die Liste ist nach Speichertypen gruppiert (Blockspeicher, Dateispeicher, etc.). Durch Klicken auf **Aktionen** können Sie weiteren Speicher autorisieren oder Zugriff widerrufen.

Ein Host kann nicht für den Zugriff auf LUNs unterschiedlicher Betriebssystemtypen gleichzeitig berechtigt werden. Ein Host kann nur für den Zugriff auf LUNs eines einzigen Betriebssystemtyps berechtigt werden. Wenn Sie versuchen, den Zugriff auf mehrere LUNs mit unterschiedlichen Betriebssystemtypen zu berechtigen, führt die Operation zu einem Fehler.
{:note}

## {{site.data.keyword.blockstorageshort}} anhängen und abhängen

Befolgen Sie auf der Basis des Betriebssystems Ihres Hosts die entsprechenden Anweisungen.

- [Verbindung zu LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verbindung zu LUNs unter CloudLinux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Verbindung zu LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## Zugriff eines Hosts auf {{site.data.keyword.blockstorageshort}} widerrufen

Wenn Sie den Zugriff eines Hosts auf eine bestimmte Speicher-LUN stoppen möchten, können Sie den Zugriff widerrufen. Wenn der Zugriff widerrufen wird, wird die Hostverbindung zur LUN gelöscht. Das Betriebssystem und die Anwendungen auf diesem Host können nicht mehr mit der LUN kommunizieren.

Um hostseitige Probleme zu vermeiden, hängen Sie die Speicher-LUN von Ihrem Betriebssystem ab, bevor Sie den Zugriff widerrufen, sodass keine Laufwerke fehlen oder Daten beschädigt werden.
{:important}

Sie können den Zugriff über die **Geräteliste** oder die **Speicheransicht** widerrufen.

### Zugriff über Geräteliste widerrufen

1. Klicken Sie in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} auf das Symbol für die klassische Infrastruktur. Klicken Sie dann auf **Geräte**, **Geräteliste** und anschließend doppelt auf das entsprechende Gerät. 
2. Wählen Sie die Registerkarte **Speicher** aus.
3. Es wird eine Liste der Speicher-LUNs angezeigt, auf die dieser Host zugreifen kann. Die Liste ist nach Speichertypen gruppiert (Blockspeicher, Dateispeicher, etc.). Klicken Sie neben dem Datenträgernamen auf **Aktionen**  und dann auf **Zugriffsberechtigung widerrufen**.
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

### Zugriff über die SLCLI widerrufen

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli block access-revoke --help
Syntax: slcli block access-revoke [OPTIONEN] DATENTRÄGER-ID

Optionen:
  -h, --hardware-id TEXT    Die ID eines Servers zum Widerrufen der Autorisierung.
  -v, --virtual-id TEXT     Die ID eines virtuellen Servers zum Widerrufen der Autorisierung.
  -i, --ip-address-id TEXT  Die ID einer IP-Adresse zum Widerrufen der Autorisierung.
  -p, --ip-address TEXT     Eine IP-Adresse zum Widerrufen der Autorisierung.
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

## Speicher-LUN abbrechen

Wenn Sie eine bestimmte LUN nicht mehr brauchen, können Sie sie jederzeit abbrechen.

Zum Abbrechen einer Speicher-LUN müssen Sie zunächst den Zugriff von allen Hosts widerrufen.
{:important}

1. Klicken Sie auf **Speicher**, **{{site.data.keyword.blockstorageshort}}**.
2. Wählen Sie den Datenträger aus, für den ein Abbruch erfolgen soll, klicken Sie auf **Aktionen** und wählen Sie **{{site.data.keyword.blockstorageshort}} abbrechen** aus.
3. Geben Sie an, ob die LUN sofort abgebrochen werden soll oder am Stichtag der Bereitstellung der LUN.

   Wenn Sie die Option auswählen, um die LUN an ihrem Stichtag abzubrechen, können Sie die Abbruchanforderung vor ihrem Stichtag annullieren.
   {:tip}
4. Klicken Sie auf **Weiter** oder **Schließen**.
5. Klicken Sie auf das Kontrollkästchen **Bestätigung** und klicken Sie auf **Bestätigen**.

Alternativ dazu können Sie den folgenden Befehl in der SLCLI verwenden.
```
# slcli block volume-cancel --help
Syntax: slcli block volume-cancel [OPTIONEN] DATENTRÄGER-ID

Optionen:
  --reason TEXT  Optionaler Grund für den Abbruch
  --immediate    Bricht den Blockspeicherdatenträger sofort ab,
                 nicht am Abrechnungsstichtag.
  -h, --help     Diese Nachricht anzeigen und Ausführung beenden.
```

Normalerweise bleibt die LUN in Ihrer Speicherliste mindestens 24 Stunden (sofortiger Abbruch) oder bis zum Ablauf eines Jahres sichtbar. Bestimmte Features bleiben zwar nicht mehr sichtbar, doch der Datenträger bleibt so lange sichtbar, bis er freigegeben wird. Die Fakturierung wird jedoch unmittelbar nach dem Klicken auf 'Löschen/Abbrechen' gestoppt.

Aktive Replikate können die Freigabe des Speicherdatenträgers blockieren. Stellen Sie sicher, dass der Datenträger nicht mehr angehängt ist, dass Hostberechtigungen widerrufen werden und dass die Replikation abgebrochen wird, bevor Sie versuchen, den ursprünglichen Datenträger abzubrechen. 
