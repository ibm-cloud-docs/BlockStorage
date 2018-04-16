---
 
copyright:
  years: 2015, 2018
lastupdated: "2018-04-16"
 
---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"}

# Mit Replikation arbeiten

Bei der Replikation werden Snapshots mithilfe eines Ihrer Snapshotpläne automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum kopiert. Im Fall beschädigter Daten oder einer Katastrophe können die Kopie an dem fernen Standort wiederhergestellt werden.

Replikate bieten Ihnen folgende Möglichkeiten: 

- Per Failover auf den Zieldatenträger eine schnelle Recovery nach Siteausfällen und anderen Katastrophen durchführen
- Failover auf einen bestimmten Zeitpunkt in der Disaster-Recovery-Kopie durchführen

Um Replikationen durchführen zu können, müssen Sie einen Snapshotplan erstellen. Beim Failover 'kippen Sie den Schalter' von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Beispiel: Ihr primäres Rechenzentrum befindet sich in London und Ihr sekundäres Rechenzentrum in Amsterdam. Bei einem Fehlerereignis können Sie ein Failover nach Amsterdam durchführen und von einer Recheninstanz in Amsterdam eine Verbindung zu dem nun primären Datenträger herstellen. Nachdem Ihr Datenträger in London repariert wurde, wird von dem Datenträger in Amsterdam ein Snapshot erstellt, um von einer Recheninstanz in London eine Rückübertragung nach London und auf den nun wieder primären Datenträger durchzuführen.

 
**Hinweis:** Sofern nicht anders vermerkt, sind die Schritte für {{site.data.keyword.blockstoragefull}} und File Storage identisch.

## Wie bestimmte ich das ferne Rechenzentrum für meinen replizierten Speicherdatenträger?

Die Rechenzentren von {{site.data.keyword.BluSoftlayer_full}} wurden weltweit in Paare aus einem primären und einem fernen Datenträger unterteilt.
In Tabelle 1 finden Sie eine vollständige Liste der Verfügbarkeit der Rechenzentren und der Replikationsziele.
Beachten Sie, dass einige in Tabelle 1 aufgeführten Städte wie Dallas, San Jose, Washington, D.C. und Amsterdam mehrere Rechenzentren haben.


<table cellpadding="1" cellspacing="1">
	<caption>Tabelle 1</caption>
	<tbody>
		<tr>
			<td><strong>US 1</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>US 2</strong></td>
			<td><strong>Latein-/Südamerika</strong></td>
			<td><strong>Kanada</strong></td>
			<td><strong>Europa</strong></td>
			<td><strong>Asien/Pazifik</strong></td>
			<td><strong>Australien</strong></td>
		</tr>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br />
			</td>
			<td>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%"><p><sup><img src="/images/numberone.png" alt="1" /></sup>Rechenzentren in diesen Regionen haben KEINEN verschlüsselten Speicher.<br /><strong>Hinweis</strong>: Rechenzentren mit verschlüsseltem Speicher können die Replikation mit nicht verschlüsselten Rechenzentren als Replikatzielen <strong>nicht</strong> einleiten.</p>
			</td>
		</tr>
	</tbody>
</table>
 

## Wie erstelle ich eine Erstreplikation?

Replikationen werden auf der Basis eines Snapshotplans ausgeführt. Sie müssen zuerst einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger einrichten, um replizieren zu können. Sie erhalten Eingabeaufforderungen bezüglich des zu kaufenden Speicherplatzes oder des einzurichtenden Plans, wenn Sie versuchen, eine Replikation einzurichten und eines der beiden Elemente fehlt. Replikationen werden unter Speicher, {{site.data.keyword.blockstorageshort}} oder Dateispeicher über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Registerkarte **Replikat** und auf den Link  **Replikation kaufen**.
Wählen Sie einen vorhandenen Snapshotplan für Ihre Replikationen aus. Die Liste enthält alle Ihre aktiven Snapshotpläne. <br />
  **Hinweis:** Sie können nur einen einzigen Plan auswählen, auch wenn Sie eine Mischung aus stündlich, täglich und wöchentlich haben. Alle seit dem letzten Replikationszyklus erfassten Snapshots werden unabhängig von dem Plan repliziert, aus dem sie stammen.<br />
  **Hinweis:** Wenn Sie keine Snapshots eingerichtet haben, werden Sie aufgefordert, dies vor der Bestellung der Replikation zu tun. Genauere Informationen hierzu finden Sie im Abschnitt [Mit Snapshots arbeiten](snapshots.html).
