---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-10"

---
{:new_window: target="_blank"}

# Einführung in {{site.data.keyword.blockstorageshort}}

Bei {{site.data.keyword.blockstoragefull}} handelt es sich um persistenten iSCSI-Speicher mit hoher Leistung, der unabhängig von Recheninstanzen bereitgestellt und verwaltet wird. iSCSI-basierte {{site.data.keyword.blockstorageshort}}-LUNs sind über MPIO-Verbindungen (MPIO - Multipath I/O) mit autorisierten Geräten verbunden.

{{site.data.keyword.blockstorageshort}} bietet dank seiner unvergleichlichen Funktionen leistungsfähige Dauerhaftigkeit und Verfügbarkeit. Basis sind Branchenstandards und bewährte Verfahren. {{site.data.keyword.blockstorageshort}} wurde dafür konzipiert, bei Wartungsereignissen und ungeplanten Ausfällen die Integrität der Daten zu schützen und die Verfügbarkeit aufrechtzuerhalten und gleichzeitig eine konsistente Leistungsbasis bereitzustellen.

## Zentrale Funktionen

Nutzen Sie die folgenden Funktionen von {{site.data.keyword.blockstorageshort}}:

- **Konsistente Leistungsbasis**
   - Wird durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern erreicht.
- **Äußerst dauerhaft und ausfallsicher**
   - Schützt bei Wartungsereignissen und ungeplanten Ausfällen die Integrität der Daten, hält die Verfügbarkeit aufrecht und stellt gleichzeitig eine konsistente Leistungsbasis bereit.
- **Verschlüsselung ruhender Daten** ([in ausgewählten Rechenzentren verfügbar](new-ibm-block-and-file-storage-location-and-features.html))
   - Anbietergesteuerte Verschlüsselung ruhender Daten ohne Zusatzkosten.
- **Gesamter durch Flashspeicher gestützter Speicher** ([in ausgewählten Rechenzentren verfügbar](new-ibm-block-and-file-storage-location-and-features.html))
   - Bereitstellung des gesamten Flashspeichers für Datenträger mit Endurance oder Performance mit 2 IOPS/GB oder höheren Stufen.
- **Snapshots** ([in ausgewählten Rechenzentren verfügbar](new-ibm-block-and-file-storage-location-and-features.html))
   - Erfasst zeitpunktgesteuerte Datensnapshots ohne Betriebsunterbrechung
- **Replikation** ([in ausgewählten Rechenzentren verfügbar](new-ibm-block-and-file-storage-location-and-features.html))
   - Kopiert Snapshots automatisch an ein Partner-{{site.data.keyword.BluSoftlayer_full}}-Rechenzentrum.
- **Hoch verfügbare Konnektivität**
   - Verwendet redundante Netzbetriebsverbindungen zum Maximieren der Verfügbarkeit 
   - iSCSI-basierter {{site.data.keyword.blockstorageshort}} verwendet Multipath I/O (MPIO).
- **Gleichzeitiger Zugriff**
   - Ermöglicht in Clusterkonfigurationen mehreren Hosts den gleichzeitigen Zugriff auf Blockdatenträger (bis zu 8).
- **Clusterdatenbanken**
   - Unterstützt erweiterte Anwendungsfälle wie Clusterdatenbanken.
     
## Abrechnung

Sie können bei einer Block-LUN stündliche oder monatliche Abrechnung auswählen. Der für eine LUN ausgewählte Abrechnungstyp gilt für ihren Snapshotbereich und zugehörige Replikate. Wenn Sie beispielsweise eine LUN mit stündlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren stündlich abgerechnet. Wenn Sie eine LUN mit monatlicher Abrechnung bereitstellen, werden alle Snapshot- oder Replikatgebühren monatlich abgerechnet. 

Bei **stündlicher Abrechnung** wird die Anzahl der Stunden, die die Block-LUN auf dem Konto vorhanden war, zu dem Zeitpunkt berechnet, an dem die LUN gelöscht wird, oder am Ende des Abrechnungszyklus, je nachdem, was zuerst eintritt. Die stündliche Abrechnung ist eine gute Wahl für Speicher, der nur für einige Tage oder weniger als einen Monat verwendet wird. Die stündliche Abrechnung ist nur für Speicher verfügbar, der in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird. 

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
</table>



## Bereitstellung

