---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Daten replizieren

Bei der Replikation werden Snapshots mithilfe eines Ihrer Snapshotpläne automatisch auf einen Zieldatenträger in einem fernen Rechenzentrum kopiert. Im Fall beschädigter Daten oder einer Katastrophe können die Kopien an dem fernen Standort wiederhergestellt werden.

Mit Replikaten können Sie nach Standortausfällen und anderen Katastrophen schnell eine Wiederherstellung durchführen. Im Notfall kann der Zieldatenträger übernehmen und zu einem bestimmten Zeitpunkt in der Disaster Recovery-Kopie (DR-Kopie) auf Ihre Daten zugreifen. Weitere Informationen hierzu finden Sie unter [Replikatdatenträger für Disaster-Recovery duplizieren](disaster-recovery.html).

Die Replikation hält Ihre Daten an zwei verschiedenen Positionen synchron. Wenn Sie Ihren Datenträger klonen und unabhängig vom ursprünglichen Datenträger verwenden möchten, lesen Sie den Abschnitt [Duplikat des Blockdatenträgers erstellen](how-to-create-duplicate-volume.html).{:tip}

Um Replikationen durchführen zu können, müssen Sie einen Snapshotplan erstellen. Beim Failover 'kippen Sie den Schalter' von Ihrem Speicherdatenträger in Ihrem primären Rechenzentrum auf den Zieldatenträger in Ihrem fernen Rechenzentrum. Beispiel: Ihr primäres Rechenzentrum befindet sich in London und Ihr sekundäres Rechenzentrum in Amsterdam. Bei einem Fehlerereignis können Sie ein Failover nach Amsterdam durchführen und von einer Recheninstanz in Amsterdam eine Verbindung zu dem nun primären Datenträger herstellen. Nachdem Ihr Datenträger in London repariert wurde, wird von dem Datenträger in Amsterdam ein Snapshot erstellt, um von einer Recheninstanz in London eine Rückübertragung nach London und auf den nun wieder primären Datenträger durchzuführen.


## Fernes Rechenzentrum für replizierten Speicherdatenträger ermitteln

Die Rechenzentren von {{site.data.keyword.BluSoftlayer_full}} wurden weltweit in Paare aus einem primären und einem fernen Datenträger unterteilt.
In Tabelle 1 finden Sie eine vollständige Liste der Verfügbarkeit der Rechenzentren und der Replikationsziele.

<table>
  <caption style="text-align: left;"><p>Tabelle 1 - In dieser Tabelle wird eine vollständige Liste der Rechenzentren mit erweiterten Leistungsmerkmalen in jeder Region aufgeführt. Jede Region wird in einer separaten Spalte angegeben. In manchen Städten, wie zum Beispiel Dallas, San Jose, Washington DC, Amsterdam, Frankfurt, London und Sydney, befinden sich mehrere Rechenzentren.</p>
  <p>&#42; Rechenzentren in der Region US 1 verfügen NICHT über erweiterten Speicher. Hosts in Rechenzentren mit erweiterten Speicherleistungsmerkmalen können die Replikation mit Replikationszielen in Rechenzentren der Region US 1 <strong>nicht</strong> einleiten.</p>
  </caption>
  <thead>
    <tr>
      <th>US 1 &#42;</th>
      <th>US 2</th>
      <th>Lateinamerika</th>
      <th>Kanada</th>
      <th>Europa</th>
      <th>Asien/Pazifik</th>
      <th>Australien</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
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
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
	  MEL01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Erstreplikation erstellen

Replikationen werden auf der Basis eines Snapshotplans ausgeführt. Sie müssen zuerst über einen Snapshotbereich und einen Snapshotplan für den Quellendatenträger verfügen, um replizieren zu können. Wenn Sie versuchen, eine Replikation zu konfigurieren und sich der eine oder andere nicht an seiner Position befindet, werden Sie aufgefordert, mehr Speicherplatz zu erwerben oder einen Zeitplan einzurichten. Replikationen werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} verwaltet.

1. Klicken Sie auf Ihren Speicherdatenträger.
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf **Replikation kaufen**.
3. Wählen Sie einen vorhandenen Snapshotplan für Ihre Replikationen aus. Die Liste enthält alle Ihre aktiven Snapshotpläne. <br />
Sie können nur einen einzigen Plan auswählen, selbst wenn Sie einen Mix aus stündlicher, täglicher und wöchentlicher Rechnungsstellung haben. Alle seit dem letzten Replikationszyklus erfassten Snapshots werden unabhängig von dem Plan repliziert, aus dem sie stammen.<br />Wenn Sie keine Snapshots eingerichtet haben, werden Sie aufgefordert, dies vor der Bestellung der Replikation zu tun. Genauere Informationen hierzu finden Sie im Abschnitt [Mit Snapshots arbeiten](snapshots.html).
   {:important}
