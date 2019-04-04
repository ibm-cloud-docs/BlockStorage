---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Commande de {{site.data.keyword.blockstorageshort}} via la console
{: #orderingthroughConsole}

Vous pouvez mettre à disposition {{site.data.keyword.blockstorageshort}} et l'ajuster en fonction de vos besoins en termes de capacité et d'E-S/s (IOPS). Profitez pleinement de votre stockage grâce à deux options vous permettant de spécifier les performances.

- Vous pouvez effectuer une sélection parmi les niveaux d'IOPS Endurance qui proposent des niveaux de performances prédéfinis afin de prendre en charge les charges de travail pour lesquelles il n'existe aucune exigence bien définie en matière de performances.
- Vous pouvez ajuster votre stockage en fonction d'exigences de performances spécifiques en spécifiant le nombre total d'IOPS avec Performance.

## Commande de {{site.data.keyword.blockstorageshort}} avec des niveaux d'IOPS prédéfinis (Endurance)

1. Connectez-vous au [catalogue IBM Cloud](https://{DomainName}/catalog){:new_window}, puis cliquez sur **Stockage**. Ensuite, sélectionnez **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur **Créer**.

   Vous pouvez également vous connecter au portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}, puis cliquer sur **Stockage** > **{{site.data.keyword.blockstorageshort}}**. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.blockstorageshort}}**.

2. Sélectionnez l'**Emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
3. Facturation. Si vous avez sélectionné un centre de données avec des possibilités améliorées (signalé par un astérisque), vous avez le choix entre une facturation au mois ou à l'heure.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du numéro d'unité logique de bloc sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. Avec une facturation **au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique de bloc est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

        La facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
        {:important}
4. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
5. Sélectionnez **Endurance (IOPS hiérarchisées)** dans la section **Options d'IOPS de stockage**.
6. Sélectionnez le niveau d'IOPS requis par votre application.
    - **0,25 E-S/s par Go** est adapté aux charges de travail avec une faible intensité d'E-S. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
    - **2 E-S/s par Go** est adapté à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données qui sauvegardent des applications Web ou des images de disques de machine virtuelle pour un hyperviseur.
    - **4 E-S/s par Go** est adapté aux charges de travail de forte intensité. Ces charges de travail sont généralement caractérisées par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - **10 E-S/s par Go** est adapté aux charges de travail les plus exigeantes telles que celles créées par les bases de données NoSQL, et au traitement de données pour analyse. Ce niveau est disponible dans des [centres de données sélectionnés](/docs/infrastructure/BlockStorage?topic=BlockStorage-news) pour un stockage qui est mis à disposition à hauteur de 4 To.
7. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Choisissez votre **Type de système d'exploitation** dans la liste.<br/>

   Ce choix est basé sur le système d'exploitation sur lequel votre hôte s'exécute et il ne peut pas être modifié ultérieurement. Par exemple, si votre serveur est Ubuntu ou RHEL, choisissez Linux. Si votre hôte est un serveur Windows 2012 ou Windows 2016, sélectionnez l'option Windows 2008+ dans la liste. Pour plus d'informations sur les différentes options Windows, voir la [Foire aux questions](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
   {:tip}
9. Sur la droite, passez en revue votre récapitulatif de commande et appliquez votre code promo si vous en avez un.

   Les remises sont appliquées lors du traitement de la commande.
   {:note}
10. Après avoir lu les dispositions, cochez la case **J'ai lu et j'accepte les contrats de service tiers**.
11. Cliquez sur **Créer**. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Commande de {{site.data.keyword.blockstorageshort}} avec un nombre d'IOPS personnalisé (Performance)

1. Connectez-vous au [catalogue IBM Cloud](https://{DomainName}/catalog){:new_window}, puis cliquez sur **Stockage**. Ensuite, sélectionnez {{site.data.keyword.blockstorageshort}}, puis cliquez sur **Créer**.

   Vous pouvez également vous connecter au portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}, puis cliquer sur **Stockage** > **{{site.data.keyword.blockstorageshort}}**. Dans l'angle supérieur droit, cliquez sur **Commander {{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté au même emplacement que celui du ou des hôtes de calcul dont vous disposez.
3. Facturation. Si vous avez sélectionné un centre de données avec des possibilités améliorées (signalé par un astérisque), vous avez le choix entre une facturation au mois ou à l'heure.
     1. Avec la facturation **horaire**, le nombre d'heures d'existence du numéro d'unité logique de bloc sur le compte est calculé lors de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, à la première occurrence de l'un de ces deux événements. La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation horaire est disponible uniquement pour le stockage qui est mis à disposition dans des [centres de données sélectionnés](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. Avec une facturation **au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique de bloc est supprimé avant la fin du cycle de facturation. La facturation mensuelle convient si vous avez besoin d'un stockage pour des charges de travail qui utilisent des données devant être stockées et rester accessibles pour de longues périodes (un mois ou plus).

        La facturation mensuelle est utilisée par défaut pour le stockage fourni dans les centres de données qui n'ont **pas** été mis à jour avec les fonctionnalités améliorées.
        {:note}
4. Entrez votre taille de stockage dans la zone **Nouvelle taille de stockage**.
5. Sélectionnez **Performance (IOPS allouées)** dans la section **Options d'IOPS de stockage**.
6. Entrez le nombre d'IOPS dans la zone **IOPS allouées**.
7. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Cet espace vient en complément de votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Choisissez votre **Type de système d'exploitation** dans la liste.<br/>

   Ce choix est basé sur le système d'exploitation sur lequel votre hôte s'exécute et il ne peut pas être modifié ultérieurement. Par exemple, si votre serveur est Ubuntu ou RHEL, choisissez Linux. Si votre hôte est un serveur Windows 2012 ou Windows 2016, sélectionnez l'option Windows 2008+ dans la liste. Pour plus d'informations sur les différentes options Windows, voir la [Foire aux questions](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
   {:tip}
9. Sur la droite, passez en revue votre récapitulatif de commande et appliquez votre code promo si vous en avez un.

   Les remises sont appliquées lors du traitement de la commande.
   {:note}
10. Après avoir lu les dispositions, cochez la case **J'ai lu et j'accepte les contrats de service tiers**.
11. Cliquez sur **Créer**. Votre nouvelle allocation de stockage est disponible en quelques minutes.

Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de vos volumes, contactez votre commercial. Pour en savoir plus sur l'augmentation des limites, cliquez [ici](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Pour connaître la limite des autorisations simultanées, reportez-vous à la [Foire aux questions](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Connexion de votre nouveau stockage
{: #mountingnewLUN}

Lorsque votre demande de mise à disposition est terminée, autorisez vos hôtes à accéder au nouveau stockage et configurez votre connexion. Suivez le lien approprié en fonction du système d'exploitation de votre hôte.
- [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Montage d'un numéro d'unité logique dans le stockage partagé XenServer](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Configuration de Block Storage pour une sauvegarde avec cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuration de Block Storage pour une sauvegarde avec Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Considérations relatives à la reprise après incident

Pour éviter toute perte de données et garantir la continuité opérationnelle, prévoyez de répliquer vos serveurs et votre stockage dans un autre centre de données. La réplication permet de synchroniser vos données entre deux emplacements différents selon votre planning d'instantané. Pour plus d'informations, voir [Réplication de données](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).

Si vous voulez cloner votre volume et l'utiliser indépendamment du volume d'origine, voir [Création d'un volume de blocs en double](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Identification de {{site.data.keyword.blockstorageshort}} sur votre facture

Tous les numéros d'unité logique apparaissent sur votre facture sous forme de lignes d'article. “Endurance Storage Service” s'affiche pour les volumes de type Endurance et "Performance Storage Service" apparaît pour les volumes de type Performance. Le taux varie en fonction de votre niveau de stockage. Vous pouvez développer Endurance ou Performance pour voir qu'il s'agit de {{site.data.keyword.blockstorageshort}}.
