---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Daten replizieren
{: #replication}

Bei der Replikation werden Snapshots mithilfe eines Ihrer Snapshotzeitpläne automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum kopiert. Im Fall beschädigter Daten oder einer Katastrophe können die Kopien an dem fernen Standort wiederhergestellt werden.

Die Replikation hält Ihre Daten an zwei verschiedenen Positionen synchron. Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie den Abschnitt [Duplikat des Blockdatenträgers erstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).
{:tip}

Um Replikationen durchführen zu können, müssen Sie einen Snapshotzeitplan erstellen.
{:important}


## Fernes Rechenzentrum für replizierten Speicherdatenträger ermitteln

Die Rechenzentren von {{site.data.keyword.cloud}} wurden weltweit in Paare aus einem primären und einem fernen Datenträger unterteilt.
In Tabelle 1 finden Sie eine vollständige Liste der Verfügbarkeit der Rechenzentren und der Replikationsziele.

| US 1 | US 2 | Lateinamerika | Kanada  | Europa  | Asien/Pazifik  | Australien  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="Tabelle 1 - In dieser Tabelle wird eine vollständige Liste der Rechenzentren mit erweiterten Leistungsmerkmalen in jeder Region aufgeführt. Jede Region wird in einer separaten Spalte angegeben. In manchen Städten, wie zum Beispiel Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London und Sydney, befinden sich mehrere Rechenzentren. Rechenzentren in der Region US 1 verfügen NICHT über erweiterten Speicher. Hosts in Rechenzentren mit erweiterten Speicherleistungsmerkmalen können die Replikation mit Replikationszielen in Rechenzentren der Region US 1 nicht einleiten." caption-side="top"}

## Erstreplikation erstellen

Replikationen werden auf der Basis eines Snapshotzeitplans ausgeführt. Sie müssen zuerst über einen Snapshotbereich und einen Snapshotzeitplan für den Quellendatenträger verfügen, um replizieren zu können. Wenn Sie versuchen, eine Replikation zu konfigurieren und sich der eine oder andere nicht an seiner Position befindet, werden Sie aufgefordert, mehr Speicherplatz zu erwerben oder einen Zeitplan einzurichten. Replikationen werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf **Replikat** und anschließende auf **Replikation kaufen**.
3. Wählen Sie einen vorhandenen Snapshotzeitplan für Ihre Replikationen aus. Die Liste enthält alle Ihre aktiven Snapshotzeitpläne. <br />
   Sie können nur einen einzigen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben. Alle seit dem letzten Replikationszyklus erfassten Snapshots werden unabhängig von dem Plan repliziert, aus dem sie stammen.<br />Wenn Sie keine Snapshots eingerichtet haben, werden Sie aufgefordert, dies vor der Bestellung der Replikation zu tun. Weitere Informationen finden Sie unter [Mit Snapshots arbeiten](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots).
   {:important}
3. Klicken Sie auf **Position** und wählen Sie das Rechenzentrum aus, das Sie als Standort für das Disaster-Recovery-Standort verwenden möchten.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Fenster sind mit den Standardwerten gefüllt.

   Rabatte werden bei der Verarbeitung der Bestellung angewendet.
   {:note}
6. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Bestellung aufgeben**.


## Vorhandene Replikation bearbeiten

Sie können in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} entweder auf der Registerkarte **Primär** oder auf der Registerkarte **Replikat** unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** Ihren Replikationsplan bearbeiten und Ihren Replikationsspeicherbereich ändern.


## Replikationszeitplan bearbeiten

Der Replikationsplan basiert auf einem vorhandenen Snapshotzeitplan. Um den Replikationsplan von 'Stündlich' in 'Täglich' oder 'Wöchentlich' zu ändern oder umgekehrt, müssen Sie den Replikatdatenträger löschen und einen neuen einrichten.

Wenn Sie jedoch die Tageszeit ändern möchten, zu der die **tägliche** Replikation stattfindet, können Sie den vorhandenen Zeitplan auf der Registerkarte des Primärdatenträgers oder des Replikatdatenträgers ändern.

1. Klicken Sie entweder auf der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
2. Wählen Sie **Snapshotzeitplan bearbeiten** aus.
3. Überprüfen Sie im Rahmen **Snapshot** die Angaben unter **Plan**, um festzustellen, welchen Plan Sie für die Replikation verwenden. Ändern Sie den gewünschten Zeitplan.
4. Klicken Sie auf **Speichern**.


## Replikationsbereich ändern

Der primäre Replikationsbereich und der Replikatsbereich müssen identisch sein. Wenn Sie den Bereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird automatisch Speicherplatz zu Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Wird der Snapshotbereich vergrößert, wird auch eine sofortige Aktualisierung der Replikation ausgelöst.

