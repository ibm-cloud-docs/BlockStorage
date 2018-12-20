---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# {{site.data.keyword.blockstorageshort}} über die SL-Befehlszeilenschnittstelle bestellen

Sie können die SL-Befehlszeilenschnittstelle verwenden, um Bestellungen für Produkte zu platzieren, die normalerweise über das [ {{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link") ](https://control.softlayer.com/){:new_window} bestellt werden. In der SL-API kann eine Bestellung aus mehreren Bestellungscontainern bestehen. Die Bestell-Befehlszeilenschnittstelle funktioniert nur mit einem Bestellcontainer.

Weitere Informationen zur Installation und Verwendung der SL-Befehlszeilenschnittstelle finden Sie unter [Python-API-Client ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Nach verfügbaren {{site.data.keyword.blockstorageshort}}-Angeboten suchen

Die erste Komponente, nach der Sie suchen, wenn Sie einen Auftrag platzieren, ist ein Paket. Pakete werden auf die verschiedenen Produkte der höchsten Ebene aufgeteilt, die für die Bestellung in {{site.data.keyword.BluSoftlayer_full}} verfügbar sind. Einige Beispielpakete sind CLOUD_SERVER für VSIs, BARE_METAL_SERVER für Bare-Metal-Server und STORAGE_AS_A_SERVICE_STAAS für {{site.data.keyword.blockstorageshort}} und {{site.data.keyword.filestorage_short}}.

Innerhalb eines Pakets werden einige Elemente in Kategorien unterteilt. Für einige Pakete sind Voreinstellungen vorhanden, für andere müssen die Elemente einzeln angegeben werden. Wenn die Kategorie eines Pakets erforderlich ist, muss ein Element aus dieser Kategorie ausgewählt werden, um das Paket zu bestellen. Abhängig von der Kategorie können sich einige Elemente in der Kategorie gegenseitig ausschließen.

Jede Bestellung muss eine zugeordnete Position (Rechenzentrum) haben. Stellen Sie bei der Bestellung von {{site.data.keyword.blockstorageshort}} sicher, dass es an derselben Position wie Ihre Berechnungsinstanzen bereitgestellt wird.
{:important}

Sie können den Befehl `slcli order package-list` verwenden, um das Paket zu suchen, das Sie bestellen möchten. Die Option `-keyword` wird bereitgestellt, um eine einfache Such- und Filterfunktion auszuführen. Diese Option erleichtert die Suche nach dem benötigten Paket.

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

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
  -h, --help      Diese Nachricht anzeigen und beenden.
```

*Benötige Anweisungen zum Suchen von Storage-as-a-Service Package 759*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         name        :       keyName       :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               name               :    categoryCode   : isRequired :
:..................................:...................:............:
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:..................................:...................:............:
```

Wählen Sie den Rest Ihrer Artikel für die Bestellung aus, indem Sie den Befehl `item-list` verwenden. Bei Paketen stehen normalerweise zahlreiche Elemente zur Auswahl. Verwenden Sie die Option `–category`, um die Artikel nur aus der gewünschten Kategorie abzurufen.

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         keyName          :                description                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

Weitere Informationen zur {{site.data.keyword.blockstorageshort}}-Bestellung finden über die API Sie unter [order_block_volume ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}.
Um auf alle neuen Funktionen zugreifen zu können, müssen Sie `Storage-as-a-Service Package 759` bestellen.
{:tip}

## Bestellung überprüfen

Wenn Sie sich nicht sicher sind, welche Kategorien in Ihrer Bestellung fehlen, können Sie den Befehl `place` mit dem Flag `-verify` verwenden. Wenn Kategorien fehlen, werden diese auf dem Bildschirm angezeigt.


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                keyName                       :                   description                   : cost :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

Die Ausgabe zeigt jedes bestellte Element zusammen mit den Kosten, die dem jeweiligen Artikel zugeordnet sind. Wenn die Bestellung überprüft wird, bedeutet dies, dass es keine in Konflikt stehenden Elemente gibt und alle erforderlichen Kategorien über ein Element verfügen, das in der Reihenfolge angegeben ist.

## Bestellung aufgeben

Der nächste Schritt besteht darin, die Bestellung zu platzieren.

```
$ slcli order place .....

Für diese Aktion fallen Gebühren auf Ihrem Konto an. Soll der Vorgang fortgesetzt werden?[y/N]: y

API-Antwort
```

Standardmäßig können Sie insgesamt 250 {{site.data.keyword.blockstorageshort}}-Datenträger bereitstellen. Wenden Sie sich an Ihren Vertriebsbeauftragten, wenn Sie die Anzahl Ihrer Datenträger erhöhen möchten. Weitere Informationen zum Erhöhen der Grenzwerte finden Sie in [Speichergrenzwerte verwalten](managing-storage-limits.html).
{:important}

## Hosts für den Zugriff auf den neuen Speicher autorisieren

TBD

Weitere Informationen zum Autorisieren von Hosts für den Zugriff auf {{site.data.keyword.blockstorageshort}} über die API finden Sie unter [authorize_host_to_volume ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}.
{:tip}

Informationen zum Grenzwert für gleichzeitige Autorisierungen finden Sie im Abschnitt [Häufig gestellte Fragen (FAQs)](faqs.html).
{:important}

## Neuen Speicher verbinden

Verwenden abhängig vom Betriebssystem des Hosts den entsprechenden Link.
- [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html)
- [Verbindung zu MPIO-iSCSI-LUNs unter CloudLinux herstellen](configure-iscsi-cloudlinux.html)
- [Verbindung zu MPIO-iSCSI-LUNS unter Microsoft Windows herstellen](accessing-block-storage-windows.html)
- [Blockspeicher für Sicherung mit cPanel konfigurieren](configure-backup-cpanel.html)
- [Blockspeicher für Sicherung mit Plesk konfigurieren](configure-backup-plesk.html)
