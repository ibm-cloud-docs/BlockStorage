---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# Häufig gestellte Fragen
{: #faqs}

## Wie viele Instanzen können einen bereitgestellten {{site.data.keyword.blockstorageshort}}-Datenträger gemeinsam nutzen?
{: faq}

Der Standardgrenzwert für die Anzahl der Berechtigungen pro Blockdatenträger ist 8. Dies bedeutet, dass bis zu 8 Hosts für den Zugriff auf die Blockspeicher-LUN berechtigt werden können. Wenn Sie eine Erhöhung des Grenzwerts anfordern möchten, wenden Sie sich an den zuständigen Vertriebsbeauftragten.

## Wie viele Datenträger können bestellt werden?
{: faq}

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenn Sie eine Erhöhung des Grenzwerts für Datenträger anfordern möchten, wenden Sie sich an den zuständigen Vertriebsbeauftragten. Weitere Informationen finden Sie in [Speichergrenzwerte verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).

## Wie viele {{site.data.keyword.blockstorageshort}}-Datenträger können an einen Host angehängt werden?
{: faq}

Dies ist abhängig von der Kapazität des Hostbetriebssystems, nicht von {{site.data.keyword.BluSoftlayer_full}}-Grenzwerten. Die Dokumentation des jeweiligen Betriebssystems enthält Informationen zu den Grenzwerten für die Anzahl der Datenträger, die angehängt werden können.

## Welche Windows-Version sollte ich für meine Blockspeicher-LUN auswählen?
{: faq}

Wenn Sie eine LUN erstellen, müssen Sie den Betriebssystemtyp angeben. Der Betriebssystemtyp muss auf dem Betriebssystem basieren, das die Hosts verwenden, die auf die LUN zugreifen. Der Betriebssystemtyp kann nicht geändert werden, nachdem die LUN erstellt wurde. Die tatsächliche Größe der LUN kann je nach Betriebssystemtyp der LUN geringfügig variieren.

**Windows 2008+**
- Die LUN speichert Windows-Daten für Windows 2008 und nachfolgende Versionen. Verwenden Sie diese Betriebssystemoption, wenn Ihr Hostbetriebssystem Windows Server 2008, Windows Server 2012 oder Windows Server 2016 ist. Es wird sowohl die Partitionierungsmethode MBR als auch die Methode GPT unterstützt.

**Windows 2003**
- Die LUN speichert einen unformatierte Plattentyp auf einer Windows-Platte mit nur einer Partition und verwendet dabei den MBR-Partitionierungsstil (Master Boot Record). Verwenden Sie diese Option nur dann, wenn Ihr Hostbetriebssystem Windows 2000 Server, Windows XP oder Windows Server 2003 ist und die Partitionierungsmethode MBR verwendet.

**Windows GPT**
-  Die LUN speichert Windows-Daten unter Verwendung des Partitionierungsstils GUID Partition Type (GPT). Verwenden Sie diese Option, wenn Sie die Partitionierungsmethode GPT verwenden wollen und Ihr Host diese Partitionierungsmethode verwenden kann. In Windows Server 2003 Service Pack 1 und höher kann die Partitionierungsmethode GPT verwendet werden. Alle 64-Bit-Versionen unterstützen diese Methode.

## Wird der zugeordnete IOPS-Grenzwert nach Instanz oder nach Datenträger umgesetzt?
{: faq}

IOPS werden auf Datenträgerebene umgesetzt. Anders ausgedrückt, zwei Hosts, die mit einem Datenträger mit 6000 IOPS verbunden sind, nutzen diese 6000 IOPS gemeinsam.

## IOPS messen
{: faq}

IOPS werden auf Basis eines Belastungsprofils von 16-KB-Blöcken mit zufälligen 50 Prozent Lese- und 50 Prozent Schreibvorgängen gemessen. Workloads, die von diesem Profil abweichen, weisen möglicherweise niedrigere Leistungswerte auf.

## Was passiert, wenn eine kleinere Blockgröße zur Messung der Leistung verwendet wird?
{: faq}

Auch bei kleineren Blockgrößen kann der maximale IOPS-Wert erreicht werden. Allerdings verringert sich der Durchsatz. Beispiel: Ein Datenträger mit 6000 IOPS hat bei verschiedenen Blockgrößen den folgenden Durchsatz:

- 16 KB * 6000 IOPS == ~93,75 MB/Sek.
- 8 KB * 6000 IOPS == ~46,88 MB/Sek.
- 4 KB * 6000 IOPS == ~23,44 MB/Sek.

## Muss der Datenträger aufgewärmt werden, um den erwarteten Durchsatz zu erzielen?
{: faq}

Ein Aufwärmung ist nicht erforderlich. Der angegebene Durchsatz wird sofort bei Bereitstellung des Datenträgers erreicht.

## Kann durch eine schnellere Ethernet-Verbindung ein höherer Durchsatz erreicht werden?
{: faq}

Die Grenzwerte für den Durchsatz werden auf LUN-Ebene festgelegt und können somit nicht durch eine schnellere Ethernet-Verbindung erhöht werden. Bei einer langsameren Ethernet-Verbindung jedoch kann Ihre Bandbreite ein potenzieller Engpass sein.

## Beeinträchtigen Firewalls und Sicherheitsgruppen die Leistung?
{: faq}

Es wird empfohlen, den Speicherdatenverkehr über ein VLAN auszuführen, das die Firewall umgeht. Eine Ausführung des Speicherdatenverkehrs über Software-Firewalls erhöht die Latenz und beeinträchtigt die Speicherleistung.

## Welche Latenzzeit kann von {{site.data.keyword.blockstorageshort}} erwartet werden?   
{: faq}

Die Ziellatenz im Speicher beträgt <1 ms. Da der vorliegende Speicher mit Recheninstanzen in einem gemeinsamen Netz verbunden ist, hängt die genaue Leistungslatenz vom Netzverkehr während des Betriebs ab.

## Warum kann {{site.data.keyword.blockstorageshort}} in manchen Rechenzentren mit einem Endurance-10/GB-IOPS-Tier bestellt werden und in anderen nicht?
{: faq}

Das {{site.data.keyword.blockstorageshort}}-Endurance-IOPS/GB-Tier des Speichertyps 10 ist nur in ausgewählten Rechenzentren verfügbar, weitere Rechenzentren folgende nach und nach. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Funktionen finden Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

## Wie kann man erkennen, welche {{site.data.keyword.blockstorageshort}}-Datenträger verschlüsselt sind?
{: faq}

Wenn Sie sich die Liste der {{site.data.keyword.blockstorageshort}} im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} ansehen, wird neben dem Datenträgernamen für die LUNs, die verschlüsselt sind, ein Sperrsymbol angezeigt.