{{site.data.keyword.blockstorageshort}}-LUNs können mit zwei Optionen von 20 GB bis 12 TB bereitgestellt werden: <br/>
- Stellen Sie **Endurance**-Tiers mit vordefinierten Leistungsstufen und weiteren Funktionen wie Snapshots und Replikation bereit.
- Erstellen Sie eine leistungsfähige **Performance**-Umgebung mit zugeordneten E/A-Operationen pro Sekunde (IOPS). 

### Mit Endurance-Tiers bereitstellen

Endurance-{{site.data.keyword.blockstorageshort}} ist in vier IOPS-Performance-Tiers verfügbar und unterstützt wechselnde Anwendungsbedürfnisse. <br />

- **0,25 IOPS pro GB** ist für Workloads mit niedriger Ein-/Ausgabeintensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein großer Prozentsatz der Daten inaktiv. Beispielanwendungen sind Speichermailboxen oder Dateifreigaben auf Abteilungsebene.

- **2 IOPS pro GB**  ist für die meisten allgemeinen Verwendungszwecke gedacht. Beispielanwendungen sind Hostings kleiner Datenbanken, Sicherungen von Webanwendungen oder Plattenimages virtueller Maschinen für einen Hypervisor.

- **4 IOPS pro GB** ist für Workloads mit höherer Intensität gedacht. Bei diesen Workloads ist in der Regel zu jedem Zeitpunkt ein hoher Prozentsatz der Daten aktiv. Beispielanwendungen sind transaktionsorientierte und andere leistungskritische Datenbanken.

- **10 IOPS pro GB** ist für die anspruchsvollsten Workloads gedacht, beispielsweise für solche, die durch NoSQL-Datenbanken erstellt werden, sowie für die Analyse. Dieses Tier ist für Speicher bis zu 4 TB verfügbar, der nur in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt wird.

Bis zu 48.000 IOPS sind bei einem 12-TB-Endurance-Datenträger verfügbar.
 
Die Auswahl des richtigen Endurance-Tiers ist für die Workload von entscheidender Bedeutung. Zur Erreichung der maximalen Leistung ist es gleichermaßen wichtig, die passende Blockgröße, Ethernet-Verbindungsgeschwindigkeit und Hostanzahl zu verwenden. Wenn diese Parameter nicht zueinander passen, kann das erhebliche Auswirkungen auf den Durchsatz haben.

 
### Mit Performance bereitstellen

Performance ist eine Klasse von {{site.data.keyword.blockstorageshort}}, deren Aufgabe darin besteht, ein- und ausgabenintensive Anwendungen mit definierten Leistungsanforderungen zu unterstützen, die nicht in ein Endurance-Tier passen. Durch die Zuordnung von IOPS auf Protokollebene zu einzelnen Datenträgern wird eine vorhersehbare Leistung erreicht. Unterschiedliche IOPS-Raten (von 100 bis 48.000) können bei Speichergrößen von 20 GB bis 12 TB bereitgestellt werden. 

Performance für {{site.data.keyword.blockstorageshort}} wird über eine MPIO-iSCSI-Verbindung (MPIO = Multipath I/O; iSCSI = Internet Small Computer System Interface) verfügbar gemacht und angehängt. {{site.data.keyword.blockstorageshort}} wird in der Regel verwendet, wenn auf den Datenträger von einem einzelnen Server zugegriffen wird. Mehrere Datenträger können an einen Host angehängt und per Stripekonfiguration verbunden werden, um größere Datenträger und höhere IOPS-Werte zu erreichen. Performance-Datenträger können entsprechend den Größen und IOPS-Raten in Tabelle 3 für die Betriebssysteme Linux, XEN und Windows bestellt werden.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>In Tabelle 3 werden Kombinationen aus Größe und IOPS-Raten für Performance-Speicher aufgeführt.<br/><sup><img src="/images/numberone.png" alt="Footnote" /></sup> Ein IOPS-Grenzwert über 6.000 ist in ausgewählten Rechenzentren verfügbar.</caption>
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
            <td>6.000 oder 10.000<sup><img src="/images/numberone.png" alt="footnote" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 oder 20.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 oder 40.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 oder 48.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
</table>


