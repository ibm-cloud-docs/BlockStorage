---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuration de {{site.data.keyword.blockstorageshort}} pour une sauvegarde avec cPanel
{: #cPanelBackups}

Suivez les instructions suivantes pour configurer vos sauvegardes dans cPanel en vue d'un stockage dans {{site.data.keyword.blockstoragefull}}. Nous supposons ici que root ou sudo SSH et un accès complet à WebHost Manager (WHM) sont disponibles. Ces instructions se fondent sur un hôte **CentOS 7**.

Pour plus d'informations, voir [cPanel - Configuration du répertoire de sauvegarde ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.
{:tip}

1. Connectez-vous à l'hôte via SSH.

2. Vérifiez qu'il existe un point de montage cible. <br />
   Par défaut, le système cPanel enregistre les fichiers de sauvegarde localement, dans le répertoire `/backup`. Dans ce document, nous supposons que le répertoire `/backup` existe et contient des sauvegardes et `/backup2` est utilisé comme nouveau point de montage.
   {:note}

3. Configurez {{site.data.keyword.blockstorageshort}} comme décrit dans [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Prenez soin de le monter dans `/backup2` et de configurer dans `/etc/fstab` pour autoriser le montage à l'amorçage.

4. **Facultatif** : Copiez les sauvegardes existantes dans le nouveau stockage. Vous pouvez utiliser `rsync`.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Cette commande compresse et transmet vos données tout en les conservant autant que possible (à l'exception des liens fixes). Elle fournit des informations sur les fichiers transférés, ainsi qu'un bref récapitulatif à la fin.
    {:tip}

5. Connectez-vous à WHM et accédez à la configuration de sauvegarde en cliquant sur **Home** > **Backup** > **Backup Configuration**.

6. Editez la configuration pour sauvegarder les sauvegardes dans le nouveau point de montage.
    - Modifiez le répertoire de sauvegarde par défaut en remplaçant le chemin d'accès absolu au répertoire /backup/ par le nouvel emplacement.
    - Sélectionnez **Enable to mount a backup drive**. Ce paramètre permet la recherche d'un montage de sauvegarde (`/backup2`) dans le fichier `/etc/fstab` lors du processus de configuration de sauvegarde. <br />

    S'il existe un montage portant le même nom que le répertoire de transfert, le processus de configuration de sauvegarde monte l'unité et sauvegarde les informations sur l'unité. Une fois le processus de sauvegarde terminé, il démonte l'unité.
    {:note}

7. Appliquez les modifications en cliquant sur **Save Configuration**.

8. **Facultatif** : En fonction de votre cas d'utilisation et de vos besoins métier particuliers, supprimez l'ancien stockage du serveur et annulez-le sur le compte.
