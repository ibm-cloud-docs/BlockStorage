---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configuration de {{site.data.keyword.blockstorageshort}} pour une sauvegarde avec cPanel

Cet article vous aidera à configurer vos sauvegardes dans cPanel en vue d'un stockage dans {{site.data.keyword.blockstoragefull}}. Nous supposons ici que root ou sudo SSH et un accès complet à WebHost Manager (WHM) sont disponibles. Nos instructions se fondent sur un hôte **CentOS 7**. 

**Remarque** : Vous trouverez la documentation relative à cPanel pour la **configuration du répertoire de sauvegarde ** [ici](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Connectez-vous à l'hôte via SSH.

2. Vérifiez qu'il existe un point de montage cible. <br />
   **Remarque** : Par défaut, le système cPanel enregistre les fichiers de sauvegarde localement, dans le répertoire `/backup`. Dans ce document, nous supposons que le répertoire `/backup` existe et contient des sauvegardes. Nous utiliserons donc `/backup2` comme nouveau point de montage.
   
3. Configurez {{site.data.keyword.blockstorageshort}} comme décrit dans [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html). Vérifiez que vous le montez vers `/backup2` et configurez-le dans `/etc/fstab` pour autoriser le montage à l'amorçage.

4. **Facultatif** : Copiez les sauvegardes existantes dans le nouveau stockage. Vous pouvez utiliser `rsync` :
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Remarque** : Cette commande transmet vos données compressées tout en les conservant autant que possible, à l'exception des liens fixes. Elle fournit des informations sur les fichiers transférés, ainsi qu'un bref récapitulatif à la fin. 
    
5. Connectez-vous à WebHost Manager et accédez à la configuration de sauvegarde en cliquant sur les options **Home** > **Backup** > **Backup Configuration**.

6. Editez la configuration pour enregistrer les sauvegardes dans le nouveau point de montage. 
    - Modifiez le répertoire de sauvegarde par défaut en remplaçant le chemin d'accès absolu au répertoire /backup/ par le nouvel emplacement. 
    - Sélectionnez **Enable to mount a backup drive**. Ce paramètre permet la recherche d'un montage de sauvegarde (`/backup2`) dans le fichier `/etc/fstab` lors du processus de configuration de sauvegarde. <br /> **Remarque** : S'il existe un montage portant le même nom que le répertoire de transfert, le processus de configuration de sauvegarde monte l'unité et sauvegarde les informations sur le montage. Une fois le processus de sauvegarde terminé, l'unité est démontée. 

7. Appliquez les modifications en cliquant sur **Save Configuration** au bas de l'interface **Backup Configuration**.

8. **Facultatif** : En fonction de votre cas d'utilisation et de vos besoins métier particuliers, supprimez l'ancien stockage du serveur et annulez-le sur le compte.

