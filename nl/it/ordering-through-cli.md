---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ordinazione di {{site.data.keyword.blockstorageshort}} tramite la CLI SL

Puoi utilizzare la CLI SL per effettuare degli ordini per prodotti che vengono normalmente ordinati tramite il [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}. Nella API SL, un ordine può essere costituito da più contenitori di ordine. La CLI degli ordini funziona solo con un singolo contenitore di ordine. 

Per ulteriori informazioni su come installare e utilizzare la CLI SL, consulta [Python API Client ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Ricerca di offerte di {{site.data.keyword.blockstorageshort}} disponibili

Il primo componente da ricercare quando effettui un ordine è un pacchetto. I pacchetti sono divisi tra i diversi prodotti di livello superiore disponibili per l'ordinazione in {{site.data.keyword.BluSoftlayer_full}}. Alcuni pacchetti di esempio sono CLOUD_SERVER per le VSI, BARE_METAL_SERVER per i server bare metal e STORAGE_AS_A_SERVICE_STAAS per {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}.

All'interno di un pacchetto, alcuni elementi sono suddivisi in categorie. Alcuni pacchetti hanno delle preimpostazioni per tua comodità e altri richiedono che gli elementi vengano specificati singolarmente. Se la categoria di un pacchetto è richiesta, per ordinare il pacchetto è necessario che venga scelto un elemento da tale categoria. A seconda della categoria, alcuni elementi all'interno della categoria possono escludersi a vicenda.

Ogni ordine deve avere un'ubicazione associata (data center). Quando ordini {{site.data.keyword.blockstorageshort}}, assicurati che ne venga eseguito il provisioning nella stessa ubicazione delle tue istanze di calcolo.
{:important}

Puoi utilizzare il comando `slcli order package-list` per trovare il pacchetto che vuoi ordinare. Viene fornita un'opzione `–keyword` per eseguire semplici operazioni di ricerca e filtro. Questa opzione rende più facile trovare il pacchetto di cui hai bisogno.

```
$ slcli order package-list --help
Usage: slcli order package-list [OPTIONS]

  List packages that can be ordered via the placeOrder API.

  Example:
      # List out all packages for ordering
      slcli order package-list

  Keywords can also be used for some simple filtering functionality to help
  find a package easier.

  Example:
     # List out all packages with "server" in the name
      slcli order package-list --keyword server

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

*Necessità delle istruzioni relative a come trovare il pacchetto Storage-as-a-Service 759*

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

Seleziona il resto dei tuoi elementi per l'ordine utilizzando il comando `item-list`. I pacchetti normalmente hanno molti elementi tra cui scegliere, per cui utilizza l'opzione `–category` per richiamare gli elementi solo dalla categoria a cui sei interessato.

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

Per ulteriori informazioni sull'ordinazione di {{site.data.keyword.blockstorageshort}} tramite l'API, consulta [order_block_volume ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}.
Per poter accedere a tutte le nuove funzioni, ordina `Storage-as-a-Service Package 759`.
{:tip}

## Verifica dell'ordine

Se non sei sicuro delle categorie richieste che potrebbero mancare nel tuo ordine, puoi utilizzare il comando `place` con l'indicatore `–verify`. Se manca qualche categoria, viene visualizzata sullo schermo. 


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

L'output mostra ogni elemento ordinato, insieme al costo ad esso associato. Se l'ordine supera la verifica, significa che non ci sono elementi in conflitto e che tutte le categorie richieste hanno un elemento specificato nell'ordine.

## Effettuazione dell'ordine 

Il passo successivo consiste nell'effettuare l'ordine. 

```
$ slcli order place .....

This action will incur charges on your account. Continue? [y/N]: y

API response
```

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Per ulteriori informazioni sull'aumento dei limiti, consulta [Gestione dei limiti di archiviazione](managing-storage-limits.html).
{:important}

## Autorizzazione degli host ad accedere alla nuova archiviazione

DA DEFINIRE

Per ulteriori informazioni sull'autorizzazione degli host ad accedere a {{site.data.keyword.blockstorageshort}} tramite l'API, consulta [authorize_host_to_volume ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}
{:tip}

Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](faqs.html)
{:important}

## Connessione alla tua nuova archiviazione

A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html)
- [Connessione ai LUN iSCSI MPIO su CloudLinux](configure-iscsi-cloudlinux.html)
- [Connessione ai LUN iSCSI MPIO su Microsoft Windows](accessing-block-storage-windows.html)
- [Configurazione di Block Storage per il backup con cPanell](configure-backup-cpanel.html)
- [Configurazione di Block Storage per il backup con Plesk](configure-backup-plesk.html)
