---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Migration de {{site.data.keyword.blockstorageshort}} vers {{site.data.keyword.blockstorageshort}} chiffré

{{site.data.keyword.blockstoragefull}} chiffré pour Endurance ou Performance est désormais disponible dans certains centres de données. Vous trouverez ci-dessous des informations sur la migration de votre service {{site.data.keyword.blockstorageshort}} non chiffré en service chiffré. Pour plus de détails sur le stockage chiffré géré par le fournisseur, lisez l'[article Chiffrement au repos de {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html). Pour afficher la liste des centres de données mis à niveau et des fonctions disponibles, cliquez [ici](new-ibm-block-and-file-storage-location-and-features.html).

Le chemin de migration préféré consiste à se connecter simultanément aux deux numéros d'unité logique et à transférer les données directement d'un LUN de fichier à l'autre. Les spécificités dépendent de votre système d'exploitation et du fait que les données doivent changer ou non lors de l'opération de copie.

Les scénarios les plus courants vous sont présentés. Nous supposons que votre LUN de fichier non chiffré est déjà connecté à votre hôte. Si tel n'est pas le cas, suivez les instructions ci-dessous qui s'appliquent au système d'exploitation que vous exécutez pour accomplir cette tâche.

- [Accès à {{site.data.keyword.blockstorageshort}} sous Linux](accessing_block_storage_linux.html)

- [Accès à {{site.data.keyword.blockstorageshort}} sous Windows](accessing-block-storage-windows.html)

 
## Création d'un numéro d'unité logique chiffré

Procédez comme suit pour créer un numéro d'unité logique chiffré d'une taille supérieure ou égale afin de faciliter le processus de migration. 
Commande d'un numéro d'unité logique de stockage Endurance

1. Cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** à partir de la page d'accueil du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}** dans le catalogue {{site.data.keyword.BluSoftlayer_full}}.

2. Cliquez sur le lien **Commander du stockage par blocs** dans la page {{site.data.keyword.blockstorageshort}}.

3. Sélectionnez **Endurance**.

4. Sélectionnez le centre de données où se trouve votre numéro d'unité logique d'origine. <br/> **Remarque** : Le chiffrement est uniquement disponible dans certains centres de données.

5. Sélectionnez le niveau d'E-S/s souhaité.

6. Sélectionnez la quantité souhaitée d'espace de stockage (en Go). Pour les To, 1 To équivaut à 1 000 Go, et 12 To à 12 000 Go.

7. Saisissez la quantité souhaitée d'espace de stockage pour les instantanés (en Go).

8. Sélectionnez le système d'exploitation VMware dans la liste déroulante.

9. Soumettez la commande.

## Commande d'un numéro d'unité logique de stockage Performance chiffré

1. Cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** à partir de la page d'accueil du portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU cliquez sur **Infrastructure** > **Stockage** > **{{site.data.keyword.blockstorageshort}}** dans le catalogue {{site.data.keyword.BluSoftlayer_full}}.

2. Cliquez sur **Commander du stockage par blocs**.

3. Sélectionnez **Performance**.

4. Sélectionnez le centre de données où se trouve votre numéro d'unité logique d'origine. Notez que le chiffrement est uniquement disponible dans les centres de données signalés par un astérisque (`*`).

5. Sélectionnez la quantité souhaitée d'espace de stockage (en Go). Pour les To, 1 To équivaut à 1 000 Go, et 12 To à 12 000 Go.

6. Saisissez la quantité souhaitée d'E-S/s en intervalles de 100.

7. Sélectionnez le système d'exploitation VMware dans la liste déroulante.

8. Soumettez la commande.

Le stockage sera mis à disposition en moins d'une minute et sera visible sur la page {{site.data.keyword.blockstorageshort}} du portail {{site.data.keyword.slportal}}.

 
## Connexion d'un nouveau volume à un hôte