## Wie weiß ich, dass ich {{site.data.keyword.blockstorageshort}} in einem aktualisierten Rechenzentrum bereitstelle?
{: faq}

Bei der Bestellung von {{site.data.keyword.blockstorageshort}} werden alle aktualisierten Rechenzentren mit einem Stern (`*`) im Bestellformular und mit einem Hinweis gekennzeichnet, dass Sie dabei sind, Speicher mit Verschlüsselung bereitzustellen. Sobald der Speicher bereitgestellt wird, wird in der Speicherliste ein Symbol angezeigt, das auf die Verschlüsselung des Speichers hinweist. Alle verschlüsselten Datenträger und LUNs werden nur in aktualisierten Rechenzentren bereitgestellt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Funktionen finden Sie [hier](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

## Wenn ich über nicht verschlüsselten {{site.data.keyword.blockstorageshort}} in einem Rechenzentrum verfüge, der vor kurzem aktualisiert wurde, kann dieser {{site.data.keyword.blockstorageshort}} dann verschlüsselt werden?
{: faq}

{{site.data.keyword.blockstorageshort}}, der vor der Aktualisierung des Rechenzentrums bereitgestellt wurde, kann nicht verschlüsselt werden.
Neuer {{site.data.keyword.blockstorageshort}}, der in aktualisierten Rechenzentren bereitgestellt wird, wird automatisch verschlüsselt. Es kann keine Einstellung für die Verschlüsselung ausgewählt werden. Diese erfolgt automatisch.
Um Daten auf einem nicht verschlüsselten Speicher in einem aktualisierten Rechenzentrum zu verschlüsseln, können Sie eine neue Block-LUN erstellen und die Daten anschließend mithilfe von hostbasierter Migration an die neu verschlüsselte LUN kopieren. Die Anweisungen dazu finden Sie [hier](migrate-block-storage-encrypted-block-storage.html).

## Unterstützt {{site.data.keyword.blockstorageshort}} die permanente SCSI-3-Reservierung zur Implementierung der E/A-Abschirmung für Db2 pureScale?
{: faq}

Ja, {{site.data.keyword.blockstorageshort}} unterstützt die permanente SCSI-2- und SCSI-3-Reservierung.

## Was passiert mit den Daten, wenn {{site.data.keyword.blockstorageshort}}-LUNs gelöscht werden?
{: faq}

{{site.data.keyword.blockstoragefull}} stellt den Kunden Blockdatenträger auf physischem Speicher bereit, der vor der Wiederverwendung bereinigt wird. Kunden mit bestimmten Compliance-Anforderungen, z. B. hinsichtlich der Einhaltung der NIST 800-88-Richtlinien für das sichere Löschen von Datenträgern, müssen die entsprechende Bereinigungsprozedur durchführen, bevor sie den Speicher löschen.

## Was passiert mit den Laufwerken, die über das Cloud-Rechenzentrum außer Betrieb gesetzt werden?
{: faq}

Wenn Laufwerke außer Betrieb gesetzt werden, werden sie vor dem Entsorgen durch IBM zerstört. Die Laufwerke werden somit unbrauchbar. Auf Daten, die auf diesem Laufwerk gespeichert waren, kann nicht mehr zugegriffen werden.
