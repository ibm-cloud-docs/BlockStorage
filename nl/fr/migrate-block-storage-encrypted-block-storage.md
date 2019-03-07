---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Mise à niveau d'un service {{site.data.keyword.blockstorageshort}} vers un service {{site.data.keyword.blockstorageshort}} amélioré
{: #migratestorage}

Le service {{site.data.keyword.blockstoragefull}} amélioré est désormais disponible dans certains centres de données. Pour afficher la liste des centres de données mis à niveau et des fonctions disponibles telles que les débits d'E-S/s ajustables et les volumes extensibles, cliquez [ici](/docs/infrastructure/BlockStorage?topic=BlockStorage-news). Pour plus d'informations sur le stockage chiffré géré par un fournisseur, voir [Chiffrement au repos pour {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

Le chemin de migration préféré consiste à se connecter simultanément aux deux numéros d'unité logique et à transférer les données directement d'un LUN à l'autre. Les spécificités dépendent de votre système d'exploitation et du fait que les données doivent changer ou non lors de l'opération de copie.

Nous supposons que votre LUN non chiffré est déjà connecté à votre hôte. Si tel n'est pas le cas, suivez les instructions qui s'appliquent à votre système d'exploitation pour accomplir cette tâche :

- [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Tous les volumes {{site.data.keyword.blockstorageshort}} améliorés mis à disposition dans ces centres de données ont un point de montage différent de celui des volumes non chiffrés. Pour vérifier que vous utilisez le bon point de montage pour les deux types de volume de stockage, vous pouvez afficher les informations sur le point de montage sur la page **Détails du volume** de la console. Vous pouvez également accéder au point de montage correct via un appel API : `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

## Création d'un {{site.data.keyword.blockstorageshort}}

Lorsque vous passez une commande via l'API, spécifiez le package "Storage as a Service" pour être certain d'obtenir les fonctionnalités mises à jour avec votre nouveau stockage.
{:important}

Vous pouvez commander un numéro d'unité logique étendu via la console IBM Cloud ou le portail {{site.data.keyword.slportal}}. Votre nouveau numéro d'unité logique doit être de taille identique ou supérieure à celle du volume d'origine pour faciliter la migration.

- [Commande de {{site.data.keyword.blockstorageshort}} avec des niveaux d'IOPS prédéfinis (Endurance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [Commande de {{site.data.keyword.blockstorageshort}} avec un nombre d'IOPS personnalisé (Performance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

Votre nouveau stockage est disponible pour montage en quelques minutes. Il figure dans la Liste de ressources et dans la liste {{site.data.keyword.blockstorageshort}}.

## Connexion d'un nouveau {{site.data.keyword.blockstorageshort}} à l'hôte

Les hôtes "autorisés" sont des hôtes auxquels des droits d'accès à un volume ont été accordés. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage ni l'utiliser depuis votre système. L'autorisation d'un hôte pour accéder à votre volume génère le nom d'utilisateur, le mot de passe et le nom qualifié iSCSI, qui est nécessaire pour monter la connexion iSCSI d'E-S multi-accès.

1. Cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom de numéro d'unité logique.
2. Faites défiler la page jusqu'à **Hôtes autorisés**.
3. A droite, cliquez sur **Hôte autorisé**. Sélectionnez les hôtes qui peuvent accéder au volume.


## Instantanés et réplication

Des instantanés et une réplication sont-ils établis pour votre numéro d'unité logique d'origine ? Si tel est le cas, vous devrez configurer la réplication, l'espace d'image instantanée et créer des plannings d'instantané pour le nouveau numéro d'unité logique avec les mêmes paramètres que le volume d'origine.

Si votre centre de données cible de réplication n'a pas encore été mis à niveau, vous ne pouvez pas établir la réplication pour le nouveau volume tant que le centre de données n'est pas mis à niveau.


## Migration de vos données

1. Connectez-vous à votre numéro d'unité logique d'origine et à vos nouveaux numéros d'unité logique {{site.data.keyword.blockstorageshort}}.

   Si vous avez besoin d'aide pour connecter les deux numéros d'unité logique à votre hôte, ouvrez un cas de support.
   {:tip}

2. Identifiez le type de données figurant sur votre numéro d'unité logique {{site.data.keyword.blockstorageshort}} d'origine et pensez à la meilleure façon de copier ces données sur votre nouveau numéro d'unité logique.
  - Si vous disposez de sauvegardes, d'un contenu statique ou d'autres contenus qui ne sont pas susceptibles d'être modifiés au cours de la copie, vous n'avez pas à vous inquiéter.
  - Si vous exécutez une base de données ou une machine virtuelle sur votre service {{site.data.keyword.blockstorageshort}}, vérifiez que les données ne sont pas modifiées lors de la copie pour éviter leur altération.
  - Si vous rencontrez des problèmes liés à la bande passante, procédez à la migration pendant les périodes creuses.
  - Si vous avez besoin d'aide sur ces questions, ouvrez un cas de support.

3. Copiez vos données.
   - Pour **Microsoft Windows**, formatez le nouveau stockage et copiez les données depuis votre numéro d'unité logique {{site.data.keyword.blockstorageshort}} d'origine vers le nouveau numéro à l'aide de l'Explorateur Windows.
   - Pour **Linux**, vous pouvez utiliser `rsync` pour copier les données.
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   Il est recommandé d'utiliser une fois la commande précédente avec l'indicateur `--dry-run` pour faire en sorte que les chemins s'alignent correctement. Si ce processus est interrompu, vous pouvez supprimer le fichier cible le plus récemment copié et veiller à ce qu'il soit bien copié depuis le début vers le nouvel emplacement.<br/>
   Une fois cette commande terminée sans l'indicateur `--dry-run`, vos données sont copiées sur le nouveau numéro d'unité logique {{site.data.keyword.blockstorageshort}}. Exécutez la commande une nouvelle fois pour être sûr qu'il ne manque aucun élément. Vous pouvez également vérifier manuellement les deux emplacements et rechercher d'éventuels éléments manquants.<br/>
   Une fois la migration terminée, vous pouvez déplacer la production vers le nouveau numéro d'unité logique. Vous pouvez ensuite détacher et supprimer votre LUN d'origine de votre configuration. Cette suppression entraîne également la suppression de tous les instantanés ou répliques du site cible qui étaient associés au numéro d'unité logique d'origine.