1. Klicken Sie entweder auf der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
2. Wählen Sie **Snapshotbereich ändern** aus.
3. Wählen Sie in der Liste die Speichergröße aus und klicken Sie auf **Weiter**.
4. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfeld nehmen die Standardwerte an.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Bestellung aufgeben**.


## Replikatdatenträger in der {{site.data.keyword.blockstorageshort}}-Liste anzeigen

Sie können Ihre Replikationsdatenträger auf der Seite {{site.data.keyword.blockstorageshort}} unter **Speicher > {{site.data.keyword.blockstorageshort}}** anzeigen. Der ursprüngliche Datenträger und der Replikatdatenträger werden in einer Gruppe zusammengefasst. Der **LUN-Name** weist den Namen des Primärdatenträgers auf, an den REP angehängt ist. Der **Typ** ist 'Endurance - Replikat' oder 'Performance - Replikat'.


## Details eines replizierten Datenträgers im Replikatrechenzentrum anzeigen

Sie können Details zum Replikatdatenträger anzeigen, indem Sie auf die Registerkarte **Replikat** klicken, während Details zum ursprünglichen Datenträger angezeigt werden. Eine weitere Option besteht darin, den Replikatdatenträger in der **{{site.data.keyword.blockstorageshort}}**-Liste auszuwählen und auf die Registerkarte **Replikat** zu klicken. 


## Snapshotbereich im Replikatrechenzentrum erhöhen, wenn der Snapshotbereich im primären Rechenzentrum erhöht wird

Ihr primärer Datenträger und der Replikatspeicherdatenträger müssen dieselbe Datenträgergröße aufweisen. Der eine darf nicht größer sein als der andere. Wenn Sie Ihren Snapshotbereich für Ihren Primärdatenträger erhöhen, wird der Replikatbereich automatisch erhöht. Wird der Snapshotbereich vergrößert, wird eine sofortige Aktualisierung der Replikation ausgelöst. Die Erhöhung der beiden Datenträger wird auf Ihrer Rechnung als Artikelposition angezeigt und bei Bedarf anteilig berechnet.

Weitere Informationen zum Vergrößern des Snapschotbereichs finden Sie unter [Snapshots bestellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:tip}


## Replikationsprotokoll anzeigen

Das Replikationsprotokoll kann im **Auditprotokoll** auf der Registerkarte **Konto** im Bereich **Verwalten** angezeigt werden. Auf dem Primär- und dem Replikatdatenträger werden identische Replikationsprotokolle angezeigt. Das Protokoll enthält die folgenden Elemente.

- Der Typ für die Replikation (Failover oder Failback).
- Der Startzeitpunkt.
- Der Snapshot, der für die Replikation verwendet wurde.
- Die Größe der Replikation.
- Der Zeitpunkt, zu dem die Replikation abgeschlossen ist.


## Duplikat eines Replikats erstellen

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.cloud}} {{site.data.keyword.blockstoragefull}}s erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.

Duplikate können sowohl für den Primär- als auch für den Replikatdatenträger erstellt werden. Das neue Duplikat wird in demselben Rechenzentrum wie der Originaldatenträger erstellt. Wenn Sie ein Duplikat von einem Replikatdatenträger erstellen, wird der neue Datenträger in demselben Rechenzentrum erstellt wie der Replikatdatenträger.

Der Lese- und Schreibzugriff auf duplizierte Datenträger kann durch einen Host erfolgen, sobald der Speicher bereitgestellt wurde. Snapshots und Replikation sind dagegen erst zulässig, nachdem die Daten vollständig vom Original auf das Duplikat kopiert wurden.

