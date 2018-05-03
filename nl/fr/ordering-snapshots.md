---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Commande d'instantanés

Pour créer des instantanés de votre volume de stockage, de façon automatisée ou manuelle, vous devez acheter de l'espace destiné à les accueillir. Vous pouvez acheter de la capacité pour atteindre la quantité de votre stockage de volume (lors de l'achat initial de volume ou ultérieurement en suivant la procédure ci-dessous).

1. Accédez à votre LUN de stockage via **Stockage**, onglet **{{site.data.keyword.blockstorageshort}}** du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Cliquez sur le lien **Ajouter de l'espace d'instantané** dans le cadre Instantanés.
3. Sélectionnez la quantité d'espace dont vous avez besoin en cliquant sur le bouton d'option en regard de la quantité appropriée.
4. Cliquez sur **Continuer**.
5. Saisissez éventuellement un code promotionnel et cliquez sur **Recalculer**. Les zones Prix pour cette commande et Vérification de la commande contiennent les valeurs par défaut.
6. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. Votre espace d'instantané sera mis à disposition dans quelques minutes.

## Calcul de la quantité d'espace d'instantané à commander

Malheureusement, il n'existe pas de meilleure pratique que la recommandation visant à tenir compte de l'application. En général, l'espace d'instantané est consommé par les instantanés en fonction de deux critères :
- le nombre de modifications de votre système de fichiers actif, et 
- la durée pendant laquelle vous prévoyez de conserver les instantanés.  

La méthode de calcul de la quantité d'espace nécessaire est essentiellement **(Taux de modification)** x **(nombre d'heures/jours/semaines/mois de conservation)**.  
**Remarque** : Le premier instantané consomme une très faible quantité d'espace lorsqu'il s'agit simplement d'une copie des métadonnées (pointeurs) indiquant les blocs du système de fichiers actif. 

Un volume avec beaucoup de modifications de données (par exemple, une base de données avec un fort taux de modification) et une longue durée de conservation des instantanés nécessiteront plus d'espace pour les instantanés qu'un volume avec un nombre modéré de modifications (par exemple, un magasin de données de machine virtuelle) et une période de conservation des instantanés relativement brève. 

Dans l'exemple d'un volume comportant 500 Go de données réelles, si vous prévoyez de prendre 12 instantanés par heure et que vous avez observé 1 % de modifications entre chacun d'eux, vous obtiendrez (Taux de modification de 5 Go) x (12 instantanés par heure) = 60 Go pour les instantanés.

A l'inverse, si pour ces 500 Go de données réelles, avec 12 instantanés par heure, vous avez observé 10 % de modifications par heure, vous obtiendrez (Taux de modification de 50 Go) × (12 instantanés par heure) = 600 Go.

Par conséquent, lorsque vous déterminez la quantité d'espace d'instantané dont vous avez besoin, faites attention au taux de modification car il influe considérablement sur l'espace d'instantané nécessaire.  Alors que la taille d'un volume correspondra vraisemblablement à un plus grand nombre de modifications, un volume de modifications de 500 Go et un volume de 10 To avec 5 Go de modifications générera la même utilisation de l'espace d'instantané.

En outre, pour la plupart des charges de travail, plus un volume est important, plus l'espace à conserver initialement pour les instantanés est réduit.  Cela est principalement dû à l'efficacité des données sous-jacentes de notre plateforme, ainsi qu'à la nature du fonctionnement des instantanés dans notre environnement.



