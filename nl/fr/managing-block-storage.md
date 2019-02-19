---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestion de {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

Vous pouvez gérer vos volumes {{site.data.keyword.blockstoragefull}} via le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}.

## Affichage des détails de numéro d'unité logique {{site.data.keyword.blockstorageshort}}

Vous pouvez afficher un récapitulatif des principales informations du numéro d'unité logique de stockage sélectionné, notamment les fonctions supplémentaires d'instantané et de réplication qui ont été ajoutées au stockage.

1. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur le nom LUN approprié dans la liste.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## Autorisation des hôtes pour l'accès à {{site.data.keyword.blockstorageshort}}

Les hôtes "autorisés" sont des hôtes auxquels des droits d'accès à un numéro d'unité logique particulier ont été accordés. Sans autorisation d'hôte, vous ne pouvez pas accéder au stockage ni l'utiliser depuis votre système. L'autorisation d'un hôte pour accéder à votre numéro d'unité logique génère le nom d'utilisateur, le mot de passe et le nom qualifié iSCSI, qui sont nécessaires pour monter la connexion iSCSI d'E-S multi-accès.

Vous pouvez autoriser et connecter des hôtes qui se trouvent dans le même centre de données que votre stockage. Si vous pouvez disposer de plusieurs comptes, vous ne pouvez pas autoriser un hôte à partir d'un compte à accéder à votre stockage sur un autre compte.
{:important}

1. Cliquez sur **Stockage** -> **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom LUN.
2. Faites défiler la page jusqu'à la section** Hôtes autorisés**.
3. A droite, cliquez sur **Hôte autorisé**. Sélectionnez les hôtes qui peuvent accéder à ce numéro d'unité logique spécifique.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

## Affichage de la liste des hôtes autorisés à accéder à un numéro d'unité logique {{site.data.keyword.blockstorageshort}}

1. Cliquez sur **Stockage** -> **{{site.data.keyword.blockstorageshort}}**, puis cliquez sur votre nom LUN.
2. Faites défiler l'écran jusqu'à la section **Hôtes autorisés**.

Cette section affiche la liste des hôtes actuellement autorisés à accéder au numéro d'unité logique. Sont également affichées les informations d'authentification nécessaires pour établir une connexion : nom d'utilisateur, mot de passe et nom qualifié iSCSI hôte. L'adresse cible figure sur la page contenant les détails du stockage. Pour NFS, elle est décrite sous forme de DNS, tandis que pour iSCSI, il s'agit de l'adresse IP du portail cible Discover.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## Affichage du service {{site.data.keyword.blockstorageshort}} auquel un hôte est autorisé à accéder

Vous pouvez afficher les numéros d'unité logique auxquels un hôte a accès, notamment les informations nécessaires pour établir une connexion (Nom LUN, Type de stockage, Adresse cible, capacité et emplacement) :

1. Cliquez sur **Périphériques** -> **Liste des périphériques** dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://control.softlayer.com/){:new_window}, puis cliquez sur le périphérique approprié.
2. Sélectionnez l'onglet **Stockage**.

Vous voyez ensuite s'afficher la liste des numéros d'unité logique de stockage auxquels cet hôte spécifique a accès. La liste est regroupée par type de stockage (bloc, fichier, autre). Vous pouvez autoriser davantage de stockage ou supprimer l'accès en cliquant sur **Actions**.

## Montage et démontage de {{site.data.keyword.blockstorageshort}}

En fonction du système d'exploitation de votre hôte, suivez les instructions appropriées.

- [Connexion à des numéros d'unité logique (LUN) sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connexion à des numéros d'unité logique (LUN) sous CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connexion à des numéros d'unité logique (LUN) sous Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuration de Block Storage pour une sauvegarde avec cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuration de Block Storage pour une sauvegarde avec Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## Révocation de l'accès d'un hôte à {{site.data.keyword.blockstorageshort}}

Si vous souhaitez ne plus autoriser un hôte à accéder à un numéro d'unité logique de stockage en particulier, vous pouvez révoquer l'accès. Lors de la révocation de l'accès, la connexion hôte est supprimée du numéro d'unité logique. Le système d'exploitation et les applications sur cet hôte ne peuvent plus communiquer avec le numéro d'unité logique.

Pour empêcher tout problème côté hôte, démontez le numéro d'unité logique de stockage de votre système d'exploitation avant de révoquer l'accès afin de prévenir tout incident lié à des unités manquantes ou à l'altération des données.
{:important}

Vous pouvez révoquer l'accès à partir de la **Liste des unités** ou de la **vue Stockage**.

### Révocation de l'accès à partir de la liste des unités

1. Cliquez sur **Périphériques**, **Liste des périphériques** dans le portail [{{site.data.keyword.slportal}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://control.softlayer.com/){:new_window}, puis cliquez deux fois sur le périphérique approprié.
2. Sélectionnez l'onglet **Stockage**.
3. Vous voyez ensuite s'afficher la liste des numéros d'unité logique de stockage auxquels cet hôte spécifique a accès. La liste est regroupée par type de stockage (bloc, fichier, autre). En regard du nom LUN, sélectionnez **Action**, puis cliquez sur **Révoquer le droit d'accès**.
4. Confirmez l'action car elle ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès d'un numéro d'unité logique, ou sur **Non** pour annuler l'action.

Si vous souhaitez déconnecter plusieurs numéros d'unité logique d'un hôte spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque LUN.
{:tip}


### Révocation de l'accès à partir de la vue Stockage

1. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}**, puis sélectionnez le numéro d'unité logique dont vous souhaitez révoquer l'accès.
2. Faites défiler la page jusqu'à **Hôtes autorisés**.
3. Cliquez sur **Actions** en regard de l'hôte dont l'accès doit être révoqué, puis sélectionnez **Révoquer le droit d'accès**.
4. Confirmez l'action car elle ne peut pas être annulée. Cliquez sur **Oui** pour révoquer l'accès d'un numéro d'unité logique, ou sur **Non** pour annuler l'action.

Si vous souhaitez déconnecter plusieurs hôtes d'un numéro d'unité logique spécifique, vous devez répéter l'action Révoquer le droit d'accès pour chaque hôte.
{:tip}

### Révocation de l'accès via l'interface SLCLI.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to revoke
                            authorization
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to revoke
                            authorization
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to revoke authorization
  --ip-address TEXT         An IP address to revoke authorization
  --help                    Show this message and exit.
```

## Annulation d'un numéro d'unité logique de stockage

Lorsque vous n'avez plus besoin d'un numéro d'unité logique spécifique, vous pouvez l'annuler à tout moment.

Pour ce faire, vous devez d'abord révoquer l'accès à partir de tous les hôtes.
{:important}

1. Cliquez sur **Stockage**, **{{site.data.keyword.blockstorageshort}}**.
2. Cliquez sur **Actions** en regard du numéro d'unité logique à annuler, puis sélectionnez **Annuler stockage par blocs**.
3. Confirmez l'annulation du numéro d'unité logique immédiatement ou à la date anniversaire de sa mise à disposition.

   Si vous sélectionnez l'option d'annulation du numéro d'unité logique à sa date anniversaire, vous pouvez annuler la demande d'annulation avant sa date anniversaire.
   {:tip}
4. Cliquez sur **Continuer** ou sur **Fermer**.
5. Cochez la case d'accusé de réception et cliquez sur **Confirmer**.

Vous pouvez également utiliser la commande suivante dans l'interface SLCLI.
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```
