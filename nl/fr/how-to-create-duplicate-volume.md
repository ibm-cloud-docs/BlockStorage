---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Création d'un volume de blocs en double

{{site.data.keyword.BluSoftlayer_full}} permet de dupliquer un service {{site.data.keyword.blockstoragefull}} existant. Le volume en double héritera par défaut des options de capacité et de performance du numéro d'unité logique/volume d'origine et contiendra une copie des données jusqu'au moment de la prise d'un instantané.    

Le doublon étant basé sur les données figurant dans un instantané ponctuel, un espace d'instantané est requis au préalable sur le volume d'origine pour que vous puissiez créer un doublon. Pour en savoir plus sur les instantanés et la commande d'espace d'instantané, consultez la [documentation relative aux instantanés](snapshots.html).   

Vous pouvez créer des doublons à partir de volumes principaux et de volumes de réplique, le nouveau doublon étant créé dans le même centre de données que le volume d'origine. Par exemple, si vous créez un doublon à partir d'un volume de réplique, le nouveau volume sera créé dans le même centre de données que le volume de réplique.     

Les volumes en double sont accessibles par un hôte en lecture/écriture dès que le stockage est mis à disposition. Les instantanés et la réplication ne sont pas autorisés tant que la copie de données de l'origine vers le doublon n'est pas terminée.  

Une fois la copie de données terminée, le doublon peut être géré et utilisé en tant que volume complètement indépendant de l'original.  

Cette fonction est uniquement disponible pour le stockage mis à disposition avec chiffrement. Cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html) pour obtenir la liste des centres de données disponibles.  

Voici quelques utilisations courantes d'un volume en double : 
- **Test de la reprise après incident** : Créez un doublon de votre volume de réplique pour vérifier que les données sont intactes et peuvent être utilisées en cas d'incident, sans interruption de la réplication.  
- **Copie finale** : Utilisez un volume de stockage comme copie finale à partir de laquelle vous pouvez créer plusieurs instances pour diverses utilisations.  
- **Actualisation des données** : Créez une copie de vos données de production en vue d'un montage vers votre environnement hors production à des fins de test.  
- **Restauration à partir d'un instantané** : Restaurez les données sur le volume d'origine à l'aide de fichiers/données spécifiques provenant d'un instantané sans écraser la totalité du volume d'origine avec une fonction de restauration d'instantané.  
- **Développement/test** : Générez jusqu'à 4 doublons simultanés d'un volume pour créer des volumes avec des données en double à des fins de développement et de test.  
- **Redimensionnement du stockage** : Créez un volume de la nouvelle taille et/ou des E-S/s sans avoir à effectuer une migration de vos données basée sur l'hôte.   
	

Il existe différentes façons de créer un volume en double via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} :  

## Création d'un doublon à partir d'un volume spécifique de la liste de stockage

Accédez à votre liste de services {{site.data.keyword.blockstorageshort}} : 

Dans le portail client, cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure->Stockage->{{site.data.keyword.blockstorageshort}}**.  


1. Sélectionnez un numéro d'unité logique dans la liste, puis cliquez sur **Actions** -> **Dupliquer le numéro d'unité logique (volume)**.  
2. Choisissez votre option d'instantané :  
    - En cas de commande à partir d'un volume autre qu'un volume de réplique : 
      - Sélectionnez l'option de création à partir d'un nouvel instantané pour créer un instantané qui sera utilisé pour le doublon. Servez-vous de cette option s'il n'existe pas d'instantanés pour votre volume ou si vous voulez créer un doublon à ce stade. 
    
      OU
      - Sélectionnez l'option de création à partir du dernier instantané pour créer un doublon à partir de l'instantané le plus récent qui existe pour ce volume.  
    - En cas de commande à partir d'un volume de réplique – la seule option pour l'instantané consiste à utiliser le dernier instantané disponible.  
