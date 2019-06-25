---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Extension de la capacité de stockage par blocs
{: #expandingcapacity}

Cette fonctionnalité permet aux utilisateurs d'{{site.data.keyword.blockstoragefull}} d'étendre immédiatement la taille de leur stockage {{site.data.keyword.blockstorageshort}} en incréments de Go jusqu'à 12 To. Ils n'ont pas besoin de créer un doublon ou de faire migrer manuellement les données vers un volume plus grand. Il n'y aura aucune indisponibilité ni refus d'accès au stockage lors du redimensionnement.

La facturation du volume est automatiquement mise à jour pour ajouter la différence au prorata du nouveau prix au cycle de facturation en cours. Le nouveau montant total est ensuite facturé dans le cycle de facturation suivant.

Cette fonctionnalité est disponible dans [la plupart des centre de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

## Avantages du stockage extensible

- **Gestion des coûts** : vous savez que vos données sont susceptibles de croître, mais vous avez besoin d'une quantité de stockage plus petite pour commencer. L'extensibilité permet à nos clients de réaliser des économies en matière de coûts de stockage, puis d'accroître la capacité de stockage en fonction de leurs besoins.  

- **Augmentation des besoins de stockage** - Les clients qui connaissent une croissance accélérée des données doivent trouver un moyen d'augmenter rapidement et facilement la taille de leur stockage pour le gérer au mieux.

## Effets de l'extension de la capacité de stockage sur la réplication

L'extension de l'espace de stockage principal entraîne un redimensionnement automatique de la réplique.

## Limitations
{: #limitsofexpandingstorage}

Cette fonctionnalité est disponible pour du stockage mis à disposition dans [la plupart des centre de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

Le stockage qui a été mis à disposition dans ces centres de données avant la mise sur le marché de cette fonction, d'**avril 2017 au 14 décembre 2017**, ne peut être augmenté que d'une taille équivalente à 10 fois sa taille d'origine. Le stockage mis à disposition après le **14 décembre 2017** peut être augmenté jusqu'à 12 To.

Les limitations de taille existantes pour le stockage {{site.data.keyword.blockstorageshort}} qui a été mis à disposition avec l'option Endurance sont toujours applicables (jusqu'à 4 To pour un niveau de 10 IOPS et jusqu'à 12 To pour tous les autres niveaux).

## Redimensionnement du stockage
{: #resizingsteps}

1. Depuis la console {{site.data.keyword.cloud}}, cliquez sur l'icône de **menu**. Cliquez ensuite sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
2. Sélectionnez le numéro d'unité logique dans la liste et cliquez sur **Actions** > **Modifier le numéro d'unité logique**.
3. Saisissez la nouvelle taille du stockage en Go.
4. Passez en revue votre sélection et la nouvelle tarification.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**.
6. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Vous pouvez également redimensionner votre volume via l'interface SLCLI.

```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
{:codeblock}

Pour plus d'informations sur l'extension du système de fichiers (et des partitions, le cas échéant) sur le volume afin d'utiliser le nouvel espace, consultez la documentation de votre système d'exploitation.
{:tip}
