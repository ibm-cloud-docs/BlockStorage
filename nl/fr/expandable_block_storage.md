---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-20"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Extension de la capacité de stockage par blocs

Cette nouvelle fonctionnalité permet aux utilisateurs d'{{site.data.keyword.blockstoragefull}} d'étendre immédiatement la taille de leur stockage {{site.data.keyword.blockstorageshort}} en incréments de Go jusqu'à 12 To. Ils n'ont pas besoin de créer un doublon ou de faire migrer manuellement les données vers un volume plus grand. Il n'y aura aucune indisponibilité ni refus d'accès au stockage lors du redimensionnement.

La facturation du volume est automatiquement mise à jour pour ajouter la différence au prorata du nouveau prix au cycle de facturation en cours. Le nouveau montant total est ensuite facturé dans le cycle de facturation suivant.

Cette fonctionnalité est disponible uniquement dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html).

## Avantages du stockage extensible

- **Gestion des coûts** : vous savez que vos données sont susceptibles de croître, mais vous avez besoin d'une quantité de stockage plus petite pour commencer. L'extensibilité permet à nos clients de réaliser des économies en matière de coûts de stockage, puis d'accroître la capacité de stockage en fonction de leurs besoins.  

- **Augmentation des besoins de stockage** - Les clients qui connaissent une croissance accélérée des données doivent trouver un moyen d'augmenter rapidement et facilement la taille de leur stockage pour le gérer au mieux.

## Effets de l'extension de la capacité de stockage sur la réplication

L'extension de l'espace de stockage principal entraîne un redimensionnement automatique de la réplique.

## Limitations

Cette fonction est disponible pour le stockage mis à disposition dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html).

Le stockage qui a été mis à disposition dans ces centres de données avant la mise sur le marché de cette fonction, d'**avril 2017 au 14 décembre 2017**, ne peut être augmenté que d'une taille équivalente à 10 fois sa taille d'origine. Le stockage mis à disposition après le **14 décembre 2017** peut être augmenté jusqu'à 12 To.

Les limitations de taille existantes pour le stockage {{site.data.keyword.blockstorageshort}} qui a été mis à disposition avec l'option Endurance sont toujours applicables (jusqu'à 4 To pour un niveau de 10 IOPS et jusqu'à 12 To pour tous les autres niveaux).

## Redimensionnement du stockage

1. Dans le portail {{site.data.keyword.slportal}}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU à partir du catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
2. Sélectionnez le numéro d'unité logique dans la liste et cliquez sur **Actions** > **Modifier le numéro d'unité logique**.
3. Saisissez la nouvelle taille du stockage en Go.
4. Passez en revue votre sélection et la nouvelle tarification.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**.
6. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Pour plus d'informations sur l'extension du système de fichiers (et des partitions, le cas échéant) sur le volume afin d'utiliser le nouvel espace, consultez la documentation de votre système d'exploitation.
{:tip}
