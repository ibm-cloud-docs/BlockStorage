---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# Commande de {{site.data.keyword.blockstorageshort}}

Vous pouvez mettre à disposition le stockage {{site.data.keyword.blockstorageshort}} et l'ajuster en fonction de vos besoins en termes de capacité et d'IOPS. Profitez pleinement de votre stockage grâce à deux options vous permettant de spécifier les performances.

- Vous pouvez effectuer une sélection parmi les niveaux d'IOPS Endurance qui proposent des niveaux de performances prédéfinis afin de prendre en charge les charges de travail pour lesquelles il n'existe aucune exigence bien définie en matière de performances. 
- Vous pouvez ajuster votre stockage en fonction d'exigences de performances très spécifiques en spécifiant le nombre total d'IOPS avec Performance.

## Commande de {{site.data.keyword.blockstorageshort}} avec des niveaux d'IOPS prédéfinis (Endurance)

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.blockstorageshort}}**.
3. Sélectionnez l'**Emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
4. Facturation. Si vous avez sélectionné un centre de données avec des possibilités améliorées (signalé par un astérisque), vous avez le choix entre une facturation au mois ou à l'heure. 
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du numéro d'unité logique de bloc sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Avec une facturation **au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique de bloc est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
        >**REMARQUE** : la facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
5. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
6. Sélectionnez **Endurance (IOPS hiérarchisées)** dans la section **Options d'IOPS de stockage**.
7. Sélectionnez le niveau d'IOPS requis par votre application.
    - **0,25 E-S/s par Go** est adapté aux charges de travail avec une faible intensité d'E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
    - **2 E-S/s par Go** est adapté à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.
    - **4 E-S/s par Go** est adapté aux charges de travail de forte intensité. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - **10 E-S/s par Go** est adapté aux charges de travail les plus exigeantes telles que celles créées par les bases de données NoSQL, et au traitement de données pour analyse. Ce niveau est disponible dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html) pour un stockage qui est mis à disposition à hauteur de 4 To.
8. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](ordering-snapshots.html).
9. Choisissez votre **Type de système d'exploitation** dans la liste.
10. Cochez les cases **Dispositions**, puis cliquez sur **Passer une commande**.
11. Votre nouvelle allocation de stockage est disponible en quelques minutes.

>**Remarque** : par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Découvrez plus de détails sur l'augmentation des limites [ici](managing-storage-limits.html).<br/><br/>Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](BlockStorageFAQ.html).
 
## Commande de {{site.data.keyword.blockstorageshort}} avec un nombre d'IOPS personnalisé (Performance)

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.blockstorageshort}}**.
3. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
4. Facturation. Si vous avez sélectionné un centre de données avec des possibilités améliorées (signalé par un astérisque), vous avez le choix entre une facturation au mois ou à l'heure.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du numéro d'unité logique de bloc sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Avec une facturation **au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique de bloc est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).
        >**REMARQUE** : la facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
5. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
6. Sélectionnez **Performance (IOPS allouées)** dans la section **Options d'IOPS de stockage**.
7. Entrez le nombre d'IOPS dans la zone **IOPS allouées**.
8. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](ordering-snapshots.html).
9. Choisissez votre **Type de système d'exploitation** dans la liste.
10. Votre nouvelle allocation de stockage est disponible en quelques minutes.

>**Remarque** : par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Découvrez plus de détails sur l'augmentation des limites [ici](managing-storage-limits.html).<br/><br/>Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](BlockStorageFAQ.html).

## Connexion de votre nouveau stockage

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html)
- [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Microsoft Windows](accessing-block-storage-windows.html)
- [Configuration de Block Storage pour une sauvegarde avec cPanel](configure-backup-cpanel.html)
- [Configuration de Block Storage pour une sauvegarde avec Plesk](configure-backup-plesk.html)

## Identification de {{site.data.keyword.blockstorageshort}} sur votre facture

Tous les numéros d'unité logique apparaissent sur votre facture sous forme de lignes d'article. “Endurance Storage Service” s'affiche pour les volumes de type Endurance et "Performance Storage Service"  apparaît pour les volumes de type Performance. Le taux varie en fonction de votre niveau de stockage. Vous pouvez développer Endurance ou Performance pour voir qu'il s'agit de {{site.data.keyword.blockstorageshort}}.
