---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacité de stockage par blocs extensible

Grâce à cette nouvelle fonction, nos utilisateurs {{site.data.keyword.blockstoragefull}} actuels peuvent augmenter instantanément la taille de leur service {{site.data.keyword.blockstorageshort}} existant par incréments en Go jusqu'à 12 To, sans avoir besoin de créer un doublon ni de migrer manuellement les données vers un volume plus grand. Il n'y aura aucune indisponibilité ni refus d'accès au stockage lors du redimensionnement.  

La facturation du volume est automatiquement mise à jour pour ajouter la différence au prorata du nouveau prix au cycle de facturation en cours. Le nouveau montant total sera ensuite facturé dans le cycle de facturation suivant. 

Cette fonction est disponible uniquement dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html).  

## Avantages du stockage extensible

- **Gestion des coûts** – Vous savez peut-être que vos données risquent d'augmenter, mais vous avez besoin d'une quantité de stockage réduite pour commencer. L'extensibilité permet à nos clients de réaliser des économies en matière de coûts de stockage, puis d'accroître la capacité de stockage en fonction de leurs besoins.   

- **Augmentation des besoins de stockage** - Les clients qui connaissent une croissance accélérée doivent trouver un moyen d'augmenter rapidement et facilement la taille de leur stockage pour gérer au mieux cet accroissement. 

## Dans quelle mesure l'augmentation de la capacité de stockage affecte-t-elle la réplication ?

Une extension de l'espace de stockage principal entraînera un redimensionnement automatique de la réplique.  

## Existe-t-il des limitations ?

Cette fonction est uniquement disponible pour le stockage mis à disposition dans des [centres de données](new-ibm-block-and-file-storage-location-and-features.html) dotés de fonctionnalités avancées. 

Le stockage mis à disposition sur un espace de stockage mis à jour dans ces centres de données avant la mise sur le marché de cette fonction (le 14 décembre 2017) ne peut être augmenté que d'une taille équivalente à 10 fois sa taille d'origine. Tous les autres espaces de stockage mis à disposition après cette date peuvent être augmentés jusqu'à 12 To au maximum.  

Les limitations de taille existantes pour un service {{site.data.keyword.blockstorageshort}} mis à disposition avec Endurance s'appliquent encore (jusqu'à 4 To pour le niveau de 10 E-S/s et 12 To pour tous les autres). 

## Comment savoir si mon stockage mis à disposition est extensible ?

Le stockage mis à disposition avec des fonctions améliorées est toujours chiffré au repos. Vous pouvez facilement déterminer que votre stockage est éligible si l'icône de verrouillage apparaît en regard de ce dernier dans l'interface utilisateur du portail.  

## Comment puis-je redimensionner mon stockage ? 

1. Dans le portail {{site.data.keyword.slportal}}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU à partir du catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
2. Sélectionnez le numéro d'unité logique dans la liste et cliquez sur **Actions** > **Modifier le numéro d'unité logique**. 
3. Saisissez la nouvelle taille du stockage en Go. 
4. Passez en revue votre sélection et la nouvelle tarification. 
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. 
6. Votre nouvelle allocation de stockage devrait être disponible dans quelques minutes.
  
