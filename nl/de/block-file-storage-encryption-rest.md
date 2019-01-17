---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-04"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Vom Provider verwaltete Verschlüsselung ruhender Daten

## {{site.data.keyword.blockstorageshort}} - Verschlüsselung ruhender Daten

{{site.data.keyword.BluSoftlayer_full}} nimmt das Bedürfnis nach Sicherheit ernst und versteht, wie wichtig es ist, Daten aus Sicherheitsgründen verschlüsseln zu können. Bei der anbietergesteuerten Verschlüsselung wird {{site.data.keyword.blockstoragefull}} standardmäßig entweder mit der Option 'Endurance' oder 'Performance', ohne Zusatzkosten und ohne Beeinträchtigung der Leistung verschlüsselt, bereitgestellt.

Bei der anbietergesteuerten ruhenden Verschlüsselungsfunktion werden folgende Branchenstandardprotokolle verwendet:

* Branchenstandardverschlüsselung AES-256
* Schlüssel werden unternehmensintern mit dem Branchenstandardprotokoll KMIP (Key Management Interoperability Protocol) verwaltet
* Die Speicherung wird per Federal Information Processing Standard (FIPS) Publication 140-2 für Konformität mit Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA) validiert. Die Speicherung ist auch für Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) und EU Data Protection Directive 95/46/EC validiert.

## Ruhende Verschlüsselung zur Speicherung von Snapshots oder Replikaten angeben  

Auch alle Screenshots und Replikate von verschlüsseltem {{site.data.keyword.blockstorageshort}} werden standardmäßig verschlüsselt. Diese Funktion kann nicht auf einzelnen Datenträgern inaktiviert werden.

## Speicher mit Verschlüsselung bereitstellen

Die anbietergesteuerte ruhende Verschlüsselungsfunktion ist für {{site.data.keyword.blockstorageshort}}-Instanzen verfügbar, die in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) bereitgestellt werden. Der gesamte in diesen Rechenzentren bestellte Speicher wird automatisch mit Verschlüsselung für ruhende Daten bereitgestellt.

Wählen Sie bei der Bestellung von {{site.data.keyword.blockstorageshort}} ein Rechenzentrum aus, das mit einem Stern (`*`) markiert ist. Rechts neben dem Feld mit der LUN bzw. dem Datenträgernamen wird ein Sperrsymbol für die Verschlüsselung des Datenträgers angezeigt.

![Das Sperrsymbol gibt an, dass die LUN verschlüsselt ist.](/images/encryptedstorage.png)
<caption>Abbildung 1. Beispiel des Sperrsymbols, das angibt, dass die LUN verschlüsselt ist.</caption>



Nicht verschlüsselter Speicher, der vor dem Upgrade des Rechenzentrums bereitgestellt wurde, **wird nicht** automatisch verschlüsselt. Wenn Sie in einem Rechenzentrum nach einem Upgrade über nicht verschlüsselten Speicher verfügen und den Speicher verschlüsseln möchten, müssen Sie einen neuen Datenträger erstellen und eine Datenmigration durchführen. Weitere Informationen finden Sie im Abschnitt [{{site.data.keyword.blockstorageshort}}-Migration in Rechenzentren nach Upgrades](migrate-block-storage-encrypted-block-storage.html).
{:important}
