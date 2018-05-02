---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Informationen zu {{site.data.keyword.blockstorageshort}}

Bei {{site.data.keyword.blockstoragefull}} handelt es sich um persistenten iSCSI-Speicher mit hoher Leistung, der unabhängig von Recheninstanzen bereitgestellt und verwaltet wird. iSCSI-basierte {{site.data.keyword.blockstorageshort}}-LUNs sind über MPIO-Verbindungen (MPIO = Multipath I/O) mit autorisierten Geräten verbunden.

{{site.data.keyword.blockstorageshort}} bietet dank seiner unvergleichlichen Funktionen leistungsfähige Dauerhaftigkeit und Verfügbarkeit, wurde mithilfe von Branchenstandards und bewährten Verfahren aufgebaut, wurde dafür konzipiert, bei Wartungsereignissen und ungeplanten Ausfällen die Integrität der Daten zu schützen und die Verfügbarkeit aufrechtzuerhalten und gleichzeitig eine konsistente Leistungsbasis bereitzustellen.

## Zentrale Funktionen

Nutzen Sie die folgenden Funktionen von {{site.data.keyword.blockstorageshort}}:

- **Konsistente Leistungsbasis**
   - Bereitgestellt durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern
- **Äußerst dauerhaft und ausfallsicher**
   - Schützt die Integrität der Daten und erhält die Verfügbarkeit bei Wartungsereignissen und ungeplanten Ausfällen, ohne ein Redundant Array of Independent Disks auf Betriebssystemebene erstellen und verwalten zu müssen
- **Verschlüsselung ruhender Daten** ([in ausgewählten Rechenzentren verfügbar](new-ibm-block-and-file-storage-location-and-features.html).)
   - Anbietergesteuerte Verschlüsselung ruhender Daten ohne Zusatzkosten
- **Gesamter durch Flashspeicher gestützter Speicher** ([in ausgewählten Rechenzentren verfügbar](new-ibm-block-and-file-storage-location-and-features.html).)
   - Bereitstellung des gesamten Flashspeichers für Datenträger mit Endurance oder Performance mit 2 IOPS/GB oder höher
- **Snapshots** (bei Bereitstellung mit Endurance oder Performance in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html).
   - Erfasst zeitpunktgesteuerte Datensnapshots ohne Betriebsunterbrechung
