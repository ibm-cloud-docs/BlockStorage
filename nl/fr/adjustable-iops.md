---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ajustement des opérations d'entrée-sortie par seconde

Grâce à cette nouvelle fonction, les utilisateurs de notre stockage {{site.data.keyword.blockstoragefull}} peuvent ajuster immédiatement les opérations d'entrée-sortie par seconde (E-S/s) de leur service {{site.data.keyword.blockstorageshort}} existant, sans avoir besoin de créer un doublon ni de migrer manuellement les données vers un nouveau stockage. Les utilisateurs ne connaîtront pas d'indisponibilité ou de refus d'accès au stockage d'aucune sorte lors de l'ajustement. 

La facturation du stockage sera mise à jour pour ajouter la différence au prorata du nouveau prix au cycle de facturation en cours. Le nouveau montant total est facturé dans le cycle de facturation suivant. 

Cette fonction est disponible uniquement dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html). 

## Avantages des opérations d'entrée-sortie par seconde ajustables

- Gestion des coûts – Certains de nos clients peuvent avoir besoin d'un nombre élevé d'opérations d'entrée-sortie par seconde lors de pics d'utilisation. Par exemple, un grand magasin de détail connaît un pic d'utilisation pendant les vacances et risque donc d'avoir besoin d'un plus grand nombre d'opérations d'entrée-sortie par seconde sur son stockage qu'au milieu de l'été. Cette fonction permet au magasin de gérer ses coûts et de payer davantage d'opérations d'entrée-sortie par seconde uniquement lorsqu'il en a réellement besoin.

## Existe-t-il des limitations ?

Cette fonction est uniquement disponible pour le stockage mis à disposition dans des [centres de données](new-ibm-block-and-file-storage-location-and-features.html) dotés de fonctionnalités avancées. 

Les clients ne peuvent pas basculer entre l'endurance et les performances lorsqu'ils ajustent leurs opérations d'entrée-sortie par seconde. Les utilisateurs peuvent spécifier un nouveau niveau d'E-S/s pour leur stockage en fonction des restrictions/critères suivants : 

- Si le niveau d'endurance du volume d'origine est de 0,25, le niveau d'E-S/s ne peut pas être mis à jour.
- Si les performances du volume d'origine sont < à 0,30 E-S/s/Go, les options disponibles ne doivent inclure que des combinaisons de taille et d'E-S/s < à 0,30 E-S/s/Go. 
- Si les performances du volume d'origine sont >= à 0,30 E-S/s/Go, les options disponibles ne doivent inclure que des combinaisons de taille et d'E-S/s >= à 0,30 E-S/s/Go (taille supérieure ou égale au volume d'origine).



## Dans quelle mesure l'ajustement des opérations d'E-S/s affecte-t-elle la réplication ?

Si la réplication est en place sur le volume, la réplique sera automatiquement mise à jour pour correspondre à la sélection d'E-S/s du volume principal. 

## Comment ajuster les opérations d'E-S/s sur mon stockage ?

1. Dans le portail {{site.data.keyword.slportal}}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU à partir du catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
2. Sélectionnez le numéro d'unité logique dans la liste et cliquez sur **Actions** > **Modifier le numéro d'unité logique**.
3. Sous les options d'E-S/s de stockage, effectuez une nouvelle sélection :
    - Endurance (E-S/s échelonnées) : sélectionnez un niveau d'E-S/s supérieur à 0,25 E-S/s/Go pour votre stockage. Vous pouvez augmenter ce niveau à tout moment. Toutefois, il ne peut être diminué qu'une fois par mois.
    - Performances (E-S/s allouées) : indiquez une nouvelle option d'E-S/s pour votre stockage en saisissant une valeur comprise entre 100 et 48 000 E-S/s. (N'oubliez pas de prendre en compte les limites spécifiques requises par la taille dans le formulaire de commande.)
4. Passez en revue votre sélection et la nouvelle tarification.
5. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**.
6. Votre nouvelle allocation de stockage devrait être disponible dans quelques minutes.
