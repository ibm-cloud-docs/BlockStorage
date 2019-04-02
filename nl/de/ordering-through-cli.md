---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} über die SLCLI bestellen
{: #orderingthroughCLI}

Sie können die SLCLI verwenden, um Bestellungen für Produkte zu platzieren, die normalerweise über das [ {{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") ](https://control.softlayer.com/){:new_window} bestellt werden. In der SL-API kann eine Bestellung aus mehreren Bestellungscontainern bestehen. Die Bestell-Befehlszeilenschnittstelle funktioniert nur mit einem Bestellcontainer.

Weitere Informationen zur Installation und Verwendung der SLCLI finden Sie unter [Python-API-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Nach verfügbaren {{site.data.keyword.blockstorageshort}}-Angeboten suchen

Die erste Komponente, nach der Sie suchen, wenn Sie einen Auftrag platzieren, ist ein Paket. Pakete werden auf die verschiedenen Produkte der höchsten Ebene aufgeteilt, die für die Bestellung in {{site.data.keyword.BluSoftlayer_full}} verfügbar sind. Einige Beispielpakete sind CLOUD_SERVER für VSIs, BARE_METAL_SERVER für Bare-Metal-Server und STORAGE_AS_A_SERVICE_STAAS für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}.

Innerhalb eines Pakets werden einige Elemente in Kategorien unterteilt. Für einige Pakete sind Voreinstellungen vorhanden, für andere müssen die Elemente einzeln angegeben werden. Wenn die Kategorie eines Pakets erforderlich ist, muss ein Element aus dieser Kategorie ausgewählt werden, um das Paket zu bestellen. Abhängig von der Kategorie können sich einige Elemente in der Kategorie gegenseitig ausschließen.

Jede Bestellung muss eine zugeordnete Position (Rechenzentrum) haben. Stellen Sie bei der Bestellung von {{site.data.keyword.blockstorageshort}} sicher, dass es an derselben Position wie Ihre Berechnungsinstanzen bereitgestellt wird.
{:important}

Sie können den Befehl `slcli order package-list` verwenden, um das Paket zu suchen, das Sie bestellen möchten. Die Option `-keyword` wird bereitgestellt, um eine einfache Such- und Filterfunktion auszuführen. Diese Option erleichtert die Suche nach dem benötigten Paket. Suchen Sie nach **Storage-as-a-Service Package 759**.

```
$ slcli order package-list --help
Syntax: slcli order package-list [OPTIONS]

  Pakete auflisten, die über die API placeOrder bestellt werden können.

  Beispiel:
      # Alle Pakete für die Bestellung auflisten
      slcli order package-list

  Schlüsselwörter können auch für einige einfache Filterfunktionen verwendet werden,
  um die Suche nach einem Paket zu vereinfachen.

  Beispiel:
     # Alle Pakete mit "Server" im Namen aufführen
      slcli order package-list --keyword server

Optionen:
  --keyword TEXT  Ein Wort (oder eine Zeichenfolge) zum Filtern von Paketnamen.
  -h, --help      Diese Nachricht anzeigen und Ausführung beenden.
```

Der Befehl `slcli block volume-order` kann ebenfalls verwendet werden.

```
# slcli block volume-order --help
Syntax: slcli block volume-order [OPTIONS]

 Blockspeicherdatenträger bestellen.

Optionen:
 --storage-type [performance|endurance]
                                 Typ des Blockspeicherdatenträgers [erforderlich]
 --size INTEGER                  Größe des Blockspeicherdatenträgers in GB.
                                 Zulässige Größen:
                                 20, 40, 80, 100, 250, 500,
                                 1000, 2000, 4000, 8000, 12000  [erforderlich]
 --iops INTEGER                  Performance-Speicher-IOPs, zwischen 100 und
                                 6000 in Vielfachen von 100 [für Speichertyp
                                 Performance erforderlich]
 --tier [0.25|2|4|10]            Endurance-Speicher-Tier (IOP pro GB)
                                 [für Speichertyp Endurance erforderlich]
 --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                 Betriebssystem [erforderlich]
 --location TEXT                 Kurzname des Rechenzentrums (z. B.: dal09)
                                 [erforderlich]
 --snapshot-size INTEGER         Optionaler Parameter für die Bestellung des
                                 Snapshotspeicherbereichs mit Endurance-
                                 Blockspeicher; gibt die Größe (in GB) des zu
                                 bestellenden Snapshotspeicherbereichs an
 --service-offering [storage_as_a_service|enterprise|performance]
                                 Serviceangebotspaket, das für die Bestellung
                                 verwendet werden soll [optional, Standardwert:
                                 'storage_as_a_service']
 --billing [hourly|monthly]      Optionaler Parameter für den Abrechnungssatz
                                 (Standardwert: monatlich)
 -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
```

Weitere Informationen zur {{site.data.keyword.blockstorageshort}}-Bestellung finden über die API Sie unter [order_block_volume ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}.
Um auf alle neuen Funktionen zugreifen zu können, müssen Sie `Storage-as-a-Service Package 759` bestellen.
{:tip}


## Bestellung aufgeben

Das folgende Beispiel veranschaulicht die Bestellung eines 80-GB-{{site.data.keyword.blockstorageshort}}-Datenträgers mit einem 20-GB-Snapshotspeicherbereich und 0,25 E/A-Operationen pro Sekunde/GB.

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}- und {{site.data.keyword.filestorage_short}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen der Grenzwerte finden Sie in [Speichergrenzwerte verwalten](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).
{:important}

## Hosts für den Zugriff auf den neuen Speicher autorisieren

```
slcli block access-authorize --help
Syntax: slcli block access-authorize [OPTIONS] VOLUME_ID

  Host für den Zugriff auf einen angegebenen Datenträger berechtigen.

Optionen:
  -h, --hardware-id TEXT    ID einer SoftLayer-Hardware zur Berechtigung
  -v, --virtual-id TEXT     ID eines virtuellen SoftLayer-Gastsystems zur Berechtigung
  -i, --ip-address-id TEXT  ID der Teilnetz-IP-Adresse eines SoftLayer-Netzes
                            zur Berechtigung
  --ip-address TEXT         IP-Adresse zur Berechtigung
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

Weitere Informationen zum Autorisieren von Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} über die API finden Sie unter [authorize_host_to_volume ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Neuen Speicher verbinden
{: #mountingCLI}

Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verbindung zu LUNs unter CloudLinux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Verbindung zu LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