- **Replikation** (bei Bereitstellung mit Endurance oder Performance in [ausgewählten Rechenzentren](/new-ibm-block-and-file-storage-location-and-features.html).
   - Kopiert Snapshots automatisch an ein Partner-{{site.data.keyword.BluSoftlayer_full}}-Rechenzentrum
- **Hoch verfügbare Konnektivität**
   - Verwendet redundante Netzbetriebsverbindungen zum Maximieren der Verfügbarkeit - iSCSI-basierter {{site.data.keyword.blockstorageshort}} verwendet Multipath I/O (MPIO)
- **Gleichzeitiger Zugriff**
   - Ermöglicht in Clusterkonfigurationen mehreren Hosts den gleichzeitigen Zugriff auf Blockdatenträger (bis zu 8).
- **Clusterdatenbanken**
   - Unterstützt erweiterte Anwendungsfälle wie Clusterdatenbanken
     
## Stündliche/monatliche Abrechnung

Sie können bei einer Block-LUN stündliche oder monatliche Abrechnung auswählen. Der für eine LUN ausgewählte Abrechnungstyp gilt für deren Snapshotbereich und Replikate. Wenn Sie beispielsweise eine LUN mit stündlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren stündlich abgerechnet. Wenn Sie eine LUN mit monatlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren monatlich abgerechnet. 

Bei **stündlicher Abrechnung** erfolgt die Abrechnung der Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus, je nachdem, was zuerst eintritt.  Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird. 

Bei **monatlicher Abrechnung** wird die Abrechnung anteilmäßig vom Datum der Erstellung bis zum Ende des Abrechnungszyklus berechnet und sofort abgerechnet. Wenn eine LUN vor dem Ende des Abrechnungszyklus gelöscht wird, gibt es keine Rückerstattung.  Die monatliche Abrechnung ist eine gute Wahl für Speicher, der in Produktionsworkloads verwendet wird, die Daten verwenden, die über längere Zeiträume gespeichert werden und im Zugriff sein müssen (einen Monat oder länger). 

### Performance:
<table>
 <tbody>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Endurance:
<table>
 <tbody>
  <tr>
   <th>IOPS-Tier</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Monatlicher Preis</th>
   <td>$0,10/GB</td>
   <td>$0,20/GB</td>
   <td>$0,35/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Stündlicher Preis</th>
   <td>0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0005/GB</td>
   <td>$0.0009/GB</td>
  </tr>
  </tbody>
</table>



## Bereitstellung

{{site.data.keyword.blockstorageshort}}-LUNs können von 20 GB bis 12 TB bereitgestellt werden, mit zwei Optionen für die Bereitstellung: <br/>
- Stellen Sie **Endurance**-Tiers mit vordefinierten Leistungsstufen und Funktionen wie Snapshots und Replikation bereit.
- Erstellen Sie eine leistungsfähige **Performance**-Umgebung mit zugeordneten E/A-Operationen pro Sekunde (IOPS). 

### Endurance-Tiers

Endurance ist in drei IOPS-Performance-Tiers verfügbar und unterstützt wechselnde Anwendungsbedürfnisse. <br />

- **0,25 IOPS pro GB** ist für Workloads mit niedriger Ein-/Ausgabeintensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein großer Prozentsatz der Daten inaktiv. Beispielanwendungen sind Speichermailboxen oder Dateifreigaben auf Abteilungsebene.
- **2 IOPS pro GB** ist für die meisten allgemeinen Verwendungszwecke gedacht. Beispielanwendungen sind Hostings kleiner Datenbanken, Sicherungen von Webanwendungen oder Plattenimages virtueller Maschinen für einen Hypervisor.
- **4 IOPS pro GB** ist für Workloads mit höherer Intensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein hoher Prozentsatz der Daten aktiv. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.
- **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse.  Dieses Tier ist für Speicher bis zu 4 TB verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird.

Bis zu 48.000 IOPS sind bei einem 12-TB-Endurance-Datenträger verfügbar.
 

Zwar ist die Wahl des richtigen Tiers von Endurance {{site.data.keyword.blockstorageshort}} für Ihre Workload von zentraler Bedeutung, doch zur Erreichung der maximalen Leistung ist es nicht weniger wichtig, die passende Blockgröße, Ethernet-Verbindungsgeschwindigkeit und Hostanzahl zu verwenden. Wenn diese Parameter nicht zueinander passen, kann das erhebliche Auswirkungen auf den Durchsatz haben.

 
### Performance

Performance ist eine Klasse von {{site.data.keyword.blockstorageshort}}, deren Aufgabe darin besteht, ein- und ausgabeintensive Anwendungen mit klar definierten Leistungsanforderungen zu unterstützen, die nicht in ein Endurance-Tier passen. Durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern wird eine vorhersehbare Leistung erreicht. IOPS von 100 bis 48.000 können bei Speichergrößen von 20 GB bis 12 TB bereitgestellt werden. 

Performance für {{site.data.keyword.blockstorageshort}} wird über eine MPIO-iSCSI-Verbindung (MPIO = Multipath I/O; iSCSI = Internet Small Computer System Interface) verfügbar gemacht und angehängt. {{site.data.keyword.blockstorageshort}} wird in der Regel verwendet, wenn der Zugriff auf den Datenträger über eine einzelne Maschine erfolgt. Mehrere Datenträger können an einen Host angehängt und per Stripekonfiguration verbunden werden, um größere Datenträger und höhere IOPS-Werte zu erreichen. Performance-Datenträger können entsprechend den Größen und IOPS in Tabelle 1 für die Betriebssysteme Linux, XEN, VMware und Windows bestellt werden.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
            <td>6.000 oder 10.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 oder 20.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 oder 40.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![footnote](/images/numberone.png)</sup> Ein IOPS-Grenzwert über 6.000 ist in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) verfügbar.


