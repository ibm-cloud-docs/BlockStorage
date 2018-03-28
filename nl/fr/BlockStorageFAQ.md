---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# {{site.data.keyword.blockstorageshort}} : Foire aux questions

## Combien d'instances peuvent partager l'utilisation d'un volume {{site.data.keyword.blockstorageshort}} mis à disposition ?
La limite par défaut pour le nombre d'autorisations par volume de blocs est 8. Veuillez contacter votre ingénieur commercial pour augmenter cette limite. 

## Lors de la mise à disposition de Performance ou Endurance {{site.data.keyword.blockstorageshort}}, les opérations d'entrée-sortie par seconde (E-S/s) sont-elles allouées par instance ou par volume ?
Les E-S/s sont allouées au niveau du volume. En d'autres termes, deux hôtes connectés à un volume avec 6 000 E-S/s partagent ces 6 000 E-S/s.

## Comment sont mesurées les E-S/s ?
Les E-S/s sont mesurées en fonction d'un profil de chargement par blocs de 16 ko avec 50 % de lectures et 50 % d'écritures aléatoires. Les charges de travail qui diffèrent de ce profil peuvent connaître une baisse des performances.

## Que se passe-t-il si j'utilise une taille de bloc inférieure lors de la mesure des performances ?
Le nombre maximal d'E-S/s peut toujours être obtenu en utilisant des tailles de bloc inférieures, mais le débit sera moindre. Par exemple, un volume avec 6 000 E-S/s aura le débit suivant aux diverses tailles de bloc :

- 16 ko * 6 000 E-S/s == ~93,75 Mo/sec 
-  8 ko * 6 000 E-S/s == ~46,88 Mo/sec
-  4 ko * 6 000 E-S/s == ~23,44 Mo/sec

## Le volume doit-il être préchauffé pour obtenir le débit prévu ?
Aucun préchauffage n'est nécessaire. Vous verrez le débit spécifié dès la mise à disposition du volume. 

## Pourquoi puis-je mettre à disposition {{site.data.keyword.blockstorageshort}} avec un niveau d'endurance de 10 E-S/s dans certains centres de données et pas dans d'autres ?
Le niveau de 10 E-S/s/Go du type de stockage {{site.data.keyword.blockstorageshort}} Endurance est uniquement disponible dans certains centres de données, mais la liste de ces centres va bientôt être enrichie. Vous trouverez la liste complète des centres de données mis à niveau et des fonctions disponibles ici. 

## Comment puis-je savoir quels numéros d'unité logiques/volumes {{site.data.keyword.blockstorageshort}} sont chiffrés ?
Lors de l'affichage de votre liste de services {{site.data.keyword.blockstorageshort}} dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, une icône de verrouillage s'affiche à droite du nom du numéro d'unité logique/volume lorsque ce dernier est chiffré. 

## Comment savoir si je mets à disposition {{site.data.keyword.blockstorageshort}} dans un centre de données mis à niveau ?
Lors de la mise à disposition du service {{site.data.keyword.blockstorageshort}}, tous les centres de données mis à niveau sont signalés par un astérisque (`*`) dans le formulaire de commande, ainsi que par un message indiquant que vous allez mettre à disposition un stockage avec chiffrement. Une fois que le stockage est mis à disposition, une icône s'affiche dans la liste de stockage pour indiquer que le volume est chiffré. Tous les volumes, chiffrés ou non, sont mis à disposition uniquement dans des centres de données mis à niveau. Vous trouverez la liste complète des centres de données mis à niveau et des fonctions disponibles ici. 

## Pourquoi puis-je mettre à disposition {{site.data.keyword.blockstorageshort}} avec un niveau d'endurance de 10 E-S/s dans certains centres de données et pas dans d'autres ?
Le niveau de 10 E-S/s/Go du type Endurance est uniquement disponible dans certains centres de données, mais la liste de ces centres va bientôt être enrichie. Vous trouverez la liste complète des centres de données mis à niveau et des fonctions disponibles [ici](new-ibm-block-and-file-storage-location-and-features.html). 

## Si un service {{site.data.keyword.blockstorageshort}} non chiffré est mis à disposition dans un centre de données qui a été mis à niveau pour le chiffrement, puis-je chiffrer {{site.data.keyword.blockstorageshort}} ?
Un service {{site.data.keyword.blockstorageshort}} mis à disposition avant la mise à niveau d'un centre de données ne peut pas être chiffré.
Un nouveau service {{site.data.keyword.blockstorageshort}} mis à disposition dans des centres de données mis à niveau est automatiquement chiffré ; aucun paramètre de chiffrement ne doit être sélectionné, car l'opération est automatique.
Les données situées sur un stockage non chiffré dans un centre de données mis à niveau peuvent être chiffrées en créant un numéro d'unité logique de bloc, puis en copiant les données sur le nouveau numéro d'unité logique chiffré à l'aide d'une migration basée sur l'hôte. Consultez cet [article](migrate-block-storage-encrypted-block-storage) pour obtenir des instructions sur la procédure de migration. 

## Combien de volumes puis-je mettre à disposition ?
Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Veuillez contacter votre ingénieur commercial pour augmenter vos volumes.

## Obtiendrai-je un débit plus important si j'utilise une connexion Ethernet plus rapide ?
Les limites en matière de débit étant définies par volume/numéro d'unité logique, l'utilisation d'une connexion Ethernet plus rapide n'augmentera pas cette limite. Toutefois, avec une connexion Ethernet plus lente, votre bande passante peut éventuellement créer un goulot d'étranglement. 

## Les pare-feux/groupes de sécurité auront-ils un impact sur les performances ?
Nous vous recommandons d'exécuter le trafic de stockage sur un réseau local virtuel qui ignore le pare-feu comme meilleure pratique. L'exécution du trafic de stockage via des pare-feux logiciels augmentera le temps d'attente et affectera de façon négative les performances de stockage. 

## {{site.data.keyword.blockstorageshort}} prend-il en charge la réservation persistante SCSI-3 pour implémenter la protection d'E-S pour Db2 pureScale ?
Oui, {{site.data.keyword.blockstorageshort}} prend en charge les réservations persistantes SCSI-2 et SCSI-3.

## Que deviennent mes données lors de la suppression des numéros d'unité logique {{site.data.keyword.blockstorageshort}} ?

Lors de la suppression du stockage, les pointeurs vers les données situées sur ce volume sont également supprimés, rendant ainsi les données complètement inaccessibles. Si le stockage physique est remis à disposition sur un autre compte, un nouvel ensemble de pointeurs est affecté. Il est impossible pour le nouveau compte d'accéder à des données qui ont pu se trouver sur le stockage physique et le nouvel ensemble de pointeurs n'affiche que des zéros. Lorsque de nouvelles données sont écrites sur le volume/numéro d'unité logique, les données inaccessibles qui existent encore sont écrasées.  

## Quel temps d'attente dois-je prévoir pour les performances dans {{site.data.keyword.blockstorageshort}} ?   

Le temps d'attente cible dans le stockage est < 1 ms. Notre stockage étant connecté à des instances de calcul sur un réseau partagé, le temps d'attente exact pour les performances dépendra du trafic réseau sur une période donnée. 
