---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="DomainName"}

# Speichergrenzwerte verwalten
{: #managingstoragelimits}

Standardmäßig können Sie global insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen.

Wenn Sie die Anzahl Ihrer Datenträger ermitteln möchten, können Sie mit dem folgenden `slcli`-Befehl Ihre Datenträger für die einzelnen Rechenzentren auflisten.
```
# slcli block volume-count --help
Syntax: slcli block volume-count [OPTIONEN]

Optionen:
  -d, --datacenter TEXT  Kurzname des Rechnzentrums
  --sortby TEXT          Spalten für die Sortierung
  -h, --help             Diese Nachricht anzeigen und Ausführung beenden.
```

Sie können eine Erhöhung des Grenzwerts anfordern, indem Sie einen Supportfall über die [Konsole](https://{DomainName}/unifiedsupport/cases/add){: external} öffnen. Wenn die Anforderung genehmigt wurde, wird ein Datenträgergrenzwert für ein bestimmtes Rechenzentrum festgelegt.  

Um eine Erhöhung eines Grenzwerts anzufordern, müssen Sie einen Fall öffnen und an Ihren Vertriebsbeauftragten weiterleiten.

Geben Sie in dem Fall die folgenden Informationen an:

- **Betreff des Tickets:** Anforderung zur Erhöhung des Speichergrenzwerts für Datenträger im Rechenzentrum

- **Was ist der Anwendungsfall für die Anforderung zusätzlicher Datenträger?** <br />
*Ihre Antwort könnte beispielsweise ein neuer VMware-Datenspeicher, eine neue Entwicklungs- bzw. Testumgebung (Dev/Test), eine SQL-Datenbank, Protokollierung etc. sein.*

- **Wie viele zusätzliche Blockdatenträger werden benötigt, nach Typ, Größe, IOPS und Position?** <br />
*Ihre Antwort könnte beispielsweise "25x Endurance 2TB @ 4 IOPS bei DAL09" oder "25x Performance 4TB @ 2 IOPS bei WDC04" sein.*

- **Wie viele zusätzliche Dateidatenträger werden benötigt, nach Typ, Größe, IOPS und Position?** <br />
*Ihre Antwort könnte beispielsweise "25x Performance 20GB @ 10 IOPS in DAL09" oder "50x Endurance 2TB @ 0,25 IOPS in SJC03" sein.*

- **Geben Sie einen geschätzten Zeitpunkt an, zu dem Sie erwarten, dass alle angeforderten Datenträgervergrößerungen bereitgestellt oder geplant sind.** <br />
*Ihre Antwort könnte beispielsweise "90 Tage" sein. *

- **Treffen Sie eine Prognose über die erwartete durchschnittliche Kapazitätsnutzung dieser Datenträger in den nächsten 90 Tagen.** <br />
*Ihre Antwort könnte beispielsweise sein: "Nutzung von 25 Prozent in 30 Tagen, von 50 Prozent in 60 Tagen und von 75 Prozent in 90 Tagen".*

Beantworten Sie alle Fragen und Aussagen in Ihrer Anfrage. Sie sind für die Verarbeitung und Genehmigung erforderlich.
{:important}

Sie werden von der Aktualisierung Ihrer Grenzwerte durch den Fall-Prozess benachrichtigt.
