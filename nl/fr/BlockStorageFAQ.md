---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-18"

---
{:new_window: target="_blank"}

# FAQ (Foire aux questions)

## Combien d'instances peuvent partager l'utilisation d'un volume {{site.data.keyword.blockstorageshort}} ?
Le nombre d'autorisations par volume de blocs est limité par défaut à 8. Cela signifie que jusqu'à 8 hôtes peuvent être autorisés à accéder au numéro d'unité logique Block Storage. Pour augmenter la limite, contactez votre commercial. 

## Combien de volumes peuvent être commandés ?
Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter votre limite, contactez votre commercial. Pour plus d'informations, voir [Gestion des limites de stockage](managing-storage-limits.html).

## Combien de volumes {{site.data.keyword.blockstorageshort}} peuvent être montés sur un hôte ?
Cela dépend de ce que peut gérer le système d'exploitation de l'hôte. Ce n'est pas {{site.data.keyword.BluSoftlayer_full}} qui fixe une limite. Consultez la documentation de votre système d'exploitation pour connaître les limites fixées pour le nombre de volumes pouvant être montés.

## La limite du nombre d'IOPS est-elle imposée par instance ou par volume ?
Les IOPS sont imposées au niveau du volume. En d'autres termes, deux hôtes connectés à un volume avec 6 000 IOPS partagent ces 6 000 IOPS.

## Mesure des IOPS
Les E-S/s sont mesurées en fonction d'un profil de chargement par blocs de 16 ko avec 50 % de lectures et 50 % d'écritures aléatoires. Les charges de travail qui diffèrent de ce profil sont susceptibles de connaître une baisse des performances.

## Que se passe-t-il lorsqu'une taille de bloc inférieure est utilisée pour mesurer les performances ?
Le nombre maximal d'IOPS peut être obtenu même si vous utilisez des tailles de bloc plus petites. Toutefois, le débit devient plus lent. Par exemple, un volume doté de 6 000 IOPS présente les débits suivants en fonction des tailles de bloc :

- 16 ko * 6 000 E-S/s == ~93,75 Mo/sec 
- 8 ko * 6 000 E-S/s == ~46,88 Mo/sec
- 4 ko * 6 000 E-S/s == ~23,44 Mo/sec

## Le volume doit-il être préchauffé pour obtenir le débit prévu ?
Aucun préchauffage n'est nécessaire. Le débit indiqué peut être observé immédiatement après la mise à disposition du volume.

## Est-il possible d'atteindre un débit plus élevé en utilisant une connexion Ethernet plus rapide ?
Les limites de débit sont configurées par volume ou par numéro d'unité logique. Par conséquent, une connexion Ethernet plus rapide ne permet pas d'augmenter la limite définie. Toutefois, avec une connexion Ethernet plus lente, votre bande passante peut éventuellement créer un goulot d'étranglement.

## Les pare-feu et groupes de sécurité ont-ils un impact sur les performances ?
Il est recommandé d'exécuter le trafic de stockage sur un réseau local virtuel qui ignore le pare-feu. L'exécution du trafic de stockage via des pare-feu logiciels augmente le temps d'attente et a un impact négatif sur les performances de stockage.

## Quel temps d'attente lié aux performances peut-on attendre du stockage {{site.data.keyword.blockstorageshort}} ?   
Le temps d'attente cible dans le stockage est < 1 ms. Le stockage est connecté à des instances de traitement sur un réseau partagé ; le temps d'attente exact des performances dépend donc du trafic réseau sur une période donnée.

## Pourquoi {{site.data.keyword.blockstorageshort}} avec un niveau Endurance de 10 IOPS peut-il être commandé dans certains centres de données et pas dans d'autres ?
Le niveau 10 IOPS/Go du type de stockage {{site.data.keyword.blockstorageshort}} Endurance est uniquement disponible dans certains centres de données, mais la liste de ces centres va bientôt être enrichie. Vous trouverez la liste complète des centres de données mis à niveau et des fonctions disponibles [ici](new-ibm-block-and-file-storage-location-and-features.html).

## Comment savoir quels numéros d'unité logiques/volumes {{site.data.keyword.blockstorageshort}} sont chiffrés ?
Lorsque vous consultez votre liste de services {{site.data.keyword.blockstorageshort}} dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, une icône de verrouillage s'affiche à droite du nom du numéro d'unité logique/volume lorsque ce dernier est chiffré.

## Comment savoir si l'ont met à disposition un stockage {{site.data.keyword.blockstorageshort}} dans un centre de données mis à niveau ?
Lorsque vous commandez {{site.data.keyword.blockstorageshort}}, tous les centres de données mis à niveau sont signalés par un astérisque (`*`) dans le formulaire de commande, ainsi que par un message indiquant que vous êtes sur le point de mettre à disposition un stockage avec chiffrement. Une fois le stockage mis à disposition, une icône apparaît dans la liste de stockage pour indiquer que le stockage est chiffré. Tous les volumes et numéros d'unité logique chiffrés sont mis à disposition uniquement dans des centres de données mis à niveau. Vous trouverez la liste complète des centres de données mis à niveau et des fonctions disponibles [ici](new-ibm-block-and-file-storage-location-and-features.html).

## Si nous possédons un stockage {{site.data.keyword.blockstorageshort}} non chiffré dans un centre de données qui a été récemment mis à jour, pouvons-nous chiffrer ce stockage {{site.data.keyword.blockstorageshort}} ?
Un service {{site.data.keyword.blockstorageshort}} qui est mis à disposition avant la mise à niveau du centre de données ne peut pas être chiffré. 
Un nouveau service {{site.data.keyword.blockstorageshort}} mis à disposition dans des centres de données mis à niveau est automatiquement chiffré. Vous n'avez pas à choisir de paramètre de chiffrement, car la procédure est automatique. 
Les données situées sur un stockage non chiffré dans un centre de données mis à niveau peuvent être chiffrées en créant un numéro d'unité logique de bloc, puis en copiant les données sur le nouveau numéro d'unité logique chiffré à l'aide d'une migration basée sur l'hôte. Cliquez [ici](migrate-block-storage-encrypted-block-storage.html) pour obtenir des instructions.

## {{site.data.keyword.blockstorageshort}} prend-il en charge la réservation persistante SCSI-3 pour implémenter la protection d'E-S pour Db2 pureScale ?
Oui, {{site.data.keyword.blockstorageshort}} prend en charge les réservations persistantes SCSI-2 et SCSI-3.

## Qu'advient-il des données lors de la suppression des numéros d'unité logique {{site.data.keyword.blockstorageshort}} ?
{{site.data.keyword.blockstoragefull}} propose aux clients les volumes de blocs sur un espace de stockage physique qui est nettoyé avant toute réutilisation. Les clients ayant des exigences particulières de conformité (telles que celles définies dans le document 800-88 du National Institute of Standards and Technology intitulé Guidelines for Media Sanitization) doivent effectuer la procédure d'expurgation des données avant de supprimer leur stockage.

## Que deviennent les unités qui sont déclassées du centre de données cloud ?
Lorsque des unités sont déclassées, IBM les détruit avant de les supprimer. Elles sont ainsi inutilisables. Toutes les données qui étaient écrites sur ces unités deviennent inaccessibles.
