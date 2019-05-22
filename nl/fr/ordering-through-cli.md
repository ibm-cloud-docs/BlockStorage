---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Commande de {{site.data.keyword.blockstorageshort}} via l'interface SLCLI
{: #orderingthroughCLI}

Vous pouvez utiliser l'interface SLCLI pour commander des produits qui se commandent normalement via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. Dans l'API SL, une commande peut se composer de plusieurs conteneurs de commandes. L'interface de ligne de commande pour les commandes fonctionne avec un seul conteneur de commandes.

Pour plus d'informations sur l'installation et l'utilisation de l'interface SLCLI, voir [Client API Python](https://softlayer-python.readthedocs.io/en/latest/cli.html){: external}.
{:tip}

## Recherche des offres {{site.data.keyword.blockstorageshort}} disponibles

Lorsque vous passez une commande, vous devez commencer par rechercher est un package. Les packages sont répartis entre les différents produits de niveau supérieur que vous pouvez commander dans {{site.data.keyword.BluSoftlayer_full}}. Entre autres exemples de packages, citons CLOUD_SERVER pour instances de serveur virtuel, BARE_METAL_SERVER pour serveurs bare metal et STORAGE_AS_A_SERVICE_STAAS pour {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}}.

Dans un package, certains éléments sont subdivisés en catégories. Certains packages disposent de préréglages pour votre commodité et d'autres nécessitent que des éléments soient spécifiés individuellement. Si une catégorie de package est requise, un élément de cette catégorie doit être sélectionné pour commander le package. Selon la catégorie, certains éléments de la catégorie s'excluent mutuellement.

A chaque commande doit être associé un emplacement (centre de données). Lorsque vous commandez {{site.data.keyword.blockstorageshort}}, veillez à ce que sa mise disposition s'effectue dans le même emplacement que vos instances de traitement.
{:important}

Vous pouvez utiliser la commande `slcli order package-list` pour rechercher le package que vous voulez commander. Une option `–keyword` est fournie pour effectuer une recherche et un filtrage simples. Cette option facilite la recherche du package dont vous avez besoin. Recherchez **Storage-as-a-Service Package 759**.

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

Vous pouvez également utiliser la commande `slcli block volume-order`.

```
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Permet de commander un volume de stockage par blocs.

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

Pour plus d'informations sur les commande {{site.data.keyword.blockstorageshort}} via l'API, voir [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Pour pouvoir accéder à toutes les nouvelles fonctions, commandez `Storage-as-a-Service Package 759`.
{:tip}


## Passation de la commande

L'exemple suivant illustre la commande d'un volume {{site.data.keyword.blockstorageshort}} de 80 Go avec 20 Go d'espace d'instantané et 0,25 E-S/s par Go.

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}} et {{site.data.keyword.filestorage_short}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour plus d'informations sur l'augmentation des limites, voir [Gestion des limites de stockage](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).
{:important}

## Autorisation des hôtes pour l'accès au nouveau stockage

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

Pour plus d'informations sur l'autorisation des hôtes à accéder à {{site.data.keyword.blockstorageshort}} via l'API, voir [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}
{:tip}

Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Connexion de votre nouveau stockage
{: #mountingCLI}

Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuration de Block Storage pour une sauvegarde avec cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuration de Block Storage pour une sauvegarde avec Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
