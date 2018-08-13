---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Création d'un volume de blocs en double

Vous pouvez créer un doublon d'un {{site.data.keyword.blockstoragefull}} existant. Le volume en double hérite par défaut des options de capacité et de performance du numéro d'unité logique/volume d'origine et contient une copie des données jusqu'au moment de la prise d'un instantané.   

Etant donné que le volume dupliqué est basé sur les données d'un instantané de point de cohérence, vous devez disposer d'un espace d'image instantanée sur le volume d'origine avant de créer un doublon. Pour plus d'informations sur les instantanés et la commande d'espace d'instantané, voir la [documentation relative aux instantanés](snapshots.html).  

Vous pouvez créer des doublons à partir de volumes **principaux** et de **réplique**. Le nouveau doublon est créé dans le même centre de données que le volume d'origine. Si vous créez un doublon à partir d'un volume de réplique, le nouveau volume est créé dans le même centre de données que le volume de réplique.

Les volumes dupliqués sont accessibles par un hôte en lecture/écriture dès la mise à disposition du stockage. Toutefois, les instantanés et la réplication ne sont pas autorisés tant que la copie des données depuis le volume d'origine vers le doublon n'est pas terminée. 

Lorsque la copie de données est terminée, le doublon peut être géré et utilisé en tant que volume complètement indépendant. 

Cette fonctionnalité est disponible dans la plupart des emplacements. Cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html) pour obtenir la liste des centres de données disponibles.

Voici quelques exemples d'utilisation courante d'un volume dupliqué :
- **Test de reprise après incident** : créez un doublon de votre volume de réplique pour vérifier que les données sont intactes et qu'elles peuvent être utilisées dans le cas d'un sinistre sans interruption de la réplication. 
- **Copie finale** : utilisez un volume de stockage comme copie finale à partir de laquelle vous pouvez créer plusieurs instances en vue d'utilisations différentes. 
- **Actualisation des données** : créez une copie de vos données de production à monter sur votre environnement de non production en vue de les tester. 
- **Restauration à partir d'un instantané** : Restaurez les données sur le volume d'origine à l'aide de fichiers/données spécifiques provenant d'un instantané sans écraser la totalité du volume d'origine avec la fonction de restauration d'instantané. 
- **Développement/Test** : créez jusqu'à quatre doublons simultanés d'un volume en même temps pour créer des données dupliquées à des fins de développement et de test. 
- **Redimensionnement de stockage** : créez un volume avec une nouvelle taille et/ou un nouveau nombre d'IOPS sans avoir à effectuer une migration de vos données.  
	
Il existe deux manières de créer un volume dupliqué via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.


## Création d'un doublon à partir d'un volume spécifique dans la liste de stockage

1. Accédez à votre liste de {{site.data.keyword.blockstorageshort}}
    - A partir du portail client, cliquez sur **Storage** > **{{site.data.keyword.blockstorageshort}}** OU
    - A partir du catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}**. 
2. Sélectionnez un volume dans la liste et cliquez sur **Actions** > **Doublon du numéro d'unité logique/volume**. 
3. Choisissez une option d'instantané : 
    - Si vous effectuez votre commande à partir d'un volume **qui n'est pas un volume de réplique**, 
      - Sélectionnez **Créer à partir d'un nouvel instantané** pour créer un nouvel instantané qui sera utilisé pour le volume dupliqué. Utilisez cette option s'il n'existe aucun instantané pour votre volume ou si vous souhaitez créer un doublon à ce point de cohérence.<br/>
      - Sélectionnez **Créer à partir du dernier instantané** pour créer un doublon à partir du dernier instantané existant pour ce volume. 
    - Si vous effectuez votre commande à partir d'un volume de **réplique**, l'unique option d'instantané consiste à utiliser le dernier instantané disponible. 
4. Le type de stockage et l'emplacement restent identiques à ce qui est indiqué pour le volume d'origine.
5. Facturation à l'heure ou au mois - vous pouvez choisir de mettre à disposition le numéro d'unité logique du doublon avec une facturation à l'heure ou au mois. Le type de facturation pour le volume d'origine est automatiquement sélectionné. Si vous voulez en choisir un autre pour votre stockage en double, vous pouvez le sélectionner ici. 
5. Si vous le souhaitez, vous pouvez spécifier des E-S/s ou un niveau d'E-S/s pour le nouveau volume. Les E-S/s du volume d'origine sont sélectionnées par défaut. Les combinaisons de performances et de taille disponibles sont affichées.
    - Si le volume d'origine a un niveau Endurance avec 0,25 IOPS, vous ne pourrez pas effectuer de nouvelle sélection. 
    - Si votre volume d'origine est un niveau Endurance de 2, 4 ou 10 E-S/s, vous pouvez choisir l'un de ces niveaux pour le nouveau volume. 
6. Vous pouvez mettre à jour la taille du nouveau volume pour qu'elle soit supérieure à celle du volume d'origine. La taille du volume d'origine est définie par défaut. 
    - **Remarque** : Le redimensionnement de {{site.data.keyword.blockstorageshort}} est soumis à la limite de 10 fois la taille du volume d'origine. 
7. Vous pouvez mettre à jour l'espace d'instantané pour le nouveau volume en ajoutant plus, moins ou pas du tout d'espace d'instantané. L'espace d'instantané du volume d'origine est défini par défaut. 
8. Cliquez sur **Continuer** pour passer commande. 



## Création d'un doublon à partir d'un instantané spécifique

1. Accédez à votre liste de {{site.data.keyword.blockstorageshort}}
2. Cliquez sur un **numéro d'unité logique/volume** dans la liste pour afficher la page des détails. (Il peut s'agir d'un volume de réplique ou non). 
3. Faites défiler l'écran et sélectionnez un instantané existant sur la page des détails, puis cliquez sur **Actions** > **Dupliquer**.   
4. Le type de stockage (Endurance ou Performance) et l'emplacement restent identiques à ce qui est indiqué pour le volume d'origine. 
5. Les combinaisons de performances et de taille disponibles sont affichées. Les IOPS du volume d'origine sont définies par défaut. Vous pouvez spécifier des IOPS ou un niveau d'IOPS pour le nouveau volume. 
    - Si le volume d'origine a un niveau Endurance avec 0,25 IOPS, vous ne pourrez pas effectuer de nouvelle sélection. 
    - Si le volume d'origine a un niveau Endurance avec 2, 4 ou 10 IOPS, vous pouvez indiquer n'importe lequel de ces niveaux pour le nouveau volume. 
6. Vous pouvez mettre à jour la taille du nouveau volume pour qu'elle soit supérieure à celle du volume d'origine. La taille du volume d'origine est définie par défaut. 
    - **Remarque** : Le redimensionnement de {{site.data.keyword.blockstorageshort}} est soumis à la limite de 10 fois la taille du volume d'origine. 
7. Vous pouvez mettre à jour l'espace d'instantané pour le nouveau volume en ajoutant plus, moins ou pas du tout d'espace d'instantané. L'espace d'instantané du volume d'origine est défini par défaut. 
8. Cliquez sur **Continuer** pour passer votre commande du doublon. 


## Gestion de votre volume en double

Pendant que les données sont copiées depuis le volume d'origine vers le doublon, un statut s'affiche sur la page des détails indiquant que la duplication est en cours. Pendant cette opération, vous pouvez vous connecter à un hôte et lire/écrire sur le volume, mais vous ne pouvez pas créer de planifications de l'image instantanée. Une fois le processus de duplication terminé, le nouveau volume est indépendant du volume d'origine ; il peut être géré avec des instantanés et des réplications comme un volume normal. 