3. Klicken Sie auf **Position** und wählen Sie das Rechenzentrum aus, das Sie als Standort für das Disaster-Recovery-Standort verwenden möchten.
4. Klicken Sie auf **Weiter**.
5. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Fenster sind mit den Standardwerten gefüllt.
6. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen** und klicken Sie auf **Auftrag erteilen**.


## Vorhandene Replikation bearbeiten

Sie können entweder auf der Registerkarte **Primär** oder **Replikat** unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} einen Replikationsplan bearbeiten und den Replikationsbereich ändern.



## Replikationszeitplan bearbeiten

Der Replikationsplan basiert auf einem vorhandenen Snapshotplan. Um den Replikationsplan zu ändern, beispielsweise von 'Stündlich' in 'Wöchentlich', müssen Sie den Replikationsplan löschen und einen neuen einrichten.

Der Plan kann auf der Registerkarte 'Primär' oder 'Replikat' geändert werden.

1. Klicken Sie entweder in der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
2. Wählen Sie **Snapshotplan bearbeiten** aus.
3. Überprüfen Sie im Rahmen **Snapshot** die Angaben unter **Plan**, um festzustellen, welchen Plan Sie für die Replikation verwenden. Ändern Sie den gewünschten Zeitplan. Wenn Ihr Replikationsplan beispielsweise **Täglich** ist, können Sie die Uhrzeit ändern, zu der die Replikation ausgeführt werden soll.
4. Klicken Sie auf **Speichern**.


## Replikationsbereich ändern

Der primäre Replikationsbereich und der Replikatsbereich müssen identisch sein. Wenn Sie den Bereich auf der Registerkarte **Primär** oder **Replikat** ändern, wird automatisch Speicherplatz zu Ihrem Quellen- und Ihrem Zielrechenzentrum hinzugefügt. Wird der Snapshotbereich vergrößert, wird auch eine sofortige Aktualisierung der Replikation ausgelöst.

1. Klicken Sie entweder in der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
2. Wählen Sie **Mehr Snapshotbereich hinzufügen** aus.
3. Wählen Sie in der Liste die Speichergröße aus und klicken Sie auf **Weiter**.
4. Geben Sie einen **Werbeaktionscode** ein, sofern vorhanden, und klicken Sie auf **Neu berechnen**. Die anderen Felder im Dialogfeld nehmen die Standardwerte an.
5. Aktivieren Sie das Kontrollkästchen **Ich habe die Rahmenvereinbarung gelesen…** und klicken Sie auf **Auftrag erteilen**.


## Replikatdatenträger in der Datenträgerliste anzeigen

Sie können Ihre Replikationsdatenträger auf der Seite {{site.data.keyword.blockstorageshort}} unter **Speicher > {{site.data.keyword.blockstorageshort}}** anzeigen. Der **LUN-Name** weist den Namen des Primärdatenträgers auf, an den REP angehängt ist. Der **Typ** ist 'Endurance - Replikat' oder 'Performance - Replikat'. Die **Zieladresse** ist  'Nicht zutreffend', weil der Replikatdatenträger nicht im Replikatrechenzentrum angehängt ist, und der **Status** ist 'Inaktiv'.


## Details eines replizierten Datenträgers im Replikatrechenzentrum anzeigen

Sie können die Details des Replikatdatenträgers unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** auf der Registerkarte **Replikat** anzeigen. Eine andere Option besteht darin, den Replikatdatenträger auf der Seite **{{site.data.keyword.blockstorageshort}}**  auszuwählen und auf die Registerkarte **Replikat** zu klicken.


## Hostberechtigungen vor Failover des Servers auf sekundäres Rechenzentrum angeben

Autorisierte Hosts und Datenträger müssen sich in demselben Rechenzentrum befinden. Wenn sich der Replikatdatenträger in London befindet, kann sich der zugehörige Host nicht in Amsterdam befinden. Entweder müssen beide in London oder beide in Amsterdam sein.

1. Klicken Sie auf der Seite **{{site.data.keyword.blockstorageshort}}** auf den Quellen- oder Zieldatenträger.
2. Klicken Sie auf **Replikat**.
3. Blättern Sie nach unten zum Rahmen **Hosts autorisieren** und klicken Sie auf der rechten Seite auf **Hosts autorisieren**.
4. Heben Sie den Host hervor, der für Replikationen autorisiert werden soll. Halten Sie zur Auswahl mehrerer Hosts die Steuertaste gedrückt und klicken Sie auf die entsprechenden Hosts.
5. Klicken Sie auf **Übergeben**. Wenn Sie über keine Hosts verfügen, werden Sie aufgefordert, Rechenressourcen in demselben Rechenzentrum zu kaufen.