Performance-Datenträger sind dazu gedacht, durchgängig eine Leistung nahe der bereitgestellten IOPS-Stufe zu bieten. Die Konsistenz macht es einfacher, Anwendungsumgebungen bei einer gegebenen Leistungsstufe zu skalieren. Darüber hinaus wird es angesichts des Bereichs der Datenträgergrößen und IOPS-Werte möglich, einen Datenträger mit einem idealen Preis-Leistungs-Verhältnis zu erstellen und so eine Umgebung zu optimieren.

### Tipps zur Bereitstellung von IOPS für {{site.data.keyword.blockstorageshort}}

IOPS für Endurance und Performance basiert auf einer Blockgröße von 16 KB mit einer 50/50-Lese-/Schreibworkload mit 50% Zufälligkeit. Ein 16-KB-Block entspricht einem Schreibvorgang auf dem Datenträger.

Die von Ihrer Anwendung verwendete Blockgröße hat einen direkten Einfluss auf die Speicherleistung.  Wenn die von Ihrer Anwendung verwendete Blockgröße kleiner als 16 KB ist, wird der IOPS-Grenzwert vor dem Durchsatzgrenzwert umgesetzt.  Wenn dagegen die von Ihrer Anwendung verwendete Blockgröße größer als 16 KB ist, wird der Durchsatzgrenzwert vor dem IOPS-Grenzwert umgesetzt.

Die Änderung der Blockgröße wirkt sich wie folgt auf die Leistung aus:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Blockgröße (KB)</th>
            <th>IOPS</th>
            <th>Durchsatz (MB/s)</th>
          </tr>
          <tr>
            <td>4 (typisch für Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (typisch für Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (typisch für SQLServer)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

Den für Ihre Workload passenden {{site.data.keyword.blockstorageshort}} auszuwählen, ist wichtig. Nicht weniger wichtig ist es, Engpässe zu vermeiden. Die Geschwindigkeit Ihrer Ethernet-Verbindung muss höher sein als der erwartete maximale Durchsatz auf Ihrem Datenträger. Generell sollten Sie nicht erwarten, Ihre Ethernet-Verbindung über 70% der verfügbaren Bandbreite hinaus auszulasten. Wenn Sie beispielsweise 6.000 IOPS haben und eine Blockgröße von 16 KB verwenden, sind auf dem Datenträger etwa 94 MB pro Sekunde möglich. Bei einer Ethernet-Verbindung von 1 Gb/s zu Ihrer LUN wird diese Verbindung zu einem Engpass, wenn Ihre Server versuchen, den maximal verfügbaren Durchsatz zu nutzen, weil 70% des theoretischen Grenzwerts von einer Ethernet-Verbindung mit 1 Gb/s (125 MB pro Sekunde) nur 88 MB pro Sekunde zulassen würden.


Ein anderer zu berücksichtigender Faktor ist die Anzahl der Hosts, die Ihren Datenträger nutzen. Wenn ein einzelner Host auf den Datenträger zugreift, könnte es schwer sein, den maximal verfügbaren IOPS-Wert umzusetzen, vor allem bei extremen IOPS-Werten (10.000s). Wenn Ihre Workload einen hohen Durchsatz erfordert, ist es am besten, mindestens zwei oder drei Server für den Zugriff auf Ihren Datenträger zu konfigurieren, um einen Engpass durch einen einzelnen Server zu vermeiden.


Um die maximalen IOPS-Werte zu erreichen, müssen geeignete Netzressourcen vorhanden sein. Außerdem sind die Nutzung privater Netze außerhalb des Speichers sowie hostseitige und anwendungsspezifische Optimierungen wie IP-Stack, Warteschlangenlängen etc. zu berücksichtigen.
