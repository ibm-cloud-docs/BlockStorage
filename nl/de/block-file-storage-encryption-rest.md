---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ihre Daten schützen - Anbietergesteuerte Verschlüsselung ruhender Daten

## {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_full_notm}} - Verschlüsselung ruhender Daten 

{{site.data.keyword.BluSoftlayer_full}} nimmt das Bedürfnis nach Sicherheit ernst und versteht, wie wichtig es ist, Daten aus Sicherheitsgründen verschlüsseln zu können. Bei der anbietergesteuerten Verschlüsselung wird sowohl {{site.data.keyword.blockstoragefull}} als auch {{site.data.keyword.filestorage_full}}, mit Endurance oder Performance bereitgestellt, standardmäßig ohne Zusatzkosten und ohne Beeinträchtigung der Leistung verschlüsselt.

Bei der anbietergesteuerten ruhenden Verschlüsselungsfunktion werden folgende Branchenstandardprotokolle verwendet:

* Branchenstandardverschlüsselung AES-256
* Schlüssel werden unternehmensintern mit dem Branchenstandardprotokoll KMIP (Key Management Interoperability Protocol) verwaltet
* Speicherung ist per Federal Information Processing Standard (FIPS) Publication 140-2 für Konformität mit Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA), Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) und EU Data Protection Directive 95/46/EC validiert

## Ruhende Verschlüsselung zur Speicherung von Snapshots oder Replikaten  

Auch alle Screenshots und Replikate von verschlüsseltem {{site.data.keyword.blockstorageshort}} werden standardmäßig verschlüsselt. Diese Funktion kann nicht auf einzelnen Datenträgern inaktiviert werden.

## Speicher mit Verschlüsselung bereitstellen

Die anbietergesteuerte ruhende Verschlüsselungsfunktion ist nur verfügbar, wenn {{site.data.keyword.blockstorageshort}} in ausgewählten Rechenzentren bereitgestellt wird, wobei regelmäßig eine neue Rechenzentrumsverfügbarkeit hinzugefügt wird. Der gesamte in diesen Rechenzentren bereitgestellte Speicher wird automatisch mit Verschlüsselung für ruhende Daten bereitgestellt. Klicken Sie [hier](new-ibm-block-and-file-storage-location-and-features.html), um eine Liste der Rechenzentren anzuzeigen, in denen eine {{site.data.keyword.blockstorageshort}}-Verschlüsselung für ruhende Daten verfügbar ist.

Wählen Sie bei der Bestellung von {{site.data.keyword.blockstorageshort}} ein Rechenzentrum aus, das mit einem * markiert ist und die Nachricht enthält, dass Verschlüsselung verfügbar ist. Rechts neben dem Feld mit der LUN / dem Datenträgernamen wird ein Sperrsymbol für die Verschlüsselung angezeigt. Siehe Abbildung 1.

![Das Sperrsymbol gibt an, dass die LUN verschlüsselt ist.](/images/encryptedstorage.png)
<caption>Abbildung 1. Beispiel des Sperrsymbols als Zeichen, dass die LUN verschlüsselt ist.</caption>



**Hinweis**: Nicht verschlüsselter Speicher, der vor dem Upgrade eines Rechenzentrums bereitgestellt wird, wird **nicht** automatisch verschlüsselt. Wenn in einem Rechenzentrum nach einem Upgrade nicht verschlüsselter Speicher vorhanden ist, müssen Sie eine neue LUN oder einen neuen Datenträger erstellen und eine Datenmigration durchführen. Anweisungen dazu finden Sie in den folgenden Artikeln.

* [{{site.data.keyword.blockstorageshort}}-Migration in Rechenzentren nach Upgrades](migrate-block-storage-encrypted-block-storage.html)