3. Le type de stockage (Endurance ou Performance) et l'emplacement restent les mêmes que dans le volume d'origine. 
4. Facturation à l'heure ou au mois - vous pouvez choisir de mettre à disposition le nouveau numéro d'unité logique du doublon avec une facturation à l'heure ou au mois. Le type de facturation pour le volume d'origine est automatiquement sélectionné, mais si vous voulez en choisir un autre pour votre nouveau stockage en double, vous pouvez effectuer votre sélection ici.  
5. Si vous le souhaitez, vous pouvez spécifier des E-S/s ou un niveau d'E-S/s pour le nouveau volume. Les E-S/s du volume d'origine sont sélectionnées par défaut.  
    - Si votre volume d'origine est un niveau Endurance de 0,25 E-S/s, vous ne pourrez pas effectuer d'autre sélection.  
    - Si votre volume d'origine est un niveau Endurance de 2, 4 ou 10 E-S/s, vous pouvez choisir l'un de ces niveaux pour le nouveau volume.  
    - Les combinaisons de performances et de taille disponibles seront affichées.  
6. Si vous le souhaitez, vous pouvez mettre à jour la taille du nouveau volume pour qu'elle soit supérieure à celle du volume d'origine. La taille du volume d'origine est définie par défaut.  
    - **Remarque** : {{site.data.keyword.blockstorageshort}} ne peut être redimensionné que pour atteindre une taille équivalente à 10 fois la taille d'origine du volume.  
7. Si vous le souhaitez, vous pouvez mettre à jour l'espace d'instantané pour le nouveau volume en ajoutant plus, moins ou pas du tout d'espace d'instantané. L'espace d'instantané du volume d'origine sera défini par défaut.  
8. Cliquez sur **Continuer** pour passer commande du doublon.  



## Création d'un doublon à partir d'un instantané spécifique

Accédez à votre liste de services {{site.data.keyword.blockstorageshort}} : 

1. Cliquez sur un **numéro d'unité logique/volume** dans la liste pour afficher la page des détails. (Il peut s'agir d'un volume de réplique ou non).  
2. Faites défiler l'écran vers le bas et sélectionnez un instantané existant dans la liste située sur la page des détails, puis cliquez sur **Actions ->Doublon**.    
3. Le type de stockage (Endurance ou Performance) et l'emplacement restent les mêmes que dans le volume d'origine.  
4. Si vous le souhaitez, vous pouvez spécifier des E-S/s ou un niveau d'E-S/s pour le nouveau volume. Les E-S/s du volume d'origine sont sélectionnées par défaut.  
    - Si votre volume d'origine est un niveau Endurance de 0,25 E-S/s, vous ne pourrez pas effectuer d'autre sélection.  
    - Si votre volume d'origine est un niveau Endurance de 2, 4 ou 10 E-S/s, vous pouvez choisir l'un de ces niveaux pour le nouveau volume.  
    - Les combinaisons de performances et de taille disponibles seront affichées.  
5. Si vous le souhaitez, vous pouvez mettre à jour la taille du nouveau volume pour qu'elle soit supérieure à celle du volume d'origine. La taille du volume d'origine est définie par défaut.  
    - **Remarque** : {{site.data.keyword.blockstorageshort}} ne peut être redimensionné que pour atteindre une taille équivalente à 10 fois la taille d'origine du volume.  
6. Si vous le souhaitez, vous pouvez mettre à jour l'espace d'instantané pour le nouveau volume en ajoutant plus, moins ou pas du tout d'espace d'instantané. L'espace d'instantané du volume d'origine sera défini par défaut.  
7. Cliquez sur **Continuer** pour passer commande du doublon.  


## Gestion du volume en double

Lorsque les données sont copiées du volume d'origine vers le doublon, un statut indiquant que la duplication est en cours s'affiche sur la page des détails. Pendant cette opération, vous pouvez vous connecter à un hôte et lire/écrire sur le volume, mais vous ne pouvez pas créer de planifications de l'image instantanée. Une fois le processus de duplication terminé, le nouveau volume est complètement indépendant du volume d'origine et peut être géré normalement avec des instantanés et une réplication, etc.  