3. Klicken Sie auf den Dropdown-Pfeil **Position** und wählen Sie das Rechenzentrum aus, das Ihr DR-Standort sein soll.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfeld nehmen die Standardwerte an.
6. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Auftrag erteilen**. 
 

## Wie bearbeite ich eine vorhandene Replikation?

Sie können über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** auf der Registerkarte **Primär** oder **Replikat** Ihren Replikationsplan bearbeiten und Ihren Replikationsbereich ändern.

 

## Wie bearbeite ich einen Replikationsplan?

Sie ändern eigentlich einen Snapshotplan, weil Ihr Replikationsplan auf einem vorhandenen Snapshotplan basiert. Um den Replikationsplan zu ändern, beispielsweise von 'Stündlich' in 'Wöchentlich', müssen Sie den Replikationsplan löschen und einen neuen einrichten.

Der Plan kann auf der Registerkarte 'Primär' oder 'Replikat' geändert werden.

1. Klicken Sie auf der Registerkarte **Primär** oder **Replikat** auf das Dropdown-Menü **Aktionen**.
2. Wählen Sie **Snapshotplan bearbeiten** aus.
3. Schauen Sie im Rahmen 'Snapshots' unter 'Plan' nach, welchen Plan Sie für die Replikation verwenden wollen. Nehmen Sie an dem Plan, der für die Replikation verwendet wird, die Änderungen vor. Wenn Ihr Replikationsplan beispielsweise **Täglich** ist, können Sie die Uhrzeit ändern, zu der die Replikation ausgeführt werden soll.
4. Klicken Sie auf **Speichern**. 
 

## Wie ändere ich den Replikationsbereich?

Ihr primärer Replikationsbereich und Ihr Replikatsbereich müssen identisch sein. Wenn Sie den Bereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird automatisch Speicherplatz zu Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Beachten Sie, dass die Erhöhung des Snapshotbereichs eine sofortige Replikationsaktualisierung auslöst.

Klicken Sie auf der Registerkarte **Primär** oder **Replikat** auf das Dropdown-Menü **Aktionen**.
Wählen Sie **Mehr Snapshotbereich hinzufügen** aus.
Wählen Sie in der Liste die Speichergröße aus und klicken Sie auf **Weiter**.
Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfeld nehmen die Standardwerte an.
Aktivieren Sie das Kontrollkästchen 'Ich habe die Rahmenvereinbarung gelesen…' und klicken Sie auf die Schaltfläche 'Auftrag erteilen'.
 

## Wie kann ich meine Replikatdatenträger in der Datenträgerliste anzeigen?

Sie können Ihre Replikationsdatenträger auf der Seite '{{site.data.keyword.blockstorageshort}}' unter **Speicher > {{site.data.keyword.blockstorageshort}}** anzeigen. Der LUN-Name hat den Namen des Primärdatenträgers, an den REP angehängt ist. Der Typ ist 'Endurance(Performance) – Replikat', die Zieladresse ist 'Nicht zutreffend', weil der Replikatdatenträger nicht im Replikatrechenzentrum angehängt ist, und der Status ist 'inaktiv'.

 

## Wie kann ich im Replikatrechenzentrum die Details eines replizierten Datenträgers anzeigen?

Sie können die Details des Replikatdatenträgers unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** auf der Registerkarte **Replikat** anzeigen. Eine andere Option besteht darin, den Replikatdatenträger auf der Seite '**{{site.data.keyword.blockstorageshort}}**' auszuwählen und auf die Registerkarte **Replikat** zu klicken.

 

## Wie gebe ich Hostberechtigungen vor dem Failover auf das sekundäre Rechenzentrum an?

Autorisierte Hosts und Datenträger müssen sich in demselben Rechenzentrum befinden. Sie können nicht einen Replikatdatenträger in London und den Host in Amsterdam haben. Beide müssen in London oder in Amsterdam sein.

