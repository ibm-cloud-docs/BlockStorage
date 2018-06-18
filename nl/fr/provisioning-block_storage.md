---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Commande de {{site.data.keyword.blockstorageshort}}

Il existe deux méthodes différentes de mise à disposition de {{site.data.keyword.blockstorageshort}} en fonction de vos besoins et préférences. Les deux options possibles sont les suivantes : 

- **Endurance** : met à disposition des niveaux Endurance qui mettent en oeuvre des niveaux de performance prédéfinis et des fonctionnalités telles que les instantanés et la réplication. 
- **Performance** : crée un environnement Performance haute puissance, dans lequel vous pouvez allouer les opérations spécifiques d'entrée/sortie par seconde (E-S/s) souhaitées.

## Commande d'Endurance pour {{site.data.keyword.blockstorageshort}}

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage** > **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander du stockage par blocs**.
3. Sélectionnez **Endurance** dans la liste **Sélectionner le type de stockage**. 
4. Sélectionnez l'**Emplacement** de votre déploiement (centre de données).
   - Vérifiez que le nouveau stockage est ajouté dans le même emplacement que l'hôte ou les hôtes de calcul commandés précédemment.
   - Si vous avez sélectionné un centre de données avec des possibilités améliorées (signalé par un astérisque), vous avez le choix entre une facturation au mois ou à l'heure. 
     1. Avec une facturation **à l'heure**, le calcul du nombre d'heures pendant lesquelles le numéro d'unité logique du bloc a existé sur le compte s'effectue au moment de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, selon l'événement qui se produit en premier. La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation à l'heure est uniquement disponible pour un stockage mis à disposition dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Avec une facturation **au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique de bloc est supprimé avant la fin du cycle de facturation. La facturation au mois représente un bon choix pour un stockage utilisé dans des charges de travail de production qui utilisent des données devant être stockées et accessibles pendant de longues périodes (un mois ou plus).
     **REMARQUE** : Le type de facturation au mois est utilisé par défaut pour le stockage mis à disposition dans des centres de données **qui ne sont pas** mis à jour avec des possibilités améliorées.
5. Sélectionnez le niveau d'E-S/s nécessaire pour votre application. 
    - **0,25 E-S/s par Go** est adapté aux charges de travail avec une faible intensité d'E-S. Ces charges de travail se caractérisent généralement par un fort pourcentage de données inactives à un moment donné. Exemples d'applications : stockage de boîtes aux lettres ou partages de fichiers au niveau d'un service dans une entreprise.
    - **2 E-S/s par Go** est adapté à des usages plus généraux. Exemples d'applications : hébergement de petites bases de données utilisées par des applications Web ou d'images de disques de machine virtuelle pour un hyperviseur.
    - **4 E-S/s par Go** est adapté aux charges de travail de forte intensité. Ces charges de travail se caractérisent généralement par un pourcentage élevé de données actives à un moment donné. Exemples d'applications : bases de données transactionnelles, bases de données sensibles aux performances.
    - **10 E-S/s par Go** est adapté aux charges de travail les plus exigeantes telles que celles créées par les bases de données NoSQL, et au traitement de données pour analyse.  Ce niveau est disponible dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html) pour un stockage mis à disposition de 4 To au maximum.
