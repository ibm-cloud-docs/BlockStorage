---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Mise à niveau d'un service {{site.data.keyword.blockstorageshort}} vers un service {{site.data.keyword.blockstorageshort}} amélioré

Le service {{site.data.keyword.blockstoragefull}} amélioré est désormais disponible dans certains centres de données. Pour afficher la liste des centres de données mis à niveau et des fonctions disponibles telles que les débits d'E-S/s ajustables et les volumes extensibles, cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html). Pour plus de détails sur le stockage chiffré géré par le fournisseur, lisez l'article [Chiffrement au repos de {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html). 

Le chemin de migration préféré consiste à se connecter simultanément aux deux numéros d'unité logique et à transférer les données directement d'un LUN à l'autre. Les spécificités dépendent de votre système d'exploitation et du fait que les données doivent changer ou non lors de l'opération de copie. 

Nous supposons que votre LUN non chiffré est déjà connecté à votre hôte. Si tel n'est pas le cas, suivez les instructions qui s'appliquent à votre système d'exploitation pour accomplir cette tâche :

- [Accès à {{site.data.keyword.blockstorageshort}} sous Linux](accessing_block_storage_linux.html)
- [Accès à {{site.data.keyword.blockstorageshort}} sous Windows](accessing-block-storage-windows.html)

 
## Création d'un service {{site.data.keyword.blockstorageshort}}

**IMPORTANT** : Lors du passage d'une commande avec une API, indiquez le package "Stockage en tant que service" pour être sûr de recevoir les fonctionnalités mises à jour avec votre nouveau stockage. 

Les instructions suivantes concernent la commande d'un numéro d'unité logique amélioré via l'interface utilisateur. Votre nouveau numéro d'unité logique doit être de taille identique ou supérieure à celle du volume d'origine pour faciliter la migration.

### Commande d'un numéro d'unité logique Endurance

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander du stockage par blocs**.
3. Sélectionnez **Endurance** dans la liste **Sélectionner le type de stockage**. 
4. Sélectionnez l'**Emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté dans le même emplacement que le volume précédent. 
5. Sélectionnez votre option de facturation. Vous avez le choix entre une facturation à l'heure ou au mois. 
6. Sélectionnez le niveau d'E-S/s. 
7. Cliquez sur *Sélectionner la taille du stockage** et sélectionnez la taille de votre stockage dans la liste.
8. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Elle vient s'ajouter à votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](ordering-snapshots.html).
9. Choisissez votre **Type de système d'exploitation** dans la liste. 
10. Cliquez sur **Continuer**. Les frais mensuels et au prorata s'affichent, avec une dernière possibilité de passer en revue les détails de la commande.
11. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. 

### Commande d'un numéro d'unité logique Performance

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander du stockage par blocs**.
3. Sélectionnez **Performance** dans la liste déroulante **Sélectionner le type de stockage**.
4. Cliquez sur la liste déroulante **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté dans le même emplacement que l'hôte ou les hôtes commandés précédemment.
5. Sélectionnez votre option de facturation. Vous avez le choix entre une facturation à l'heure ou au mois. 
6. Sélectionnez le bouton d'option en regard de la **Taille de stockage** appropriée.
7. Saisissez les E-S/s dans la zone **Spécifier les IOPS**.
8. Cliquez sur **Continuer**. Les frais mensuels et au prorata s'affichent, avec une dernière possibilité de passer en revue les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier votre commande.
9. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur le bouton **Valider la commande. 


Le stockage sera mis à disposition en moins d'une minute et sera visible sur la page {{site.data.keyword.blockstorageshort}} du portail {{site.data.keyword.slportal}}.


 
## Connexion d'un nouveau service {{site.data.keyword.blockstorageshort}} à l'hôte

Les hôtes "autorisés" sont des hôtes qui disposent de droits d'accès à un volume. Sans autorisation de l'hôte, vous ne pouvez pas accéder au stockage à partir de votre système, ni l'utiliser. L'autorisation d'un hôte pour accéder à votre volume génère le nom d'utilisateur, le mot de passe et le nom qualifié iSCSI, qui est nécessaire pour monter la connexion iSCSI d'E-S multi-accès.

1. Cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom LUN.

2. Faites défiler la page jusqu'à **Hôtes autorisés**. 

3. A droite, cliquez sur **Hôte autorisé**. Sélectionnez les hôtes qui peuvent accéder au volume.

 
## Instantanés et réplication

Des instantanés et une réplication sont-ils établis pour votre numéro d'unité logique d'origine ? Si tel est le cas, vous devrez configurer la réplication, l'espace d'instantané et créer des plannings d'instantané pour le nouveau numéro d'unité logique avec les mêmes paramètres que le volume d'origine. 

Notez que si votre centre de données cible de réplication n'a pas été mis à niveau pour le chiffrement, vous ne serez pas en mesure d'établir la réplication pour le nouveau volume tant que le centre de données ne sera pas mis à niveau.

 
## Migration des données

Vous devez être connecté à la fois au nouveau numéro d'unité logique {{site.data.keyword.blockstorageshort}} et à celui d'origine.  
- Si vous avez besoin d'aide pour connecter les deux numéros d'unité logique à votre hôte, ouvrez un ticket de demande de service. 

### Remarques concernant les données

A ce stade, vous devez prendre en compte le type de données que vous avez sur votre unité logique {{site.data.keyword.blockstorageshort}} d'origine et déterminer la meilleure façon de les copier sur votre nouveau LUN. Si vous disposez de sauvegardes, de contenu statique et d'objets qui ne doivent pas être modifiés lors de la copie, il n'y aura pas beaucoup d'éléments à prendre en compte.

Si vous exécutez une base de données ou une machine virtuelle sur votre service {{site.data.keyword.blockstorageshort}}, vérifiez que les données ne sont pas modifiées lors de la copie pour éviter leur altération. Si vous avez des problèmes de bande passante, vous devez effectuer la migration en heures creuses. Si vous avez besoin d'assistance pour ces questions, ouvrez un ticket de demande de service.
 
### Microsoft Windows

Pour copier des données de votre LUN {{site.data.keyword.blockstorageshort}} d'origine vers votre nouveau numéro d'unité logique, formatez le nouveau stockage et copiez les fichiers dessus à l'aide de l'Explorateur Windows.

 
### Linux

Vous pouvez utiliser 'rsync' pour copier les données. Voici un exemple de commande :

```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

Il est conseillé d'utiliser la commande ci-dessus avec l'indicateur `--dry-run` une fois pour vérifier que les chemins sont alignés correctement. Si ce processus est interrompu, vous voudrez peut-être supprimer le dernier fichier de destination qui a été copié pour vous assurer qu'il a été copié vers le nouvel emplacement depuis le début.

Lorsque cette commande s'exécute sans l'indicateur `--dry-run`, vos données doivent être copiées vers le nouveau numéro d'unité logique {{site.data.keyword.blockstorageshort}}. Faites défiler l'écran vers le haut et exécutez à nouveau la commande pour vous assurer qu'aucun élément n'a été oublié. Vous pouvez également passer en revue manuellement les deux emplacements pour vérifier qu'il ne manque rien.

Une fois la migration terminée, vous pourrez déplacer la production vers le nouveau numéro d'unité logique. Vous pouvez ensuite détacher et supprimer votre LUN d'origine de votre configuration. Notez que la suppression concernera également les instantanés ou répliques figurant sur le site cible qui ont été associés au numéro d'unité logique d'origine.
