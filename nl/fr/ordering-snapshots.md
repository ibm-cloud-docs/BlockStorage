---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Commande d'instantanés

Pour créer des instantanés de votre volume de stockage, de façon automatisée ou manuelle, vous devez acheter de l'espace destiné à les accueillir. Vous pouvez acheter de la capacité pour atteindre la quantité de votre volume de stockage (lors de l'achat initial de volume ou ultérieurement en suivant les étapes décrites dans cet article).

1. Accédez à votre LUN de stockage via **Stockage**, onglet **{{site.data.keyword.blockstorageshort}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Cliquez sur **Ajouter de l'espace d'instantané** dans le cadre Instantanés.
3. Sélectionnez la quantité d'espace dont vous avez besoin.
4. Cliquez sur **Continuer**.
5. Saisissez éventuellement un **code promotionnel** et cliquez sur **Recalculer**. Les zones Prix pour cette commande et Vérification de la commande sont renseignées par défaut.
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. Votre espace d'instantané sera mis à disposition dans quelques minutes.

## Calcul de la quantité d'espace d'instantané à commander

En général, l'espace d'instantané est utilisé par les instantanés en fonction de deux critères :
- le nombre de modifications de votre système de fichiers actif,
- la durée pendant laquelle vous prévoyez de conserver les instantanés.  

La méthode de calcul de la quantité d'espace nécessaire est essentiellement **(Taux de modification)** x **(nombre d'heures/jours/semaines/mois de conservation des données)**.  
**Remarque** : Le premier instantané utilise une très faible quantité d'espace lorsqu'il s'agit simplement d'une copie des métadonnées (pointeurs) indiquant les blocs du système de fichiers actif. 

Un volume avec beaucoup de modifications de données (comme une base de données avec un fort taux de modification) et une longue durée de conservation des instantanés nécessiteront plus d'espace pour les instantanés qu'un volume avec un nombre modéré de modifications (comme un magasin de données de machine virtuelle) et une période de conservation des instantanés relativement brève. 

Si vous prévoyez de prendre 12 instantanés par heure d'un volume comportant 500 Go de données réelles et que vous avez observé 1 % de modifications entre chaque instantané, vous obtiendrez 60 Go pour les instantanés.

*(Taux de modification de 5 Go) x (12 instantanés par heure) = (60 Go d'espace utilisé)*

A l'inverse, si pour ces 500 Go de données réelles, avec 12 instantanés par heure, vous avez observé 10 % de modifications par heure, vous obtiendrez 600 Go.

*(Taux de modification de 50 Go) x (12 instantanés par heure) = (600 Go d'espace utilisé)*

Par conséquent, lorsque vous déterminez la quantité d'espace d'instantané dont vous avez besoin, faites attention au taux de modification car il influe considérablement sur l'espace d'instantané nécessaire. Alors que la taille d'un volume correspondra vraisemblablement à un plus grand nombre de modifications, un volume de 500 Go avec 5 Go de modifications et un volume de 10 To avec 5 Go de modifications généreront la même utilisation de l'espace d'instantané.

En outre, pour la plupart des charges de travail, plus un volume est important, plus l'espace à conserver initialement pour les instantanés est réduit. Cela est principalement dû à l'efficacité des données sous-jacentes de notre plateforme, ainsi qu'à la nature du fonctionnement des instantanés dans notre environnement.