1. Klicken Sie auf der entsprechenden Seite '**{{site.data.keyword.blockstorageshort}}**' auf Ihren Quellen- oder Zieldatenträger.
2. Klicken Sie auf die Registerkarte **Replikat**.
3. Blättern Sie abwärts bis zum Rahmen **Hosts autorisieren** und klicken Sie rechts in der Anzeige auf den Link **Hosts autorisieren**.
4. Heben Sie den Host hervor, der für Replikationen autorisiert werden soll. Halten Sie zur Auswahl mehrerer Hosts die Steuertaste gedrückt und klicken Sie auf die entsprechenden Hosts.
5. Klicken Sie auf **Übergeben**. Wenn Sie keine Hosts haben, bieten Ihnen das Dialogfeld die Option, Rechenressourcen in demselben Rechenzentrum zu kaufen oder auf **Schließen** zu klicken.
 

## Wie erhöhe ich meinen Snapshotbereich in einem Replikatrechenzentrum, wenn ich den Speicherplatz in meinem primären Rechenzentrum erhöhe?

Ihr primärer Datenträger und der Replikatspeicherdatenträger müssen dieselbe Datenträgergröße haben. Der eine darf nicht größer sein als der andere. Wenn Sie Ihren Snapshotbereich für Ihren Primärdatenträger erhöhen, wird der Replikatbereich automatisch erhöht. Beachten Sie, dass die Erhöhung des Snapshotbereichs eine sofortige Replikationsaktualisierung auslöst. Die Erhöhung der beiden Datenträger wird auf Ihrer Rechnung als Artikelposition angezeigt und bei Bedarf anteilig berechnet.

Klicken Sie [hier](snapshots.html), um Informationen zur Erhöhung Ihres Snapshotbereichs zu erhalten.

 

## Wie leite ich einen Failover von einem Datenträger auf dessen Replikat ein?

Bei einem Fehlerereignis können Sie mit der Aktion **Failover** einen Failover auf Ihren Zieldatenträger einleiten. Der Zieldatenträger wird aktiv, der letzte erfolgreich replizierte Snapshot wird aktiviert und der Datenträger wird zum Anhängen aktiviert. Alle seit dem letzten Replikationszyklus auf den Quellendatenträger geschriebenen Daten werden gelöscht. Beachten Sie, dass mit dem Einleiten eines Failovers die **Replikationsbeziehung kippt**. Ihr Zieldatenträger ist nun Ihr Quellendatenträger und Ihr früherer Quellendatenträger wird Ihr Ziel, wie durch den **LUN-Namen** mit angehängtem **REP** angezeigt.

Failover werden über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** eingeleitet.

**Es empfiehlt sich, den Datenträger zu trennen, bevor Sie mit diesem Prozess fortfahren. Wenn Sie das nicht tun, sind Datenbeschädigung und/oder Datenverlust die Folge.**

1. Klicken Sie auf Ihre aktive LUN ('Quelle').
2. Klicken Sie auf die Registerkarte **Replikat** und in der rechten oberen Ecke auf den Link **Aktionen**.
3. Wählen Sie 'Failover' aus.
 Oben auf der Seite wird eine Nachricht angezeigt, dass der Failover in Bearbeitung ist. Außerdem wird neben Ihrem Datenträger auf dem **{{site.data.keyword.blockstorageshort}}** ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie die Maus über das Symbol bewegen, wird ein Dialogfeld mit Angaben zu der Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. Während des Failover-Prozesses sind konfigurationsrelevante Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten, Snapshotbereiche nicht ändern etc. Das Ereignis wird im Replikationsprotokoll protokolliert.
 Eine andere Nachricht informiert Sie, wenn Ihr Zieldatenträger aktiv ist. An den LUN-Namen Ihres Originalquellendatenträgers wird REP angehängt, sein Status ist 'Inaktiv'.
4. Klicken Sie in in der rechten oberen Ecke auf den Speicherlink **Alles anzeigen** (**{{site.data.keyword.blockstorageshort}}**).
5. Klicken Sie auf Ihre aktive LUN (zuvor Ihr Zieldatenträger). Dieser Datenträger hat nun den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger an den Host an. Die Anweisungen dazu finden Sie [hier](provisioning-block_storage.html).
 

## Wie leite ich eine Rückübertragung von einem Datenträger auf dessen Replikat ein?

