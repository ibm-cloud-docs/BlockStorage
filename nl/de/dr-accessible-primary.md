---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, accessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Disaster-Recovery - Failover mit einem zugänglichen Primärdatenträger
{: #dr-accessible}

Wenn eine Betriebsunterbrechung oder einer Katastrophe, die einen Ausfall am primären Standort verursacht, auftritt und der primäre Speicher noch zugänglich ist, können Kunden die folgenden Aktionen ausführen, um schnell am sekundären Standort auf ihre Daten zuzugreifen.

Bevor Sie mit dem Failover beginnen, stellen Sie sicher, dass die gesamte Hostberechtigung in Kraft ist.
{:important}

Autorisierte Hosts und Datenträger müssen sich in demselben Rechenzentrum befinden. Wenn sich der Replikatdatenträger beispielsweise in London befindet, kann sich der zugehörige Host nicht in Amsterdam befinden. Entweder müssen beide in London oder beide in Amsterdam sein.
{:note}

1. Melden Sie sich an der [{{site.data.keyword.cloud_notm}}-Konsole](https://{DomainName}/){: external} an und klicken Sie auf das **Menüsymbol** oben links. Wählen Sie **Klassische Infrastruktur** aus.
2. Klicken Sie auf der Seite **{{site.data.keyword.blockstorageshort}}** auf den Quellen- oder Zieldatenträger.
3. Klicken Sie auf **Replikat**.
4. Blättern Sie nach unten zum Rahmen **Hosts autorisieren** und klicken Sie auf der rechten Seite auf **Hosts autorisieren**.
5. Heben Sie den Host hervor, der für Replikationen autorisiert werden soll. Halten Sie zur Auswahl mehrerer Hosts die Steuertaste gedrückt und klicken Sie auf die entsprechenden Hosts.
6. Klicken Sie auf **Übergeben**. Wenn Sie über keine verfügbaren Hosts verfügen, werden Sie aufgefordert, Rechenressourcen in demselben Rechenzentrum zu kaufen.


## Failover von einem Datenträger auf dessen Replikat starten

Bei einem Fehlerereignis können Sie einen **Failover** für Ihr Ziel bzw. Ihren Datenträger starten. Der Zieldatenträger wird aktiv. Der letzte erfolgreich replizierte Snapshot wird aktiviert und der Datenträger wird zum Anhängen verfügbar gemacht. Alle seit dem letzten Replikationszyklus auf den Quellendatenträger geschriebenen Daten werden gelöscht.

Mit dem Starten eines Failovers wird die Replikationsbeziehung umgedreht. Ihr Zieldatenträger ist nun Ihr Quellendatenträger und Ihr früherer Quellendatenträger wird Ihr Ziel, wie durch den **LUN-Namen** mit angehängtem **REP** angezeigt.

Failovers werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} gestartet.

Bevor Sie mit den folgenden Schritten fortfahren, unterbrechen Sie die Verbindung zum Datenträger. Wenn Sie das nicht tun, sind Datenbeschädigungen und/oder Datenverlust die Folge.
{:important}

1. Klicken Sie auf Ihre aktive LUN ("Quelle").
2. Klicken Sie auf die Registerkarte **Replikat** und klicken Sie auf **Aktionen**.
3. Wählen Sie **Failover** aus.

   Auf der Seite wird eine Nachricht angezeigt, die besagt, dass der Failover in Bearbeitung ist. Außerdem wird neben Ihrem Datenträger auf der Seite **{{site.data.keyword.blockstorageshort}}** ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie den Mauszeiger über das Symbol bewegen, wird ein Fenster mit Angaben zur Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. Während der Failover-Verarbeitung sind konfigurationsrelevante Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten und Snapshotbereiche nicht ändern. Das Ereignis wird im Replikationsprotokoll protokolliert.<br/> Wenn der Zieldatenträger aktiv ist, wird eine andere Nachricht angezeigt. Der LUN-Name des ursprünglichen Quellendatenträgers wird aktualisiert und weist jetzt die Endung 'REP' auf, sein Status wechselt zu 'Inaktiv'.
   {:note}
4. Klicken Sie auf den Link **Alle {{site.data.keyword.blockstorageshort}}-Instanzen anzeigen**.
5. Klicken Sie auf Ihre aktive LUN (Ihr vorheriger Zieldatenträger).
6. Hängen Sie Ihren Speicherdatenträger an und verbinden Sie ihn mit dem Host. Weitere Informationen finden Sie in [Verbindung zum Speicher herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#mountingnewLUN).


## Rückübertragung von einem Datenträger auf dessen Replikat starten

Sobald Ihr Originalquellendatenträger repariert ist, können Sie eine gesteuerte Rückübertragung auf Ihren Originalquellendatenträger starten. Bei einer gesteuerten Rückübertragung (Failback) geschieht Folgendes:

- Der aktive Quellendatenträger wird offline geschaltet.
- Ein Snapshot wird erstellt.
- Der Replikationszyklus wird abgeschlossen.
- Der soeben erstellte Datensnapshot wird aktiviert.
- Der Quellendatenträger wird zum Anhängen aktiviert.

Mit dem Starten der Rückübertragung wird die Replikationsbeziehung wieder umgedreht. Ihr Quellendatenträger wird als Ihr Quellendatenträger wiederhergestellt und Ihr Zieldatenträger ist wieder der Zieldatenträger, wie durch **LUN-Name** und angehängtem **REP** angezeigt.

Rückübertragungen (Failbacks) werden unter **Speicher**, **{{site.data.keyword.blockstorageshort}}** in der [{{site.data.keyword.cloud}}-Konsole](https://{DomainName}/classic){: external} gestartet.

1. Klicken Sie auf Ihre aktive LUN ('Ziel').
2. Klicken Sie oben rechts auf **Replikat** und danach auf **Aktionen**.
3. Wählen Sie **Rückübertragung** aus.

   Auf der Seite wird eine Nachricht angezeigt, die besagt, dass der Failover in Bearbeitung ist. Außerdem wird neben Ihrem Datenträger auf dem **{{site.data.keyword.blockstorageshort}}** ein Symbol angezeigt, das angibt, dass eine aktive Transaktion läuft. Wenn Sie den Mauszeiger über das Symbol bewegen, wird ein Fenster mit Angaben zur Transaktion angezeigt. Sobald die Transaktion abgeschlossen ist, wird das Symbol ausgeblendet. Während des Rückübertragungsprozesses sind konfigurationsrelevante Aktionen schreibgeschützt. Sie können Snapshotpläne nicht bearbeiten und Snapshotbereiche nicht ändern. Das Ereignis wird im Replikationsprotokoll protokolliert.
   {:note}
4. Klicken Sie rechts oben auf **Alle {{site.data.keyword.blockstorageshort}}-Instanzen anzeigen**.
5. Klicken Sie auf Ihre aktive LUN ('Quelle').
6. Hängen Sie Ihren Speicherdatenträger an und verbinden Sie ihn mit dem Host. Weitere Informationen finden Sie in [Verbindung zum Speicher herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#mountingnewLUN).
