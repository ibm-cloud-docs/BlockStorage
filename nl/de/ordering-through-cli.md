---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} über die SLCLI bestellen
{: #orderingthroughCLI}

Sie können die SLCLI verwenden, um Bestellungen für Produkte zu platzieren, die normalerweise über die [{{site.data.keyword.cloud_notm}}-Konsole](https://{DomainName}/){: external} bestellt werden. In der SL-API kann eine Bestellung aus mehreren Bestellungscontainern bestehen. Die Bestell-Befehlszeilenschnittstelle funktioniert nur mit einem Bestellcontainer.

Weitere Informationen zur Installation und Verwendung der SLCLI finden Sie unter [Python-CLI-Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Nach verfügbaren {{site.data.keyword.blockstorageshort}}-Angeboten suchen

Die erste Komponente, nach der Sie suchen, wenn Sie einen Auftrag platzieren, ist ein Paket. Pakete werden auf die verschiedenen Produkte der höchsten Ebene aufgeteilt, die für die Bestellung in {{site.data.keyword.cloud}} verfügbar sind. Einige Beispielpakete sind CLOUD_SERVER für VSIs, BARE_METAL_SERVER für Bare-Metal-Server und STORAGE_AS_A_SERVICE_STAAS für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}.

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
                                 Standardwert: 'storage_as_a_service']
 --billing [hourly|monthly]      Optionaler Parameter für den Abrechnungssatz
                                 (Standardwert: monatlich)
 -h, --help                      Diese Nachricht anzeigen und Ausführung beenden.
```

Weitere Informationen zur {{site.data.keyword.blockstorageshort}}-Bestellung über die API finden Sie unter [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Um auf alle neuen Funktionen zugreifen zu können, müssen Sie `Storage-as-a-Service Package 759` bestellen.
{:tip}

Weitere Informationen zu den Typen von Windows-Betriebssystem finden Sie im Abschnitt mit den [FAQs](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).


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
  -h, --hardware-id TEXT    Die ID eines Servers zur Autorisierung.
  -v, --virtual-id TEXT     Die ID eines virtuellen Servers zur Autorisierung.
  -i, --ip-address-id TEXT  Die ID einer IP-Adresse zur Autorisierung.
  -p, --ip-address TEXT     Eine IP-Adresse zur Autorisierung.
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```

Sie können Hosts autorisieren und verbinden, die sich in demselben Rechenzentrum wie Ihr Speicher befinden. Sie können mehrere Konten haben, Sie können jedoch nicht einen Host über das eine Konto für den Zugriff auf Ihren Speicher in einem anderen Konto berechtigen. Beachten Sie auch, dass ein Host nicht für den Zugriff auf mehrere LUNs unterschiedlicher Betriebssystemtypen gleichzeitig berechtigt werden kann. Ein Host kann nur für den Zugriff auf LUNs eines einzigen Betriebssystemtyps berechtigt werden. Wenn Sie versuchen, den Zugriff auf mehrere LUNs mit unterschiedlichen Betriebssystemtypen zu berechtigen, führt die Operation zu einem Fehler.
{:note}
{:important}

Weitere Informationen zum Autorisieren von Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} über die API finden Sie unter [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}
{:tip}

Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}


## Neuen Speicher verbinden
{: #mountingCLI}

Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu LUNs unter Linux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Verbindung zu LUNs unter CloudLinux herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Verbindung zu LUNS unter Microsoft Windows herstellen](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