Weitere Informationen finden Sie in [Duplikat von {{site.data.keyword.blockstorageshort}} erstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Verwenden von Replikaten für ein Failover bei einem Ausfall

Beim Failover 'kippen Sie den Schalter' von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Beispiel: Ihr primäres Rechenzentrum befindet sich in London und Ihr sekundäres Rechenzentrum in Amsterdam. Bei einem Fehlerereignis können Sie ein Failover nach Amsterdam durchführen und von einer Recheninstanz in Amsterdam eine Verbindung zu dem nun primären Datenträger herstellen. Nachdem Ihr Datenträger in London repariert wurde, wird von dem Datenträger in Amsterdam ein Snapshot erstellt, um von einer Recheninstanz in London eine Rückübertragung nach London und auf den nun wieder primären Datenträger durchzuführen.

* Wenn der primäre Standort sich in unmittelbarer Gefahr befindet oder erheblich beeinträchtigt ist, finden Sie weitere Informationen unter [Failover mit einem zugänglichen Primärdatenträger](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible).
* Wenn der primäre Standort inaktiv ist, finden Sie weitere Informationen unter [Failover mit einem nicht zugänglichen Primärdatenträger](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible).


## Vorhandene Replikation abbrechen

Der Abbruch kann sofort oder am Stichtag erfolgen und bewirkt das Ende der Abrechnung. Die Replikation kann auf der Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' auf den Datenträger.
2. Klicken Sie entweder auf der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
3. Wählen Sie **Replikat abbrechen** aus.
4. Wählen Sie den Zeitpunkt des Abbruchs aus. Wählen Sie **Sofort** oder **Stichtag** aus und klicken Sie auf **Weiter**.
5. Aktivieren Sie **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Replikat abbrechen**.


## Replikation beim Abbrechen des Primärdatenträgers abbrechen

Wenn ein Primärdatenträger abgebrochen wird, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite '{{site.data.keyword.blockstorageshort}}' abgebrochen. 

 1. Klicken Sie auf der Seite von **{{site.data.keyword.blockstorageshort}}** auf den Datenträgernamen. 
 2. Klicken Sie auf der Detailseite von **{{site.data.keyword.blockstorageshort}}** auf **Aktionen** und wählen Sie **Replikat abbrechen** aus.
 3. Wählen Sie den Zeitpunkt des Abbruchs aus. Wählen Sie **Sofort** oder **Stichtag** aus und klicken Sie auf **Weiter**.
 4. Bestätigen Sie, dass Ihnen bekannt ist, dass es beim Abbrechen der Aktion für den Datenträger zu Datenverlust kommen kann, indem Sie das Feld auswählen. 
 5. Klicken Sie auf **Replikat abbrechen**. 

 Naturgemäß bleibt die LUN in Ihrer Speicherliste mindestens 24 Stunden (sofortiger Abbruch) oder bis zum Ablauf eines Jahres sichtbar. Bestimmte Features bleiben zwar nicht mehr sichtbar, doch der Datenträger bleibt so lange sichtbar, bis er freigegeben wird. Die Abrechnung wird jedoch unmittelbar nach dem Klicken auf die Schaltfläche zum Löschen oder Abbrechen des Replikats gestoppt. 

 Aktive Replikate können die Freigabe des Speicherdatenträgers blockieren. Stellen Sie sicher, dass der Datenträger nicht mehr angehängt ist, dass Hostberechtigungen widerrufen werden und dass die Replikation abgebrochen wird, bevor Sie versuchen, den ursprünglichen Datenträger abzubrechen.
 {:important}


## SLCLI-Befehle im Zusammenhang mit der Replikation
{: #clicommands}

* Geeignete Replikationsrechenzentren für einen bestimmten Datenträger auflisten.
  ```
  # slcli block replica-locations --help
  Syntax: slcli block replica-locations [OPTIONEN] DATENTRÄGER_ID

  Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, langer Name, kurzer Name
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Blockspeicherreplikatdatenträger bestellen.
  ```
  # slcli block replica-order --help
  Syntax: slcli block replica-order [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Snapshotzeitplan für die Replikation,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [erforderlich]
  -l, --location TEXT             Kurzname des Rechenzentrums für den
                                  Replikatdatenträger (z. B. dal09)  [erforderlich]
  --tier [0.25|2|4|10]            Endurance-Speichertier (IOPS pro GB) des primären
                                  Datenträgers, für den ein Replikatdatenträger
                                  bestellt wird [optional]
  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  Betriebssystemtyp (z. B. LINUX) des primären
                                  Datenträgers, für den ein Replikat bestellt
                                  wird [optional]
  -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Vorhandene Replikatdatenträger für einen Blockspeicherdatenträger auflisten.
  ```
  # slcli block replica-partners --help
  Syntax: slcli block replica-partners [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --sortby TEXT   Spalten für die Sortierung
  --columns TEXT  Spalten für die Anzeige. Optionen: ID, Benutzername, Konto-ID,
                  Kapazität (GB), Hardware-ID, Gastsystem-ID, Host-ID
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Failover eines Dateidatenträgers auf einen bestimmten Replikatdatenträger durchführen.
  ```
  # slcli block replica-failover --help
  Syntax: slcli block replica-failover [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --replicant-id TEXT  ID des Replikatdatenträgers
  --immediate          Sofortige Funktionsübernahme durch den Replikatdatenträger.
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
  ```

* Failback eines Blockdatenträgers von einem bestimmten Replikatdatenträger durchführen.
  ```
  # slcli block replica-failback --help
  Syntax: slcli block replica-failback [OPTIONEN] DATENTRÄGER-ID

  Optionen:
  --replicant-id TEXT  ID des Replikatdatenträgers
  -h, --help           Diese Nachricht anzeigen und Ausführung beenden.
  ```