## Snapshotbereich im Replikatrechenzentrum erhöhen, wenn der Snapshotbereich im primären Rechenzentrum erhöht wird

Ihr primärer Datenträger und der Replikatspeicherdatenträger müssen dieselbe Datenträgergröße aufweisen. Der eine darf nicht größer sein als der andere. Wenn Sie Ihren Snapshotbereich für Ihren Primärdatenträger erhöhen, wird der Replikatbereich automatisch erhöht. Wird der Snapshotbereich vergrößert, wird eine sofortige Aktualisierung der Replikation ausgelöst. Die Erhöhung der beiden Datenträger wird auf Ihrer Rechnung als Artikelposition angezeigt und bei Bedarf anteilig berechnet.

Weitere Informationen zum Vergrößern des Snapschotbereichs finden Sie unter [Snapshots bestellen](ordering-snapshots.html).{:tip}


## Failover von einem Datenträger auf dessen Replikat starten

Bei einem Fehlerereignis können Sie einen **Failover** für Ihr Ziel bzw. Ihren Datenträger starten. Der Zieldatenträger wird aktiv. Der letzte erfolgreich replizierte Snapshot wird aktiviert und der Datenträger wird zum Anhängen verfügbar gemacht. Alle seit dem letzten Replikationszyklus auf den Quellendatenträger geschriebenen Daten werden gelöscht. Mit dem Starten eines Failovers wird die Replikationsbeziehung umgedreht. Ihr Zieldatenträger ist nun Ihr Quellendatenträger und Ihr früherer Quellendatenträger wird Ihr Ziel, wie durch den **LUN-Namen** mit angehängtem **REP** angezeigt.

Failover werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} gestartet.

**Bevor Sie mit den folgenden Schritten fortfahren, unterbrechen Sie die Verbindung zum Datenträger. Wenn Sie das nicht tun, sind Datenbeschädigungen und/oder Datenverlust die Folge.**

1. Klicken Sie auf Ihre aktive LUN ("Quelle").
2. klicken Sie auf **Replikat** und klicken Sie in der rechten oberen Ecke auf den Link **Aktionen**.
3. Wählen Sie **Failover** aus.
   >Oben auf der Seite wird eine Nachricht angezeigt, die besagt, dass der Failover in Bearbeitung ist. Außerdem wird neben Ihrem Datenträger auf dem **{{site.data.keyword.blockstorageshort}}** ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie den Mauszeiger über das Symbol bewegen, wird ein Fenster mit Angaben zur Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. Während der Failover-Verarbeitung sind konfigurationsrelevante Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten und Snapshotbereiche nicht ändern. Das Ereignis wird im Replikationsprotokoll protokolliert.<br/> Wenn der Zieldatenträger aktiv ist, wird eine andere Nachricht angezeigt. Der LUN-Name des ursprünglichen Quellendatenträgers wird aktualisiert und weist jetzt die Endung 'REP' auf, sein Status wechselt zu 'Inaktiv'.
4. Klicken Sie auf **Alles anzeigen ({{site.data.keyword.blockstorageshort}})**.
5. Klicken Sie auf Ihre aktive LUN (zuvor Ihr Zieldatenträger).
6. Hängen Sie Ihren Speicherdatenträger an und verbinden Sie ihn mit dem Host. Die Anweisungen dazu finden Sie [hier](provisioning-block_storage.html).


## Rückübertragung von einem Datenträger auf dessen Replikat starten

Sobald Ihr Originalquellendatenträger repariert ist, können Sie eine gesteuerte Rückübertragung auf Ihren Originalquellendatenträger starten. Bei einer gesteuerten Rückübertragung geschieht Folgendes:

- Der aktive Quellendatenträger wird offline geschaltet.
- Ein Snapshot wird erstellt.
- Der Replikationszyklus ist abgeschlossen.
- Der soeben erstellte Datensnapshot wird aktiviert.
- Der Quellendatenträger wird zum Anhängen aktiviert.

Mit dem Starten der Rückübertragung wird die Replikationsbeziehung wieder umgedreht. Ihr Quellendatenträger wird als Ihr Quellendatenträger wiederhergestellt und Ihr Zieldatenträger ist wieder der Zieldatenträger, wie durch **LUN-Name** und angehängtem **REP** angezeigt.

