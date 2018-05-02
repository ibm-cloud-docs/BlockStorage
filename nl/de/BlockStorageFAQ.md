---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-24"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Häufig gestellte Fragen zu {{site.data.keyword.blockstorageshort}}

## Wie viele Instanzen können einen bereitgestellten {{site.data.keyword.blockstorageshort}}-Datenträger gemeinsam nutzen?
Der Standardgrenzwert für die Anzahl der Berechtigungen pro Blockdatenträger ist 8. Wenden Sie sich zur Erhöhung des Grenzwerts an Ihren Vertriebsbeauftragten.

## Werden bei der Bereitstellung von Performance oder Endurance {{site.data.keyword.blockstorageshort}} die zugeordneten IOPS nach Instanz oder Datenträger umgesetzt?
IOPS werden auf Datenträgerebene umgesetzt. Anders ausgedrückt, zwei Hosts, die mit einem Datenträger mit 6000 IOPS verbunden sind, nutzen diese 6000 IOPS gemeinsam.

## Wie werden IOPS gemessen?
IOPS werden auf Basis eines Belastungsprofils von 16-KB-Blöcken mit zufälligen 50% Lese- und 50% Schreibvorgängen gemessen. Workloads, die von diesem Profil abweichen, weisen möglicherweise niedrigere Leistungswerte auf.

## Was passiert, wenn ich bei der Leistungsmessung eine kleinere Blockgröße verwende?
Auch bei kleineren Blockgrößen kann man den maximalen IOPS-Wert erhalten. Allerdings verringert sich der Durchsatz. Beispiel: Ein Datenträger mit 6000 IOPS hat bei verschiedenen Blockgrößen den folgenden Durchsatz:

- 16 KB * 6000 IOPS == ~93,75 MB/Sek. 
-  8 KB * 6000 IOPS == ~46,88 MB/Sek.
-  4 KB * 6000 IOPS == ~23,44 MB/Sek.

## Muss der Datenträger aufgewärmt werden, um den erwarteten Durchsatz zu erzielen?
Ein Aufwärmung ist nicht erforderlich. Der angegebene Durchsatz wird sofort bei Bereitstellung des Datenträgers erreicht.

## Warum kann ich {{site.data.keyword.blockstorageshort}} in manchen Rechenzentren mit einem Endurance-10-IOPS-Tier bereitstellen und in anderen nicht?
Das {{site.data.keyword.blockstorageshort}}-Endurance-IOPS/GB-Tier des Speichertyps 10 ist nur in ausgewählten Rechenzentren verfügbar, wobei bald neue Rechenzentren hinzugefügt werden.  Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Funktionen finden Sie hier.

## Wie kann ich erkennen, welche meiner {{site.data.keyword.blockstorageshort}}-LUNs/-Datenträger verschlüsselt sind?
Beim Anzeigen Ihrer {{site.data.keyword.blockstorageshort}}-Liste im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} wird rechts neben der entsprechenden LUN bzw. dem entsprechenden Datenträgernamen ein Sperrsymbol für die Verschlüsselung angezeigt.

## Wie weiß ich, dass ich {{site.data.keyword.blockstorageshort}} in einem aktualisierten Rechenzentrum bereitstelle?
Bei der Bereitstellung von {{site.data.keyword.blockstorageshort}} werden alle aktualisierten Rechenzentren mit einem Stern (`*`) im Bestellformular und mit einem Hinweis gekennzeichnet, dass Sie dabei sind, Speicher mit Verschlüsselung bereitzustellen. Sobald der Speicher bereitgestellt ist, wird in der Speicherliste ein Symbol angezeigt, das auf die Verschlüsselung des Datenträgers oder der LUN hinweist. Alle verschlüsselten Datenträger und LUNs werden nur in aktualisierten Rechenzentren bereitgestellt. Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Funktionen finden Sie hier.

## Warum kann ich {{site.data.keyword.blockstorageshort}} in manchen Rechenzentren mit einem Endurance-10-IOPS-Tier bereitstellen und in anderen nicht?
Das Endurance-IOPS/GB-Tier des Typs 10 ist nur in ausgewählten Rechenzentren verfügbar, wobei bald neue Rechenzentren hinzugefügt werden.  Eine vollständige Liste der aktualisierten Rechenzentren und der verfügbaren Funktionen finden Sie [hier](new-ibm-block-and-file-storage-location-and-features.html).

