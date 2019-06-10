---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-08"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# Tutoriel d'initiation
{: #getting-started}

{{site.data.keyword.blockstoragefull}} est un stockage iSCSI persistant, haute performance, qui est mis à disposition et géré indépendamment des instances de calcul. Les numéros d'unité logique {{site.data.keyword.blockstorageshort}} basés sur iSCSI sont connectés à des périphériques autorisés via des connexions en E-S multi-accès (MPIO) redondantes.

{{site.data.keyword.blockstorageshort}} fournit les meilleurs niveaux de durabilité et de disponibilité du marché grâce à un ensemble de fonctionnalités hors pair. Il a été construit dans le respect des normes de l'industrie et des meilleures pratiques. {{site.data.keyword.blockstorageshort}} est conçu pour protéger l'intégrité des données et assurer la disponibilité pendant des événements de maintenance et des pannes inattendues et offrir une base de référence cohérente pour les performances.
{:shortdesc}

## Avant de commencer
{: #prereqs}

Des numéros d'unité logique {{site.data.keyword.blockstorageshort}} peuvent être mis à disposition de 20 Go à 12 To avec deux options : <br/>
- Effectuez la mise à disposition avec des niveaux **Endurance** offrant des niveaux de performance prédéfinis et d'autres fonctionnalités telles que les instantanés et la réplication.
- Créez un environnement de **Performance** haute puissance avec des opérations d'entrée-sortie par seconde (IOPS) allouées.

Pour en savoir plus sur l'offre {{site.data.keyword.blockstorageshort}}, voir [A propos de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

## Remarques sur la mise à disposition

### Taille de bloc

Les IOPS pour les niveaux Endurance et Performance se fondent sur une taille de bloc de 16 Ko avec une charge de travail aléatoire/séquentielle de 50/50 et de lecture/écriture de 50/50. Un bloc de 16 Ko équivaut à une écriture sur le volume.
{:important}

La taille de bloc utilisée par votre application a une incidence directe sur les performances de stockage. Si la taille de bloc employée par votre application est inférieure à 16 Ko, la limite des opérations d'entrée-sortie par seconde est atteinte avant la limite de débit. A l'inverse, si la taille de bloc qui est utilisée par votre application est supérieure à 16 Ko, la limite de débit est atteinte avant la limite des opérations d'entrée-sortie par seconde.

| Taille de bloc (ko) | IOPS | Débit (Mo/s) |
|-----|-----|-----|
| 4 | 1 000 | 16 |
| 8 | 1 000 | 16 |
| 16 | 1 000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Le tableau 1 présente des exemples de l'impact de la taille de bloc et des opérations d'entrée-sortie par seconde sur le débit.<br/Taille E-S moyennes x IOPS = Débit en Mo/s." caption-side="top"}

### Hôtes autorisés

Un autre facteur à prendre en compte est le nombre d'hôtes qui utilisent votre volume. Si un seul hôte accède au volume, il peut s'avérer difficile de réaliser le nombre maximal d'IOPS disponible, surtout avec des nombres d'IOPS extrêmes (10 000). Si votre charge de travail requiert un débit élevé, il est préférable de configurer au moins deux serveurs pour accéder à votre volume afin d'éviter un goulot d'étranglement dû à un seul serveur.

### Connexion réseau

La vitesse de votre connexion Ethernet doit être supérieure au débit maximal attendu de votre volume. En règle générale, vous ne devriez pas saturer votre connexion Ethernet au-delà de 70 % de la bande passante disponible. Par exemple, si vous disposez de 6 000 IOPS et que vous utilisez une taille de bloc de 16 Ko, le volume peut traiter un débit d'environ 94 Mo par seconde. Si vous disposez d'une connexion Ethernet de 1 Gbit/s
vers votre numéro d'unité logique, vous rencontrez un goulot d'étranglement lorsque vos serveurs tentent d'utiliser le débit maximal disponible. Cela est dû au fait que 70 % de la limite théorique d'une connexion Ethernet de 1 Gbit/s (125 Mo par seconde) n'autorisent que 88 Mo par seconde.

Pour atteindre le nombre maximal d'IOPS, vous devez mettre en place les ressources réseau adéquates. Vous devez également tenir compte de l'utilisation du réseau privé en dehors du stockage, ainsi que des réglages côté hôte et spécifiques aux applications (pile IP ou [nombre de lignes de file d'attente](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings), etc.).

Le trafic de stockage doit être isolé des autres types de trafic et il ne doit pas être dirigé via des pare-feu et des routeurs. Pour plus d'informations, voir la [Foire aux questions](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic).

Le trafic de stockage est inclus dans l'utilisation réseau totale des serveurs virtuels publics. Pour plus d'informations sur les limites que peut imposer le service, voir la [documentation sur les serveurs virtuels](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers).
{:tip}

## Soumission de votre commande
{: #submitorder}

Lorsque vous êtes prêt à passer votre commande, utilisez la [console](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) ou [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI).

## Connexion de votre nouveau stockage
{: #mountingstorage}

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuration de Block Storage pour une sauvegarde avec cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuration de Block Storage pour une sauvegarde avec Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Gestion de votre nouveau stockage

Via le portail ou l'interface SLCLI, vous pouvez gérer différents aspects de votre stockage de fichiers, tels que les autorisations et annulations d'hôtes. Pour plus d'informations, voir [Gestion de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
