---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Mise à niveau d'un service {{site.data.keyword.blockstorageshort}} vers un service {{site.data.keyword.blockstorageshort}} amélioré

Le service {{site.data.keyword.blockstoragefull}} amélioré est désormais disponible dans certains centres de données. Pour afficher la liste des centres de données mis à niveau et des fonctions disponibles telles que les débits d'E-S/s ajustables et les volumes extensibles, cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html). Pour plus d'informations sur le stockage chiffré géré par un fournisseur, voir [Chiffrement au repos pour {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html).

Le chemin de migration préféré consiste à se connecter simultanément aux deux numéros d'unité logique et à transférer les données directement d'un LUN à l'autre. Les spécificités dépendent de votre système d'exploitation et du fait que les données doivent changer ou non lors de l'opération de copie.

Nous supposons que votre LUN non chiffré est déjà connecté à votre hôte. Si tel n'est pas le cas, suivez les instructions qui s'appliquent à votre système d'exploitation pour accomplir cette tâche :

- [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html)
- [Connexion à des numéros d'unité logique MPIO iSCSI sous CloudLinux](configure-iscsi-cloudlinux.html)
- [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Microsoft Windows](accessing-block-storage-windows.html)

## Création de {{site.data.keyword.blockstorageshort}}

Lorsque vous passez une commande via l'API, spécifiez le package "Storage as a Service" pour être certain d'obtenir les fonctionnalités mises à jour avec votre nouveau stockage.
{:important}

Les instructions suivantes concernent la commande d'un numéro d'unité logique amélioré via le portail {{site.data.keyword.slportal}}. Votre nouveau numéro d'unité logique doit être de taille identique ou supérieure à celle du volume d'origine pour faciliter la migration.

### Commande d'un numéro d'unité logique Endurance

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander du stockage par blocs**.
3. Sélectionnez **Endurance** dans la liste **Sélectionner le type de stockage**.
4. Sélectionnez l'**Emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté dans le même emplacement que le volume précédent.
5. Sélectionnez votre option de facturation. Vous avez le choix entre une facturation à l'heure ou au mois.
6. Sélectionnez le niveau d'IOPS.
7. Cliquez sur **Sélectionner la taille du stockage** et sélectionnez la taille de votre stockage dans la liste.
8. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable.
   Pour plus de remarques et de recommandations en matière d'espace d'image instantanée, voir [Commande d'images instantanés](ordering-snapshots.html).
   {:tip}
9. Choisissez votre **Type de système d'exploitation** dans la liste.
10. Cliquez sur **Continuer**. Les frais mensuels et au prorata s'affichent, avec une dernière possibilité de passer en revue les détails de la commande.
11. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**.

### Commande d'un numéro d'unité logique Performance

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Sur la droite, cliquez sur **Commander {{site.data.keyword.blockstorageshort}}**.
3. Sélectionnez **Performance** dans la liste **Sélectionner le type de stockage**.
4. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes que vous avez précédemment commandés.
5. Sélectionnez votre option de facturation. Vous avez le choix entre une facturation à l'heure ou au mois.
6. Sélectionnez la **taille de stockage** appropriée.
7. Saisissez les E-S/s dans la zone **Spécifier les IOPS**.
8. Cliquez sur **Continuer**. Les frais mensuels et au prorata s'affichent, avec une dernière possibilité de passer en revue les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier votre commande.
9. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**.

Le stockage est mis à disposition en moins d'une minute et est visible sur la page {{site.data.keyword.blockstorageshort}} du portail {{site.data.keyword.slportal}}.



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
   Si vous avez besoin d'aide pour connecter les deux numéros d'unité logique à votre hôte, ouvrez un cas de support. {:tip}

2. Identifiez le type de données figurant sur votre numéro d'unité logique {{site.data.keyword.blockstorageshort}} d'origine et pensez à la meilleure façon de copier ces données sur votre nouveau numéro d'unité logique.
  - Si vous disposez de sauvegardes, d'un contenu statique ou d'autres contenus qui ne sont pas susceptibles d'être modifiés au cours de la copie, le processus s'avère assez simple.
  - Si vous exécutez une base de données ou une machine virtuelle sur votre service {{site.data.keyword.blockstorageshort}}, vérifiez que les données ne sont pas modifiées lors de la copie pour éviter leur altération. Si vous rencontrez des problèmes liés à la bande passante, procédez à la migration pendant les périodes creuses. Si vous avez besoin d'assistance pour ces questions, ouvrez un ticket de demande de service.

3. Copiez vos données.
   - **Microsoft Windows** : pour copier des données depuis votre numéro d'unité logique {{site.data.keyword.blockstorageshort}} d'origine vers le nouveau numéro d'unité logique, formatez le nouveau stockage et copiez les fichiers à l'aide de l'Explorateur Windows.
   - **Linux** : vous pouvez utiliser `rsync` pour copier les données. Exemple :
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   Il est recommandé d'utiliser une fois la commande précédente avec l'indicateur `--dry-run` pour faire en sorte que les chemins s'alignent correctement. Si ce processus est interrompu, vous pouvez supprimer le fichier cible le plus récemment copié et veiller à ce qu'il soit bien copié depuis le début vers le nouvel emplacement.<br/>
   Une fois cette commande terminée sans l'indicateur `--dry-run`, vos données sont copiées sur le nouveau numéro d'unité logique {{site.data.keyword.blockstorageshort}}. Exécutez la commande une nouvelle fois pour être sûr qu'il ne manque aucun élément. Vous pouvez également vérifier manuellement les deux emplacements et rechercher d'éventuels éléments manquants.<br/>
   Une fois la migration terminée, vous pouvez déplacer la production vers le nouveau numéro d'unité logique. Vous pouvez ensuite détacher et supprimer votre LUN d'origine de votre configuration. Cette suppression entraîne également la suppression de tous les instantanés ou répliques du site cible qui étaient associés au numéro d'unité logique d'origine.
