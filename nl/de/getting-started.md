---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# Lernprogramm zur Einführung
{: #getting-started}

Bei {{site.data.keyword.blockstoragefull}} handelt es sich um persistenten iSCSI-Speicher mit hoher Leistung, der unabhängig von Recheninstanzen bereitgestellt und verwaltet wird. iSCSI-basierte {{site.data.keyword.blockstorageshort}}-LUNs sind über MPIO-Verbindungen (MPIO - Multipath I/O) mit autorisierten Geräten verbunden.

{{site.data.keyword.blockstorageshort}} bietet dank seiner unvergleichlichen Funktionen leistungsfähige Dauerhaftigkeit und Verfügbarkeit. Basis sind Branchenstandards und bewährte Verfahren. {{site.data.keyword.blockstorageshort}} wurde dafür konzipiert, bei Wartungsereignissen und ungeplanten Ausfällen die Integrität der Daten zu schützen und die Verfügbarkeit aufrechtzuerhalten und gleichzeitig eine konsistente Leistungsbasis bereitzustellen.
{:shortdesc}

## Vorbereitungen
{: #prereqs}

{{site.data.keyword.blockstorageshort}}-LUNs können mit zwei Optionen von 20 GB bis 12 TB bereitgestellt werden: <br/>
- Stellen Sie **Endurance**-Tiers mit vordefinierten Leistungsstufen und weiteren Funktionen wie Snapshots und Replikation bereit.
- Erstellen Sie eine leistungsfähige **Performance**-Umgebung mit zugeordneten E/A-Operationen pro Sekunde (IOPS).

Weitere Informationen zum {{site.data.keyword.blockstorageshort}}-Angebot finden Sie in [Informationen zu {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

## Hinweise zur Bereitstellung

### Blockgröße

IOPS für Endurance und Performance basiert auf einer Blockgröße von 16 KB mit einer 50/50-Lese-/Schreibworkload mit 50/50 zufälliger/sequenqzieller Verarbeitung. Ein 16-KB-Block entspricht einem Schreibvorgang auf dem Datenträger.
{:important}

Die von der Anwendung verwendete Blockgröße beeinflusst direkt die Speicherleistung. Wenn die von Ihrer Anwendung verwendete Blockgröße kleiner als 16 KB ist, wird der IOPS-Grenzwert vor dem Durchsatzgrenzwert umgesetzt. Wenn dagegen die von Ihrer Anwendung verwendete Blockgröße größer als 16 KB ist, wird der Durchsatzgrenzwert vor dem IOPS-Grenzwert umgesetzt.

| Blockgröße (KB) | IOPS | Durchsatz (MB/s) |
|-----|-----|-----|
| 4 | 1.000 | 16 |
| 8 | 1.000 | 16 |
| 16 | 1.000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="In Tabelle 1 werden Beispiele für die Beeinflussung des Durchsatzes durch Blockgröße und IOPS-Rate aufgeführt.<br/Durchschnittliche IO-Größe x IOPS = Durchsatz in MB/s." caption-side="top"}>

### Autorisierte Hosts

Ein anderer zu berücksichtigender Faktor ist die Anzahl der Hosts, die den Datenträger nutzen. Wenn ein einzelner Host auf den Datenträger zugreift, kann es schwierig sein, den maximal verfügbaren IOPS-Wert umzusetzen, vor allem bei extremen IOPS-Werten (10.000 s). Wenn die Workload einen hohen Durchsatz erfordert, ist es am besten, eine Mindestmenge an Servern für den Zugriff auf den Datenträger zu konfigurieren, um einen Engpass durch einen einzelnen Server zu vermeiden.

### Netzverbindung

Die Geschwindigkeit Ihrer Ethernet-Verbindung muss höher sein als der erwartete maximale Durchsatz auf Ihrem Datenträger. Generell sollten Sie nicht erwarten, Ihre Ethernet-Verbindung über 70 % der verfügbaren Bandbreite hinaus auszulasten. Wenn Sie beispielsweise über 6.000 IOPS verfügen und eine Blockgröße von 16 KB verwenden, sind auf dem Datenträger etwa 94 MBps möglich. Bei einer Ethernet-Verbindung von 1 Gb/s zu einer LUN wird diese Verbindung zu einem Engpass, wenn die Server versuchen, den maximal verfügbaren Durchsatz zu nutzen. Ursache hierfür ist, dass 70 Prozent des theoretischen Grenzwerts von einer Ethernet-Verbindung mit 1 Gb/s (125 MB pro Sekunde) nur 88 MB pro Sekunde zulassen würden.

Um die maximalen IOPS-Werte zu erreichen, müssen geeignete Netzressourcen vorhanden sein. Außerdem sind die Nutzung privater Netze außerhalb des Speichers sowie hostseitige und anwendungsspezifische Optimierungen (zum Beispiel IP-Stack oder [Warteschlangenlängen](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings) und andere Einstellungen) zu berücksichtigen.

Der Speicherverkehr muss von anderen Datenverkehrstypen isoliert werden und darf nicht über Firewalls und Router übertragen werden. Weitere Informationen finden Sie bei den [häufig gestellten Fragen](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic).

Der Speicherdatenverkehr ist in der gesamten Netznutzung von öffentlichen virtuellen Servern enthalten. Weitere Informationen zu den Grenzwerten, die für die Verwendung des Service gelten können, finden Sie in der [Dokumentation zu virtuellen Servern](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers).
{:tip}

## Bestellung aufgeben
{: #submitorder}

Wenn Sie bereit sind, die Bestellung aufzugeben, können Sie dies über die [Konsole](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole), die [SL-CLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI) oder die [IBMCLOUD CLI](/docs/cli/reference/ibmcloud?topic=cloud-cli-sl-block-storage#sl_block_volume_order) tun.

Informationen zur {{site.data.keyword.blockstorageshort}}-Bestellung über die API finden Sie unter [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Um auf alle neuen Funktionen zugreifen zu können, müssen Sie `Storage-as-a-Service Package 759` bestellen.
{:tip}

## Neuen Speicher verbinden
{: #mountingstorage}

Wenn die Bereitstellungsanforderung abgeschlossen ist, autorisieren Sie die Hosts, um auf den neuen Speicher zuzugreifen und die Verbindung zu konfigurieren. Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verbindung zu LUNs unter CloudLinux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Verbindung zu LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Neuen Speicher verwalten

Über das Portal oder die SL-CLI können Sie verschiedene Aspekte des {{site.data.keyword.blockstorageshort}} verwalten, z. B. Hostberechtigungen und Stornierungen. Weitere Informationen finden Sie in [{{site.data.keyword.blockstorageshort}}verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