Sobald Ihr Originalquellendatenträger repariert ist, können Sie mit der Aktion **Rückübertragung** eine gesteuerte Rückübertragung auf Ihren Originalquellendatenträger einleiten. Bei einer gesteuerten Rückübertragung geschieht Folgendes:

- Der aktive Quellendatenträger wird offline geschaltet.
- Ein Snapshot wird gemacht.
- Der Replikationszyklus wird abgeschlossen.
- Der gerade gemachte Datensnapshot wird aktiviert.
- Der Quellendatenträger wird zum Anhängen aktiviert.

Beachten Sie, dass mit dem Einleiten einer Rückübertragung die **Replikationsbeziehung wieder kippt**. Ihr Quellendatenträger wird als Ihr Quellendatenträger wiederhergestellt und Ihr Zieldatenträger ist wieder der Zieldatenträger, wie durch den **LUN-Namen** mit angehängtem **REP** angezeigt.

Rückübertragungen werden über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** eingeleitet.

1. Klicken Sie auf Ihre aktive Endurance-LUN ('Ziel').
2. Klicken Sie auf die Registerkarte **Replikat** und in der rechten oberen Ecke auf den Link **Aktionen**.
3. Wählen Sie **Rückübertragung** aus.
 Oben auf der Seite wird eine Nachricht angezeigt, dass der Failover in Bearbeitung ist. Außerdem wird neben Ihrem Datenträger auf dem **{{site.data.keyword.blockstorageshort}}** ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie die Maus über das Symbol bewegen, wird ein Dialogfeld mit Angaben zu der Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. Während der Rückübertragung sind konfigurationsrelevante Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten, Snapshotbereiche nicht ändern etc. Das Ereignis wird im Replikationsprotokoll protokolliert.
 Eine andere Nachricht informiert Sie, wenn Ihr Quellendatenträger aktiv ist. Ihr Zieldatenträger hat nun den Status 'Inaktiv'.
4. Klicken Sie in in der rechten oberen Ecke auf den Link **Alles anzeigen** (**{{site.data.keyword.blockstorageshort}}**).
5. Klicken Sie auf Ihre aktive Endurance-LUN (Quelle). Dieser Datenträger hat nun den Status **Aktiv**.
6. Hängen Sie Ihren Speicherdatenträger an den Host an. Die Anweisungen dazu finden Sie [hier](provisioning-block_storage.html).
 

## Wie zeige ich mein Replikationsprotokoll an?

Das Replikationsprotokoll wird im **Auditprotokoll** auf der Registerkarte **Konto** im Bereich **Verwalten** angezeigt. Auf dem Primär- und dem Replikatdatenträger wird ein identisches Replikationsprotokoll mit den folgenden Angaben angezeigt:

- Art der Replikation (Failover oder Rückübertragung)
- Startzeitpunkt
- Snapshot, der für die Replikation verwendet wurde
- Größe der Replikation
- Beendigungszeitpunkt
 

## Wie breche ich eine vorhandene Replikation ab?

Der Abbruch kann sofort oder am Stichtag erfolgen und bewirkt das Ende der Abrechnung. Die Replikation kann auf der Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' auf den Datenträger.
2. Klicken Sie auf der Registerkarte **Primär** oder **Replikat** auf das Dropdown-Menü **Aktionen**.
3. Wählen Sie **Replikat abbrechen** aus.
4. Wählen Sie den Zeitpunkt des Abbruchs aus – **Sofort** oder **Stichtag** - und klicken Sie auf **Weiter**.
5. Aktivieren Sie das Kontrollkästchen **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Replikat abbrechen**.
 

## Wie breche ich die Replikation ab, wenn der Primärdatenträger abgebrochen wird?

Wenn ein Primärdatenträger abgebrochen wird, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite '{{site.data.keyword.blockstorageshort}}' abgebrochen.

 1. Heben Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' Ihren Datenträger hervor.
 2. Klicken Sie auf das Dropdown-Menü **Aktionen** und wählen Sie **Abbrechen{{site.data.keyword.blockstorageshort}}** aus.
 3. Wählen Sie den Zeitpunkt des Abbruchs des Datenträgers aus – **Sofort** oder **Stichtag** - und klicken Sie auf **Weiter**.
 4. Aktivieren Sie das Kontrollkästchen **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Abbrechen**.