Les hôtes "autorisés" sont des hôtes qui disposent de droits d'accès à un volume. Sans autorisation de l'hôte, vous ne pouvez pas accéder au stockage à partir de votre système, ni l'utiliser. L'autorisation d'un hôte pour accéder à votre volume génère le nom d'utilisateur, le mot de passe et le nom qualifié iSCSI, qui est nécessaire pour monter la connexion iSCSI d'E-S multi-accès.

1. Cliquez sur **Stockage**  > **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom LUN.

2. Faites défiler la page jusqu'à la section** Hôtes autorisés**.

3. Cliquez sur le lien **Hôte autorisé** dans la partie droite de la page. Sélectionnez les hôtes qui peuvent accéder au volume.

 
## Instantanés et réplication

Des instantanés et une réplication sont-ils établis pour votre numéro d'unité logique d'origine ? Si tel est le cas, vous devez configurer la réplication, l'espace d'instantané et créer des plannings d'instantané pour le nouveau numéro d'unité logique chiffré avec les mêmes paramètres que le volume d'origine. 

Notez que si votre centre de données cible de réplication n'a pas été mis à niveau pour le chiffrement, vous ne serez pas en mesure d'établir la réplication pour le volume chiffré tant que le centre de données ne sera pas mis à niveau.

 
## Migration des données

Vous devez être connecté à la fois à vos numéros d'unité logique {{site.data.keyword.blockstorageshort}} d'origine et chiffré. 
- Dans le cas contraire, assurez-vous que vous avez suivi correctement les étapes ci-dessus et référencées dans d'autres articles. Ouvrez un ticket de demande de service pour obtenir de l'aide sur la connexion des deux numéros d'unité logique.

### Remarques concernant les données

A ce stade, vous voudrez prendre en compte le type de données que vous avez sur votre unité logique {{site.data.keyword.blockstorageshort}} d'origine et déterminer la meilleure façon de les copier sur votre LUN chiffré. Si vous disposez de sauvegardes, de contenu statique et d'objets qui ne doivent pas être modifiés lors de la copie, il n'y aura pas beaucoup d'éléments à prendre en compte.

Si vous exécutez une base de données ou une machine virtuelle sur votre service {{site.data.keyword.blockstorageshort}}, vérifiez que les données situées sur le numéro d'unité logique d'origine ne sont pas modifiées lors de la copie pour éviter leur altération. Si vous avez des problèmes de bande passante, vous devez effectuer la migration en heures creuses. Si vous avez besoin d'assistance pour ces questions, n'hésitez pas à ouvrir un ticket de demande de service.
 
### Microsoft Windows

Pour copier des données de votre LUN {{site.data.keyword.blockstorageshort}} d'origine vers votre numéro d'unité logique chiffré, formatez le nouveau stockage et copiez les fichiers dessus à l'aide de l'Explorateur Windows.

 
### Linux

Vous pouvez utiliser rsync pour copier les données. Voici un exemple de commande :

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

Il est conseillé d'utiliser la commande ci-dessus avec l'indicateur --dry-run une fois pour vérifier que les chemins sont alignés correctement. Si ce processus est interrompu, vous voudrez peut-être supprimer le dernier fichier de destination qui a été copié pour vous assurer qu'il a été copié depuis le début vers le nouvel emplacement.

Une fois que cette commande s'est exécutée sans l'indicateur --dry-run, vos données doivent être copiées vers le numéro d'unité logique {{site.data.keyword.blockstorageshort}} chiffré. Vous devez faire défiler l'écran vers le haut et exécuter à nouveau la commande pour vous assurer qu'aucun élément n'a été oublié. Vous pouvez également passer en revue manuellement les deux emplacements pour vérifier qu'il ne manque rien.

Une fois la migration terminée, vous pourrez déplacer la production vers le numéro d'unité logique chiffré, puis détacher et supprimer votre LUN d'origine de votre configuration. Notez que la suppression concernera également les instantanés ou répliques figurant sur le site cible qui ont été associés au numéro d'unité logique d'origine.