## Kann ich, wenn ich in einem Rechenzentrum, das für die Verschlüsselung aktualisiert wurde, nicht verschlüsselten {{site.data.keyword.blockstorageshort}} habe, diesen {{site.data.keyword.blockstorageshort}} verschlüsseln?
{{site.data.keyword.blockstorageshort}}, der vor der Aktualisierung eines Rechenzentrums bereitgestellt wird, kann nicht verschlüsselt werden. 
Neuer {{site.data.keyword.blockstorageshort}}, der in aktualisierten Rechenzentren bereitgestellt wird, wird automatisch verschlüsselt. Es kann keine Einstellung für die Verschlüsselung ausgewählt werden. Diese erfolgt automatisch. 
Um Daten auf einem nicht verschlüsselten Speicher in einem aktualisierten Rechenzentrum zu verschlüsseln, können Sie eine neue Block-LUN erstellen und die Daten anschließend mithilfe von hostbasierter Migration an die neu verschlüsselte LUN kopieren. Anweisungen zur Durchführung dieser Migration finden Sie im folgenden [Artikel](migrate-block-storage-encrypted-block-storage).

## Wie viele Datenträger kann ich bereitstellen?
Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen.  Wenden Sie sich zur Erhöhung Ihrer Datenträger an Ihren Vertriebsbeauftragten.

## Kann ich mit einer schnelleren Ethernet-Verbindung einen höheren Durchsatz erreichen?
Die Grenzwerte für den Durchsatz werden auf Datenträger- bzw. LUN-Ebene festgelegt und können somit nicht durch eine schnellere Ethernet-Verbindung erhöht werden. Bei einer langsameren Ethernet-Verbindung jedoch kann Ihre Bandbreite ein potenzieller Engpass sein.

## Beeinträchtigen Firewalls/Sicherheitsgruppen die Leistung?
Es empfiehlt sich, Speicherdatenverkehr über ein VLAN auszuführen, das die Firewall umgeht. Eine Ausführung des Speicherdatenverkehrs über Software-Firewalls erhöht die Latenz und beeinträchtigt die Speicherleistung.

## Welche Leistungslatenz kann ich von meinem {{site.data.keyword.blockstorageshort}} erwarten?   

Die Ziellatenz im Speicher beträgt < 1 ms. Da der vorliegende Speicher mit Recheninstanzen in einem gemeinsamen Netz verbunden ist, hängt die genaue Leistungslatenz vom Netzverkehr in einem bestimmten Zeitrahmen ab.

## Unterstützt {{site.data.keyword.blockstorageshort}} die permanente SCSI-3-Reservierung zur Implementierung der E/A-Abschirmung für Db2 pureScale?
Ja, {{site.data.keyword.blockstorageshort}} unterstützt die permanente SCSI-2- und SCSI-3-Reservierung.

## Was passiert mit meinen Daten, wenn {{site.data.keyword.blockstorageshort}}-LUNs gelöscht werden?

Beim Löschen von Speicher werden alle Verweise auf die Daten auf diesem Datenträger entfernt. Somit ist kein Zugriff auf diese Daten mehr möglich. Wird die physische Speichereinheit einem anderen Konto bereitgestellt, wird eine neue Verweisgruppe zugewiesen. Das neue Konto hat keine Möglichkeit, auf die Daten zuzugreifen, die sich auf der physischen Speichereinheit befunden haben. Die neue Verweisgruppe zeigt ausschließlich den Wert 0 an. Werden neue Daten auf den Datenträger bzw. die LUN geschrieben, werden alle noch vorhandenen nicht zugänglichen Daten überschrieben.

## Was passiert mit den Laufwerken, die über das Cloud-Rechenzentrum außer Betrieb gesetzt werden? 

Wenn Laufwerke außer Betrieb gesetzt werden, werden sie vor dem Entsorgen durch IBM zerstört, sodass sie nicht mehr verwendet werden können. Auf Daten, die auf diesem Laufwerk gespeichert waren, kann nicht mehr zugegriffen werden. 
