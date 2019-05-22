---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

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
- **Verschlüsselung ruhender Daten** ([in ausgewählten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Anbietergesteuerte Verschlüsselung ruhender Daten ohne Zusatzkosten.
- **Gesamter durch Flashspeicher gestützter Speicher** ([in ausgewählten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Bereitstellung des gesamten Flashspeichers für Datenträger mit Endurance oder Performance mit 2 IOPS/GB oder höheren Stufen.
- **Snapshots** ([in ausgewählten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Erfasst zeitpunktgesteuerte Datensnapshots ohne Betriebsunterbrechung
- **Replikation** ([in ausgewählten Rechenzentren verfügbar](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Kopiert Snapshots automatisch an ein Partner-{{site.data.keyword.BluSoftlayer_full}}-Rechenzentrum.
- **Hoch verfügbare Konnektivität**
   - Verwendet redundante Netzbetriebsverbindungen zum Maximieren der Verfügbarkeit
   - iSCSI-basierter {{site.data.keyword.blockstorageshort}} verwendet Multipath I/O (MPIO).
- **Gleichzeitiger Zugriff**
   - Ermöglicht in Clusterkonfigurationen mehreren Hosts den gleichzeitigen Zugriff auf Blockdatenträger (bis zu 8).
- **Clusterdatenbanken**
   - Unterstützt erweiterte Anwendungsfälle wie Clusterdatenbanken.

## Abrechnung
{: #billing}

Sie können bei einer Block-LUN stündliche oder monatliche Abrechnung auswählen. Der für eine LUN ausgewählte Abrechnungstyp gilt für ihren Snapshotbereich und zugehörige Replikate. Wenn Sie beispielsweise eine LUN mit stündlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren stündlich abgerechnet. Wenn Sie eine LUN mit monatlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren monatlich abgerechnet.

Bei **stündlicher Abrechnung** wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus, je nachdem, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations) bereitgestellt wird.

Bei **monatlicher Abrechnung** wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung. Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und verfügbar sein müssen (einen Monat oder länger).

**Performance**
<table>
  <caption>In Tabelle 1 werden die Preise für Performance-Speicher mit monatlicher und stündlicher Abrechnung aufgeführt.</caption>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>In Tabelle 2 werden die Preise für Endurance-Speicher für jedes Tier mit Optionen für monatliche und stündliche Abrechnung aufgeführt.</caption>
  <tr>
   <th>IOPS-Tier</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,06/GB</td>
   <td>$0,15/GB</td>
   <td>$0,20/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>$0,0001/GB</td>
   <td>$0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



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

- **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse. Dieses Tier ist für Speicher bis zu 4 TB verfügbar, der nur in [ausgewählten Rechenzentren](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations) bereitgestellt wird.

Bis zu 48.000 IOPS sind bei einem 12-TB-Endurance-Datenträger verfügbar.

Die Auswahl des richtigen Endurance-Tiers ist für die Workload von entscheidender Bedeutung. Zur Erreichung der maximalen Leistung ist es gleichermaßen wichtig, die passende Blockgröße, Ethernet-Verbindungsgeschwindigkeit und Hostanzahl zu verwenden. Wenn diese Parameter nicht zueinander passen, kann das erhebliche Auswirkungen auf den Durchsatz haben.


### Mit Performance bereitstellen
{: #provperformance}

Performance ist eine Klasse von {{site.data.keyword.blockstorageshort}}, deren Aufgabe darin besteht, ein- und ausgabenintensive Anwendungen mit definierten Leistungsanforderungen zu unterstützen, die nicht in ein Endurance-Tier passen. Durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern wird eine vorhersehbare Leistung erreicht. Unterschiedliche IOPS-Raten (von 100 bis 48.000) können bei Speichergrößen von 20 GB bis 12 TB bereitgestellt werden.

Performance für {{site.data.keyword.blockstorageshort}} wird über eine MPIO-iSCSI-Verbindung (MPIO = Multipath I/O; iSCSI = Internet Small Computer System Interface) verfügbar gemacht und angehängt. {{site.data.keyword.blockstorageshort}} wird in der Regel verwendet, wenn auf den Datenträger von einem einzelnen Server zugegriffen wird. Mehrere Datenträger können an einen Host angehängt und per Stripekonfiguration verbunden werden, um größere Datenträger und höhere IOPS-Werte zu erreichen. Performance-Datenträger können entsprechend den Größen und IOPS-Raten in Tabelle 3 für die Betriebssysteme Linux, XEN und Windows bestellt werden.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>In Tabelle 3 werden Kombinationen aus Größe und IOPS-Raten für Performance-Speicher aufgeführt.<br/><sup><img src="/images/numberone.png" alt="Fußnote" /></sup> IOPS-Grenzwerte über 6.000 sind in ausgewählten Rechenzentren verfügbar.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Größe (GB)</th>
            <th>Minimum IOPS</th>
            <th>Maximum IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 oder 10.000 <sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 oder 20.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000</td>
            <td>200</td>
            <td>6.000 oder 40.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>3.000 - 7.000</td>
            <td>300</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Fußnote" /></sup></td>
          </tr>
</table>


Performance-Datenträger sind für eine durchgängig Leistung bei Verwendung der bereitgestellten IOPS-Stufe vorgesehen. Die Konsistenz macht es einfacher, Anwendungsumgebungen bei einer bestimmten Leistungsstufe zu skalieren. Darüber hinaus ist es möglich, einen Datenträger mit einem idealen Preis-Leistungs-Verhältnis zu erstellen und so eine Umgebung zu optimieren.
