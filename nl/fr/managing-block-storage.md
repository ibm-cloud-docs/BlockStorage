---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestion de {{site.data.keyword.blockstorageshort}}

Vous pouvez gérer vos volumes {{site.data.keyword.blockstoragefull}} via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. Cet article fournit des instructions pour les tâches les plus courantes.

## Affichage des détails d'un numéro d'unité logique {{site.data.keyword.blockstorageshort}} mis à disposition

Vous pouvez afficher un récapitulatif des principales informations du numéro d'unité logique de stockage sélectionné, notamment les fonctions supplémentaires d'instantané et de réplication qui ont été ajoutées au stockage.

1. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur le nom LUN approprié dans la liste.

## Autorisation d'accès à {{site.data.keyword.blockstorageshort}} pour des hôtes

Les hôtes "autorisés" sont des hôtes qui disposent de droits d'accès à un numéro d'unité logique spécifique. Sans autorisation de l'hôte, vous ne pouvez pas accéder au stockage à partir de votre système, ni l'utiliser. L'autorisation d'un hôte pour accéder à votre numéro d'unité logique génère le nom d'utilisateur, le mot de passe et le nom qualifié iSCSI, qui est nécessaire pour monter la connexion iSCSI d'E-S multi-accès.

**Remarque** : Vous pouvez uniquement autoriser et connecter des hôtes qui se trouvent dans le même centre de données que votre stockage. Si vous disposez de plusieurs comptes, vous ne pouvez pas autoriser un hôte à partir d'un compte à accéder à votre stockage sur un autre compte.

1. Cliquez sur **Stockage** -> **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom LUN.
2. Faites défiler la page jusqu'à la section Hôtes autorisés.
3. Cliquez sur le lien **Hôte autorisé** dans la partie droite de la page. Sélectionnez les hôtes qui peuvent accéder à ce numéro d'unité logique spécifique.

 

## Affichage de la liste des hôtes autorisés à accéder à un numéro d'unité logique {{site.data.keyword.blockstorageshort}}

Procédez comme suit pour afficher la liste des hôtes autorisés à accéder à un numéro d'unité logique.

1. Cliquez sur **Stockage** -> **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom LUN.
2. Faites défiler la page vers le bas jusqu'à la section Hôtes autorisés.

La liste des hôtes actuellement autorisés à accéder au numéro d'unité logique s'affiche ici et, en particulier pour iSCSI, les informations d'authentification nécessaires pour établir une connexion : Nom d'utilisateur, Mot de passe et Nom qualifié iSCSI hôte. L'adresse cible figure en haut de la page contenant les détails du stockage. Pour NFS, elle est décrite sous forme de DNS, tandis que pour iSCSI, il s'agit de l'adresse IP du portail cible Discover.

 

## Affichage du service {{site.data.keyword.blockstorageshort}} auquel un hôte est autorisé à accéder

Vous pouvez afficher les numéros d'unité logique auxquels un hôte a accès, notamment les informations nécessaires pour établir une connexion (Nom LUN, Type de stockage, Adresse cible, capacité et emplacement) :

1. Cliquez sur **Périphériques** -> **Liste des unités** dans le portail [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window}, puis cliquez sur le périphérique approprié.
2. Sélectionnez l'onglet **Stockage**.

Vous verrez ensuite s'afficher la liste des numéros d'unité logique de stockage auxquels cet hôte spécifique a accès, regroupés par type de stockage (bloc, fichier, autre). A partir des menus Action respectifs, vous pouvez autoriser un stockage supplémentaire ou supprimer un accès.

 

## Montage et démontage de {{site.data.keyword.blockstorageshort}}

Consultez les articles suivants, qui contiennent des détails sur le montage et le démontage de {{site.data.keyword.blockstorageshort}} à partir d'un hôte.

- [{{site.data.keyword.blockstorageshort}} sous Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} sous Microsoft Windows](accessing-block-storage-windows.html)

 

## Révocation de l'accès d'un hôte à {{site.data.keyword.blockstorageshort}}

Pour qu'un hôte n'ait plus accès à un numéro d'unité logique spécifique, vous pouvez révoquer son accès. Lors de la révocation de l'accès, la connexion hôte sera supprimée du numéro d'unité logique et ni le système d'exploitation, ni les applications ne pourront communiquer avec le numéro d'unité logique.

**Remarque** : Pour empêcher tout problème côté hôte, démontez le numéro d'unité logique de stockage de votre système d'exploitation avant de révoquer l'accès afin d'éviter qu'il manque des unités ou que des données soient altérées.

Vous pouvez révoquer l'accès à partir de l'option Stockage des vues Liste des unités ou Stockage.

### Révocation de l'accès à partir de la vue Liste des unités :

1. Cliquez sur **Périphériques**, **Liste des unités** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puis cliquez deux fois sur le périphérique approprié.
2. Sélectionnez l'onglet **Stockage**.
3. Vous verrez ensuite s'afficher la liste des numéros d'unité logique de stockage auxquels cet hôte spécifique a accès, regroupés par type de stockage (bloc, fichier, autre). Sélectionnez le menu Action respectif en regard du numéro d'unité logique dont vous voulez révoquer l'accès, puis cliquez sur **Révoquer le droit d'accès**.
4. Une confirmation vous sera demandée car l'action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès d'un numéro d'unité logique, ou sur **Non** pour annuler l'action.

**Remarque** : Si vous souhaitez déconnecter plusieurs numéros d'unité logique d'un hôte spécifique, vous devrez répéter l'action Révoquer le droit d'accès pour chaque LUN.


### Révocation de l'accès à partir de la vue Stockage :

1. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}**, puis sélectionnez le numéro d'unité logique dont vous souhaitez révoquer l'accès.
2. Faites défiler la page vers le bas jusqu'à la section** Hôtes autorisés**.
3. Cliquez sur la flèche déroulante **Actions** en regard de l'hôte dont l'accès doit être révoqué, puis sélectionnez **Révoquer le droit d'accès**.
4. Une confirmation vous sera demandée car l'action ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès d'un numéro d'unité logique, ou sur **Non** pour annuler l'action.

**Remarque** : Si vous souhaitez déconnecter plusieurs hôtes d'un numéro d'unité logique spécifique, vous devrez répéter l'action Révoquer le droit d'accès pour chaque hôte.

 

## Annulation d'un numéro d'unité logique de stockage

Si vous n'avez plus besoin d'un numéro d'unité logique spécifique, vous pouvez l'annuler. Pour ce faire, vous devez d'abord révoquer l'accès à partir des hôtes.

1. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur la flèche déroulante **Actions** en regard du numéro d'unité logique à annuler, puis sélectionnez **Annuler stockage par blocs**.
3. Vous serez invité à confirmer l'annulation du numéro d'unité logique immédiatement ou à la date anniversaire de sa mise à disposition. Cliquez sur **Continuer** ou sur **Fermer**.
**Remarque** : Si vous sélectionnez l'option d'annulation du numéro d'unité logique à sa date anniversaire, vous pouvez annuler la demande d'annulation avant sa date anniversaire.
4. Cochez la case d'accusé de réception et cliquez sur **Confirmer**.

 