Performance-Datenträger sind für eine durchgängig Leistung bei Verwendung der bereitgestellten IOPS-Stufe vorgesehen. Die Konsistenz macht es einfacher, Anwendungsumgebungen bei einer bestimmten Leistungsstufe zu skalieren. Darüber hinaus ist es möglich, einen Datenträger mit einem idealen Preis-Leistungs-Verhältnis zu erstellen und so eine Umgebung zu optimieren.

### Hinweise zur Bereitstellung

**Blockgröße**

IOPS für Endurance und Performance basiert auf einer Blockgröße von 16 KB mit einer 50/50-Lese-/Schreibworkload mit 50 Prozent Zufälligkeit. Ein 16-KB-Block entspricht einem Schreibvorgang auf dem Datenträger.

Die von der Anwendung verwendete Blockgröße beeinflusst direkt die Speicherleistung. Wenn die von Ihrer Anwendung verwendete Blockgröße kleiner als 16 KB ist, wird der IOPS-Grenzwert vor dem Durchsatzgrenzwert umgesetzt. Wenn dagegen die von Ihrer Anwendung verwendete Blockgröße größer als 16 KB ist, wird der Durchsatzgrenzwert vor dem IOPS-Grenzwert umgesetzt.

<table>
  <caption>In Tabelle 4 werden Beispiele für die Beeinflussung des Durchsatzes durch Blockgröße und IOPS-Rate aufgeführt.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Blockgröße (KB)</th>
            <th>IOPS</th>
            <th>Durchsatz (MB/s)</th>
          </tr>
        </thead>
        <tbody>
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
            <td>32 (typisch für SQL Server)</td>
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

**Autorisierte Hosts**

Ein anderer zu berücksichtigender Faktor ist die Anzahl der Hosts, die den Datenträger nutzen. Wenn ein einzelner Host auf den Datenträger zugreift, kann es schwierig sein, den maximal verfügbaren IOPS-Wert umzusetzen, vor allem bei extremen IOPS-Werten (10.000 s). Wenn die Workload einen hohen Durchsatz erfordert, ist es am besten, eine Mindestmenge an Servern für den Zugriff auf den Datenträger zu konfigurieren, um einen Engpass durch einen einzelnen Server zu vermeiden.

**Netzverbindung**

Die Geschwindigkeit Ihrer Ethernet-Verbindung muss höher sein als der erwartete maximale Durchsatz auf Ihrem Datenträger. Generell sollten Sie nicht erwarten, Ihre Ethernet-Verbindung über 70 % der verfügbaren Bandbreite hinaus auszulasten. Wenn Sie beispielsweise über 6.000 IOPS verfügen und eine Blockgröße von 16 KB verwenden, sind auf dem Datenträger etwa 94 MBps möglich. Bei einer Ethernet-Verbindung von 1 Gb/s zu einer LUN wird diese Verbindung zu einem Engpass, wenn die Server versuchen, den maximal verfügbaren Durchsatz zu nutzen. Ursache hierfür ist, dass 70 Prozent des theoretischen Grenzwerts von einer Ethernet-Verbindung mit 1 Gb/s (125 MB pro Sekunde) nur 88 MB pro Sekunde zulassen würden.

Um die maximalen IOPS-Werte zu erreichen, müssen geeignete Netzressourcen vorhanden sein. Außerdem sind die Nutzung privater Netze außerhalb des Speichers sowie hostseitige und anwendungsspezifische Optimierungen (zum Beispiel IP-Stack, Warteschlangenlängen und andere Einstellungen) zu berücksichtigen.

Der Speicherdatenverkehr ist in der gesamten Netznutzung von öffentlichen virtuellen Servern enthalten. Die [Dokumentation zu virtuellen Servern](https://console.bluemix.net/docs/vsi/vsi_public.html#public-virtual-servers) enthält Informationen zu den möglichen Einschränkungen im Zusammenhang mit dem Service.

## Auftrag erteilen

Wenn Sie bereit sind, den Auftrag zu erteilen, befolgen Sie die [hier](provisioning-block_storage.html) aufgeführten Anweisungen.

## Neuen Speicher verbinden

Wenn die Bereitstellungsanforderung abgeschlossen ist, autorisieren Sie die Hosts, um auf den neuen Speicher zuzugreifen und die Verbindung zu konfigurieren. Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html)
- [Verbindung zu MPIO-iSCSI-LUNS unter Microsoft Windows herstellen](accessing-block-storage-windows.html)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](configure-backup-cpanel.html)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](configure-backup-plesk.html)

