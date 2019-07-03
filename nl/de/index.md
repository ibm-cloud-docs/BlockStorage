---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Informationen zu {{site.data.keyword.blockstorageshort}}
{: #About}

Bei {{site.data.keyword.blockstoragefull}} handelt es sich um persistenten iSCSI-Speicher mit hoher Leistung, der unabhängig von Recheninstanzen bereitgestellt und verwaltet wird. iSCSI-basierte {{site.data.keyword.blockstorageshort}}-LUNs sind über MPIO-Verbindungen (MPIO - Multipath I/O) mit autorisierten Geräten verbunden.

{{site.data.keyword.blockstorageshort}} bietet dank seiner unvergleichlichen Funktionen leistungsfähige Dauerhaftigkeit und Verfügbarkeit. Basis sind Branchenstandards und bewährte Verfahren. {{site.data.keyword.blockstorageshort}} wurde dafür konzipiert, bei Wartungsereignissen und ungeplanten Ausfällen die Integrität der Daten zu schützen und die Verfügbarkeit aufrechtzuerhalten und gleichzeitig eine konsistente Leistungsbasis bereitzustellen.

## Zentrale Funktionen
{: #corefeatures}

Nutzen Sie die folgenden Funktionen von {{site.data.keyword.blockstorageshort}}:

- **Konsistente Leistungsbasis**
   - Wird durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern erreicht.
- **Äußerst dauerhaft und ausfallsicher**
   - Schützt bei Wartungsereignissen und ungeplanten Ausfällen die Integrität der Daten, hält die Verfügbarkeit aufrecht und stellt gleichzeitig eine konsistente Leistungsbasis bereit.
- **Verschlüsselung ruhender Daten** ([in den meisten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Anbietergesteuerte Verschlüsselung ruhender Daten ohne Zusatzkosten.
- **Gesamter durch Flashspeicher gestützter Speicher** ([in den meisten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Bereitstellung des gesamten Flashspeichers für Datenträger mit Endurance oder Performance mit 2 IOPS/GB oder höheren Stufen.
- **Snapshots** ([in den meisten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Erfasst zeitpunktgesteuerte Datensnapshots ohne Betriebsunterbrechung
- **Replikation** ([in den meisten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC))
   - Kopiert Snapshots automatisch an ein Partner-{{site.data.keyword.cloud}}-Rechenzentrum.
- **Hoch verfügbare Konnektivität**
   - Verwendet redundante Netzbetriebsverbindungen zum Maximieren der Verfügbarkeit
   - iSCSI-basierter {{site.data.keyword.blockstorageshort}} verwendet Multipath I/O (MPIO).
- **Gleichzeitiger Zugriff**
   - Ermöglicht in Clusterkonfigurationen mehreren Hosts den gleichzeitigen Zugriff auf Blockdatenträger (bis zu 8).
- **Clusterdatenbanken**
   - Unterstützt erweiterte Anwendungsfälle wie Clusterdatenbanken.


## Bereitstellung
{: #provisioning}

{{site.data.keyword.blockstorageshort}}-LUNs können mit zwei Optionen von 20 GB bis 12 TB bereitgestellt werden: <br/>
- Stellen Sie **Endurance**-Tiers mit vordefinierten Leistungsstufen und weiteren Funktionen wie Snapshots und Replikation bereit.
- Erstellen Sie eine leistungsfähige **Performance**-Umgebung mit zugeordneten E/A-Operationen pro Sekunde (IOPS).


### Mit Endurance-Tiers bereitstellen
{: #provendurance}

Endurance-{{site.data.keyword.blockstorageshort}} ist in vier IOPS-Performance-Tiers verfügbar und unterstützt wechselnde Anwendungsbedürfnisse. <br />

- **0,25 IOPS pro GB** ist für Workloads mit niedriger Ein-/Ausgabeintensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein großer Prozentsatz der Daten inaktiv. Beispielanwendungen sind Speichermailboxen oder gemeinsam genutzte Dateiressourcen auf Abteilungsebene.

- **2 IOPS pro GB**  ist für die meisten allgemeinen Verwendungszwecke gedacht. Beispielanwendungen sind Hostings kleiner Datenbanken, Sicherungen von Webanwendungen oder Plattenimages virtueller Maschinen für einen Hypervisor.

- **4 IOPS pro GB** ist für Workloads mit höherer Intensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein hoher Prozentsatz der Daten aktiv. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.

- **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse. Dieses Tier ist für Speicher bis zu 4 TB verfügbar, der [in den meisten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) bereitgestellt wird.

Bis zu 48.000 IOPS sind bei einem 12-TB-Endurance-Datenträger verfügbar.

Die Auswahl des richtigen Endurance-Tiers ist für die Workload von entscheidender Bedeutung. Zur Erreichung der maximalen Leistung ist es gleichermaßen wichtig, die passende Blockgröße, Ethernet-Verbindungsgeschwindigkeit und Hostanzahl zu verwenden. Wenn diese Parameter nicht zueinander passen, kann das erhebliche Auswirkungen auf den Durchsatz haben.


### Mit Performance bereitstellen
{: #provperformance}

Performance ist eine Klasse von {{site.data.keyword.blockstorageshort}}, deren Aufgabe darin besteht, ein- und ausgabenintensive Anwendungen mit definierten Leistungsanforderungen zu unterstützen, die nicht in ein Endurance-Tier passen. Durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern wird eine vorhersehbare Leistung erreicht. Unterschiedliche IOPS-Raten (von 100 bis 48.000) können bei Speichergrößen von 20 GB bis 12 TB bereitgestellt werden.

Performance für {{site.data.keyword.blockstorageshort}} wird über eine MPIO-iSCSI-Verbindung (MPIO = Multipath I/O; iSCSI = Internet Small Computer System Interface) verfügbar gemacht und angehängt. {{site.data.keyword.blockstorageshort}} wird in der Regel verwendet, wenn auf den Datenträger von einem einzelnen Server zugegriffen wird. Mehrere Datenträger können an einen Host angehängt und per Stripekonfiguration verbunden werden, um größere Datenträger und höhere IOPS-Werte zu erreichen. Performance-Datenträger können entsprechend den Größen und IOPS-Raten in Tabelle 3 für die Betriebssysteme Linux, XEN und Windows bestellt werden.

| Größe (GB) | Minimum IOPS | Maximum IOPS
|-----|-----|-----|
| 20 | 100 | 1.000 |
| 40 | 100 | 2.000  |
| 80 | 100 | 4.000 |
| 100 | 100 | 6.000 |
| 250 | 100 | 6.000 |
| 500 | 100  | 6.000 oder 10.000 |
| 1.000 | 100 | 6.000 oder 20.000 ![Fußnote](/images/numberone.png) |
| 2.000 | 200 | 6.000 oder 40.000 ![Fußnote](/images/numberone.png) |
| 3.000 - 7.000 | 300 | 6.000 oder 48.000 ![Fußnote](/images/numberone.png) |
| 8.000-9.000 | 500 | 6.000 oder 48.000 ![Fußnote](/images/numberone.png) |
| 10.000-12.000 | 1.000 | 6.000 oder 48.000 ![Fußnote](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabellenvergleich" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![Fußnote](/images/numberone.png) * IOPS-Grenzwerte über 6.000 sind in den meisten Rechenzentren verfügbar.*

Performance-Datenträger sind für eine durchgängig Leistung bei Verwendung der bereitgestellten IOPS-Stufe vorgesehen. Die Konsistenz macht es einfacher, Anwendungsumgebungen bei einer bestimmten Leistungsstufe zu skalieren. Darüber hinaus ist es möglich, einen Datenträger mit einem idealen Preis-Leistungs-Verhältnis zu erstellen und so eine Umgebung zu optimieren.

## Abrechnung
{: #billing}

Sie können bei einer Block-LUN stündliche oder monatliche Abrechnung auswählen. Der für eine LUN ausgewählte Abrechnungstyp gilt für ihren Snapshotbereich und zugehörige Replikate. Wenn Sie beispielsweise eine LUN mit stündlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren stündlich abgerechnet. Wenn Sie eine LUN mit monatlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren monatlich abgerechnet.

 * Bei **stündlicher Abrechnung** wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus, je nachdem, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist in den [meisten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) verfügbar.

 * Bei **monatlicher Abrechnung** wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und verfügbar sein müssen (einen Monat oder länger).

### Endurance
{: #pricing-comparison-endurance}

| Preisoptionen für vordefinierte IOPS-Tiers | 0,25 IOPS | 2 IOPS/GB | 4 IOPS/GB | 10 IOPS/GB |
|-----|-----|-----|-----|-----|
| Monatlicher Preis | $0,06/GB | $0,15/GB | $0,20/GB | $0,58/GB |
| Stündlicher Preis | $0,0001/GB | $0,0002/GB | $0,0003/GB | $0.0009/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabellenvergleich" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Performance
{: #pricing-comparison-performance}

| Preisoptionen für angepasste IOPS | Preisberechnung |
|-----|-----|
| Monatlicher Preis | $0,10/GB + $0,07/IOP |
| Stündlicher Preis | $0,0001/GB + $0,0002/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabellenvergleich" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