6. Cliquez sur *Sélectionner la taille du stockage** et sélectionnez la taille de votre stockage dans la liste.
7. Cliquez sur **Indiquer la taille de l'espace d'instantané** et sélectionnez la taille de l'image instantanée dans la liste. Elle vient s'ajouter à votre espace utilisable. Pour les considérations et recommandations relatives à l'espace d'instantané, lisez la section [Commande d'instantanés](ordering-snapshots.html).
8. Choisissez votre **Type de système d'exploitation** dans la liste. 
9. Cliquez sur **Continuer**. Les frais mensuels et au prorata s'affichent, avec une dernière possibilité de passer en revue les détails de la commande.
10. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur **Valider la commande**. 
11. Votre nouvelle allocation de stockage devrait être disponible dans quelques minutes.

**Remarque** : Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Contactez votre ingénieur commercial pour augmenter le nombre de volumes. Découvrez plus de détails sur l'augmentation des limites [ici](managing-storage-limits.html).

Pour la limite relative aux autorisations simultanées, consultez notre [Foire aux questions](BlockStorageFAQ.html).
 
## Commande de Performance pour Block Storage

1. Dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}** OU dans le catalogue {{site.data.keyword.BluSoftlayer_full}}, cliquez sur **Infrastructure > Stockage > {{site.data.keyword.blockstorageshort}}**.
2. Dans l'angle supérieur droit, cliquez sur **Commander du stockage par blocs**.
3. Sélectionnez **Performance** dans la liste déroulante **Sélectionner le type de stockage**.
4. Cliquez sur la liste déroulante **Emplacement** et sélectionnez votre centre de données.
   - Vérifiez que le nouveau stockage est ajouté dans le même emplacement que l'hôte ou les hôtes commandés précédemment.
   - Si vous avez sélectionné un centre de données avec des possibilités améliorées (signalé par un astérisque (*) dans la liste déroulante), vous avez le choix entre une facturation au mois ou à l'heure. 
     1. Avec une facturation **à l'heure**, le calcul du nombre d'heures pendant lesquelles le numéro d'unité logique du bloc a existé sur le compte s'effectue au moment de la suppression du numéro d'unité logique ou à la fin du cycle de facturation, selon l'événement qui se produit en premier.  La facturation à l'heure représente un bon choix pour un stockage qui est utilisé pendant quelques jours ou moins d'un mois entier. La facturation à l'heure est uniquement disponible pour un stockage mis à disposition dans [certains centres de données](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Avec une facturation **au mois**, le calcul du prix est proportionnel à la durée écoulée entre la date de création et la fin du cycle de facturation, et facturé immédiatement. Aucun remboursement n'est possible si un numéro d'unité logique de bloc est supprimé avant la fin du cycle de facturation. La facturation au mois représente un bon choix pour un stockage utilisé dans des charges de travail de production qui utilisent des données devant être stockées et accessibles pendant de longues périodes (un mois ou plus).
     **REMARQUE** : Le type de facturation au mois est utilisé par défaut pour le stockage mis à disposition dans des centres de données **qui ne sont pas** mis à jour avec des possibilités améliorées.
5. Sélectionnez le bouton d'option en regard de la **Taille de stockage** appropriée.
6. Saisissez les E-S/s dans la zone **Spécifier les IOPS**.
7. Cliquez sur **Continuer**. Les frais mensuels et au prorata s'affichent, avec une dernière possibilité de passer en revue les détails de la commande. Cliquez sur **Précédent** si vous souhaitez modifier votre commande.
8. Cochez la case **J'ai lu et j'accepte l'intégralité du Contrat cadre de service...** et cliquez sur le bouton **Valider la commande. 
9. Votre nouvelle allocation de stockage devrait être disponible dans quelques minutes.

**Remarque** : Par défaut, vous pouvez mettre à disposition un total combiné de 250 volumes {{site.data.keyword.blockstorageshort}}. Pour augmenter le nombre de volumes, contactez votre ingénieur commercial. Découvrez plus de détails sur l'augmentation des limites [ici](managing-storage-limits.html).

Pour la limite relative aux autorisations simultanées, consultez notre [Foire aux questions](BlockStorageFAQ.html).

## Identification du service {{site.data.keyword.blockstorageshort}} sur ma facture

Tous les numéros d'unité logique apparaîtront sur votre facture sous forme de ligne d'article ; Endurance s'affichera sous la forme "Endurance Storage Service", tandis que Performance s'affichera en tant que "Performance Storage Service". Le taux variera en fonction de votre niveau de stockage. Vous pouvez ensuite développer Endurance ou Performance pour voir qu'il s'agit de {{site.data.keyword.blockstorageshort}}.