Rückübertragungen werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} gestartet.

1. Klicken Sie auf Ihre aktive LUN ('Ziel').
2. Klicken Sie oben rechts auf **Replikat** und danach auf **Aktionen**.
3. Wählen Sie **Rückübertragung** aus. Oben auf der Seite wird eine Nachricht angezeigt, die besagt, dass der Failover in Bearbeitung ist. Außerdem wird neben Ihrem Datenträger auf dem **{{site.data.keyword.blockstorageshort}}** ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie den Mauszeiger über das Symbol bewegen, wird ein Fenster mit Angaben zur Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. Während des Rückübertragungsprozesses sind konfigurationsrelevante Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten und Snapshotbereiche nicht ändern. Das Ereignis wird im Replikationsprotokoll protokolliert.
4. Klicken Sie rechts oben auf den Link **Alles anzeigen ({{site.data.keyword.blockstorageshort}})**.
5. Klicken Sie auf Ihre aktive LUN ('Quelle').
6. Hängen Sie Ihren Speicherdatenträger an und verbinden Sie ihn mit dem Host. Die Anweisungen dazu finden Sie [hier](provisioning-block_storage.html).


## Replikationsprotokoll anzeigen

Das Replikationsprotokoll kann im **Auditprotokoll** auf der Registerkarte **Konto** im Bereich **Verwalten** angezeigt werden. Auf dem Primär- und dem Replikatdatenträger wird ein identisches Replikationsprotokoll angezeigt. Das Protokoll umfasst folgende Angaben:

- Art der Replikation (Failover oder Rückübertragung)
- Startzeitpunkt
- Snapshot, der für die Replikation verwendet wurde
- Größe der Replikation
- Beendigungszeitpunkt


## Duplikat eines Replikats erstellen

Sie können ein Duplikat eines vorhandenen {{site.data.keyword.BluSoftlayer_full}}-{{site.data.keyword.blockstoragefull}}s erstellen. Das Duplikat übernimmt standardmäßig die Kapazitäts- und Leistungsoptionen der Original-LUN bzw. des Originaldatenträgers und enthält bis zum Zeitpunkt eines Snapshots eine Kopie der Daten.

Duplikate können sowohl für den Primär- als auch für den Replikatdatenträger erstellt werden. Das neue Duplikat wird in demselben Rechenzentrum wie der Originaldatenträger erstellt. Wenn Sie ein Duplikat von einem Replikatdatenträger erstellen, wird der neue Datenträger in demselben Rechenzentrum erstellt wie der Replikatdatenträger.

Der Lese- und Schreibzugriff auf duplizierte Datenträger kann durch einen Host erfolgen, sobald der Speicher bereitgestellt wurde. Snapshots und Replikation sind dagegen erst zulässig, nachdem die Daten vollständig vom Original auf das Duplikat kopiert wurden.

Weitere Informationen finden Sie unter [Duplikat eines Blockdatenträgers erstellen](how-to-create-duplicate-volume.html).


## Vorhandene Replikation abbrechen

Der Abbruch kann sofort oder am Stichtag erfolgen und bewirkt das Ende der Abrechnung. Die Replikation kann auf der Registerkarte **Primär** oder **Replikat** abgebrochen werden.

1. Klicken Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' auf den Datenträger.
2. Klicken Sie entweder in der Registerkarte **Primär** oder **Replikat** auf **Aktionen**
3. Wählen Sie **Replikat abbrechen** aus.
4. Wählen Sie den Zeitpunkt des Abbruchs aus. Wählen Sie **Sofort** oder **Stichtag** aus und klicken Sie auf **Weiter**.
5. Aktivieren Sie **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Replikat abbrechen**.


## Replikation beim Abbrechen des Primärdatenträgers abbrechen

Wenn ein Primärdatenträger abgebrochen wird, werden der Replikationsplan und der Datenträger im Replikatrechenzentrum gelöscht. Replikate werden auf der Seite '{{site.data.keyword.blockstorageshort}}' abgebrochen.

 1. Heben Sie auf der Seite '**{{site.data.keyword.blockstorageshort}}**' Ihren Datenträger hervor.
 2. Klicken Sie auf **Aktionen** und wählen Sie **{{site.data.keyword.blockstorageshort}} abbrechen** aus.
 3. Wählen Sie den Zeitpunkt des Abbruchs aus. Wählen Sie **Sofort** oder **Stichtag** aus und klicken Sie auf **Weiter**.
 4. Aktivieren Sie **Ich bestätige, dass der Abbruch einen Datenverlust zur Folge haben kann** und klicken Sie auf **Abbrechen**.
