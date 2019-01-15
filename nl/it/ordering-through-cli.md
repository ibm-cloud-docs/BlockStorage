---

copyright:
  years: 2014, 2019
lastupdated: "2019-01-07"

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

Puoi utilizzare il comando `slcli order package-list` per trovare il pacchetto che vuoi ordinare. Viene fornita un'opzione `–keyword` per eseguire semplici operazioni di ricerca e filtro. Questa opzione rende più facile trovare il pacchetto di cui hai bisogno. Cerca **Storage-as-a-Service Package 759**.

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

Puoi anche utilizzare il comando `slcli block volume-order`.

```
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Order a block storage volume.

Options:
 --storage-type [performance|endurance]
                                 Type of block storage volume  [required]
 --size INTEGER                  Size of block storage volume in GB.
                                 Permitted Sizes:
                                 20, 40, 80, 100, 250, 500,
                                 1000, 2000, 4000, 8000, 12000  [required]
 --iops INTEGER                  Performance Storage IOPs, between 100 and
                                 6000 in multiples of 100  [required for
                                 storage-type performance]
 --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                 [required for storage-type endurance]
 --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                 Operating System  [required]
 --location TEXT                 Datacenter short name (e.g.: dal09)
                                 [required]
 --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                 space along with endurance block storage;
                                 specifies the size (in GB) of snapshot space
                                 to order
 --service-offering [storage_as_a_service|enterprise|performance]
                                 The service offering package to use for
                                 placing the order [optional, default is
                                 'storage_as_a_service']
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

Per ulteriori informazioni sull'ordinazione di {{site.data.keyword.blockstorageshort}} tramite l'API, consulta [order_block_volume ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}.
Per poter accedere a tutte le nuove funzioni, ordina `Storage-as-a-Service Package 759`.
{:tip}


## Effettuazione dell'ordine

Il seguente esempio mostra come ordinare un volume {{site.data.keyword.blockstorageshort}} da 80-GB con 20-GB di spazio dell'istantanea e 0,25 IOPS per ogni GB.

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Per ulteriori informazioni sull'aumento dei limiti, consulta [Gestione dei limiti di archiviazione](managing-storage-limits.html).
{:important}

## Autorizzazione degli host ad accedere alla nuova archiviazione

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

Per ulteriori informazioni sull'autorizzazione degli host ad accedere a {{site.data.keyword.blockstorageshort}} tramite l'API, consulta [authorize_host_to_volume ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}
{:tip}

Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](faqs.html)
{:important}

## Connessione alla tua nuova archiviazione

A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN iSCSI su Linux](accessing_block_storage_linux.html)
- [Connessione ai LUN iSCSI su CloudLinux](configure-iscsi-cloudlinux.html)
- [Connessione ai LUN iSCSI su Microsoft Windows](accessing-block-storage-windows.html)
- [Configurazione di Block Storage per il backup con cPanell](configure-backup-cpanel.html)
- [Configurazione di Block Storage per il backup con Plesk](configure-backup-plesk.html)
