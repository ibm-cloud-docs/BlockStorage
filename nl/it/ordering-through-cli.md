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

# Ordinazione di {{site.data.keyword.blockstorageshort}} tramite la SLCLI
{: #orderingthroughCLI}

Puoi utilizzare la SLCLI per effettuare degli ordini per prodotti che vengono normalmente ordinati tramite la [console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}. Nella API SL, un ordine può essere costituito da più contenitori di ordine. La CLI degli ordini funziona solo con un singolo contenitore di ordine.

Per ulteriori informazioni su come installare e utilizzare la SLCLI, vedi [Python CLI Client](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Ricerca di offerte di {{site.data.keyword.blockstorageshort}} disponibili

Il primo componente da ricercare quando effettui un ordine è un pacchetto. I pacchetti sono divisi tra i diversi prodotti di livello superiore disponibili per l'ordinazione in {{site.data.keyword.cloud}}. Alcuni pacchetti di esempio sono CLOUD_SERVER per le VSI, BARE_METAL_SERVER per i server bare metal e STORAGE_AS_A_SERVICE_STAAS per {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}.

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
                                 The default is 'storage_as_a_service'
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

Per ulteriori informazioni sull'ordine di {{site.data.keyword.blockstorageshort}} tramite l'API, vedi [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Per poter accedere a tutte le nuove funzioni, ordina `Storage-as-a-Service Package 759`.
{:tip}

Per ulteriori informazioni sui vari tipi di SO di Windows, consulta [FAQ](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).


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

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Per ulteriori informazioni sull'aumento dei limiti, consulta [Gestione dei limiti di archiviazione](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).
{:important}

## Autorizzazione degli host ad accedere alla nuova archiviazione

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The ID of one hardware server to authorize.
  -v, --virtual-id TEXT     The ID of one virtual server to authorize.
  -i, --ip-address-id TEXT  The ID of one IP address to authorize.
  -p, --ip-address TEXT     An IP address to authorize.
  --help                    Show this message and exit.
```

Puoi autorizzare e connettere gli host che si trovano nello stesso data center della tua archiviazione. Puoi avere più account ma non puoi autorizzare un host da un account ad accedere alla tua archiviazione su un altro account. Inoltre nota che un host non può essere autorizzato ad accedere a più LUN di diversi tipi di SO contemporaneamente. Un host può soltanto essere autorizzato ad accedere a LUN di un solo tipo di SO. Se tenti di autorizzare l'accesso a più LUN con diversi tipi di SO, l'operazione genera un errore.
{:note}
{:important}

Per ulteriori informazioni sull'autorizzazione degli host ad accedere a {{site.data.keyword.blockstorageshort}} tramite l'API, vedi [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}
{:tip}

Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](/docs/infrastructure/BlockStorage?topic=block-storage-faqs)
{:important}


## Connessione alla tua nuova archiviazione
{: #mountingCLI}

A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connessione ai LUN su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connessione ai LUN su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurazione di Block Storage per il backup con cPanell](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurazione di Block Storage per il backup con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
