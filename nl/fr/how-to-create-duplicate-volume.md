---

copyright:
  years: 2017, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Création d'un volume de blocs en double
{: #duplicatevolume}

Vous pouvez créer un doublon d'un {{site.data.keyword.blockstoragefull}} existant. Le volume dupliqué hérite par défaut des options de capacité et de performance du volume d'origine et contient une copie des données jusqu'au point de cohérence d'un instantané.   

Etant donné que le volume dupliqué est basé sur les données d'un instantané de point de cohérence, vous devez disposer d'un espace d'instantané sur le volume d'origine avant de créer un doublon. Pour plus d'informations sur les instantanés et la commande d'espace d'instantané, voir la [documentation relative aux instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots).  

Vous pouvez créer des doublons à partir d'un volume **principal** et d'un volume de **réplique**. Le nouveau doublon est créé dans le même centre de données que le volume d'origine. Si vous créez un doublon à partir d'un volume de réplique, le nouveau volume est créé dans le même centre de données que le volume de réplique.

Les volumes dupliqués sont accessibles par un hôte en lecture/écriture dès la mise à disposition du stockage. Toutefois, les instantanés et la réplication ne sont pas autorisés tant que la copie des données depuis le volume d'origine vers le doublon n'est pas terminée.

Une fois la copie de données terminée, le doublon peut être géré et utilisé en tant que volume indépendant.

Cette fonctionnalité est disponible dans la plupart des emplacements. Pour plus d'informations, voir [la liste des centres de données disponibles](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

Si vous êtes un utilisateur de compte Dedicated d'{{site.data.keyword.containerlong}}, consultez vos options de duplication d'un volume dans la [{{site.data.keyword.containerlong_notm}}documentation ](/docs/containers?topic=containers-block_storage#block_backup_restore).
{:tip}

Voici quelques exemples d'utilisation courante d'un volume dupliqué :
- **Test de reprise après incident**. Créez un doublon de votre volume de réplique pour vérifier que les données sont intactes et qu'elles peuvent être utilisées dans le cas d'un sinistre sans interruption de la réplication.
- **Copie finale**. Utilisez un volume de stockage comme copie finale à partir de laquelle vous pouvez créer plusieurs instances en vue d'utilisations différentes.
- **Actualisation des données**. Créez une copie de vos données de production à monter sur votre environnement de non production en vue de les tester.
- **Restauration à partir d'un instantané**. Restaurez les données sur le volume d'origine à l'aide de fichiers et de données spécifiques provenant d'un instantané sans écraser la totalité du volume d'origine avec la fonction de restauration d'instantané.
- **Développement/Test**. Créez jusqu'à quatre doublons simultanés d'un volume en même temps pour créer des données dupliquées à des fins de développement et de test.
- **Redimensionnement de stockage**. Créez un volume avec une nouvelle taille et/ou un nouveau nombre d'IOPS sans avoir à effectuer une migration de vos données.  

Il existe deux manières de créer un volume dupliqué via la [console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}.


## Création d'un doublon à partir d'un volume spécifique dans la liste de stockage

1. Accédez à votre liste de {{site.data.keyword.blockstorageshort}} sur la console {{site.data.keyword.cloud_notm}} en cliquant sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}**.
2. Sélectionnez un volume dans la liste et cliquez sur **Actions** > **Doublon du numéro d'unité logique/volume**.
3. Choisissez une option d'instantané :
    - Si vous effectuez votre commande à partir d'un volume **qui n'est pas un volume de réplique**,
      - Sélectionnez **Créer à partir d'un nouvel instantané** pour créer un nouvel instantané qui sera utilisé pour le volume dupliqué. Utilisez cette option s'il n'existe aucun instantané pour votre volume ou si vous souhaitez créer un doublon à ce point de cohérence.<br/>
      - Sélectionnez **Créer à partir du dernier instantané** pour créer un doublon à partir du dernier instantané existant pour ce volume.
    - Si vous effectuez votre commande à partir d'un volume de **réplique**, l'unique option d'instantané consiste à utiliser le dernier instantané disponible.
4. Le type de stockage et l'emplacement restent identiques à ce qui est indiqué pour le volume d'origine.
5. Facturation à l'heure ou au mois - vous pouvez choisir de mettre à disposition le numéro d'unité logique du doublon avec une facturation à l'heure ou au mois. Le type de facturation pour le volume d'origine est automatiquement sélectionné. Si vous voulez en choisir un autre pour votre stockage en double, vous pouvez le sélectionner ici.
5. Si vous le souhaitez, vous pouvez spécifier des IOPS ou un niveau d'IOPS pour le nouveau volume. Les IOPS du volume d'origine sont définies par défaut. Les combinaisons de performances et de taille disponibles sont affichées.
    - Si le volume d'origine a un niveau Endurance avec 0,25 IOPS, vous ne pourrez pas effectuer de nouvelle sélection.
    - Si votre volume d'origine est un niveau Endurance de 2, 4 ou 10 IOPS, vous pouvez choisir l'un de ces niveaux pour le nouveau volume.
6. Vous pouvez mettre à jour la taille du nouveau volume pour qu'elle soit supérieure à celle du volume d'origine. La taille du volume d'origine est définie par défaut.

   Le redimensionnement de {{site.data.keyword.blockstorageshort}} est soumis à la limite de 10 fois la taille du volume d'origine.
   {:tip}
7. Vous pouvez mettre à jour l'espace d'instantané pour le nouveau volume en ajoutant plus, moins ou pas du tout d'espace d'instantané. L'espace d'instantané du volume d'origine est défini par défaut.
8. Cliquez sur **Continuer** pour passer commande.

## Création d'un doublon à partir d'un instantané spécifique

1. Accédez à votre liste de {{site.data.keyword.blockstorageshort}}.
2. Cliquez sur un numéro d'unité logique dans la liste pour afficher la page des détails. (Il peut s'agir d'un volume de réplique ou non).
3. Faites défiler l'écran et sélectionnez un instantané existant sur la page des détails, puis cliquez sur **Actions** > **Dupliquer**.   
4. Le type de stockage (Endurance ou Performance) et l'emplacement restent identiques à ce qui est indiqué pour le volume d'origine.
5. Les combinaisons de performances et de taille disponibles sont affichées. Les IOPS du volume d'origine sont définies par défaut. Vous pouvez spécifier des IOPS ou un niveau d'IOPS pour le nouveau volume.
    - Si le volume d'origine a un niveau Endurance avec 0,25 IOPS, vous ne pourrez pas effectuer de nouvelle sélection.
    - Si le volume d'origine a un niveau Endurance avec 2, 4 ou 10 IOPS, vous pouvez indiquer n'importe lequel de ces niveaux pour le nouveau volume.
6. Vous pouvez mettre à jour la taille du nouveau volume pour qu'elle soit supérieure à celle du volume d'origine. La taille du volume d'origine est définie par défaut.

   Le redimensionnement de {{site.data.keyword.blockstorageshort}} est soumis à la limite de 10 fois la taille du volume d'origine.
   {:tip}
7. Vous pouvez mettre à jour l'espace d'instantané pour le nouveau volume en ajoutant plus, moins ou pas du tout d'espace d'instantané. L'espace d'instantané du volume d'origine est défini par défaut.
8. Cliquez sur **Continuer** pour passer votre commande du doublon.


## Création d'un doublon via l'interface SLCLI
Vous pouvez utiliser la commande suivante dans l'interface SLCLI pour créer un doublon du volume {{site.data.keyword.blockstorageshort}}.

```
# slcli block volume-duplicate --help
Usage: slcli block volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate block volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Potential Sizes:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] Minimum: [the size of the
                                  origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin block volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```
{:codeblock}

## Gestion de votre volume en double

Pendant que les données sont copiées depuis le volume d'origine vers le doublon, un statut s'affiche sur la page des détails indiquant que la duplication est en cours. Pendant cette opération, vous pouvez vous connecter à un hôte et lire et écrire sur le volume, mais vous ne pouvez pas créer de plannings d'instantané. Une fois le processus de duplication terminé, le nouveau volume est indépendant du volume d'origine ; il peut être géré avec des instantanés et des réplications comme un volume normal.
