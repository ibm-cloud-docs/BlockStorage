---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block storage, Plesk, backups, mountpoint, ISCSI

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuration de {{site.data.keyword.blockstorageshort}} en vue de la sauvegarde avec Plesk
{: #PleskBackups}

Suivez les instructions suivantes pour configurer {{site.data.keyword.blockstoragefull}} pour vos sauvegardes dans Plesk. Cela suppose que vous disposiez d'un accès racine ou sudo SSH et d'un niveau d'administrateur Plesk complet. Ces instructions se fondent sur un hôte CentOS7.

Pour plus d'informations, voir la [documentation relative à Plesk pour la sauvegarde et la restauration ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Connectez-vous à l'hôte via SSH.
2. Vérifiez qu'il existe un point de montage cible.

   Plesk dispose de deux options pour stocker les sauvegardes. L'une correspond au stockage Plesk interne (stockage situé sur votre serveur Plesk). L'autre correspond à un stockage FTP externe (stockage situé sur un serveur externe sur le Web ou sur votre réseau local). En général, dans les environnements Plesk, les sauvegardes internes sont stockées dans `/var/lib/psa/dumps` et utilisent `/tmp` comme répertoire temporaire. Dans cet exemple, le répertoire temporaire est conservé en local, mais le répertoire dumps est déplacé vers le répertoire cible {{site.data.keyword.blockstorageshort}} (`/backup/psa/dumps`). Aucune donnée d'identification utilisateur FTP n'est requise.
   {:note}   
3. Configurez {{site.data.keyword.blockstorageshort}} comme décrit dans [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html). Montez {{site.data.keyword.blockstorageshort}} dans `/backup` et configurez `/etc/fstab` pour activer le montage à l'amorçage.
4. **Facultatif** : Copiez les sauvegardes existantes dans le nouveau stockage. Vous pouvez utiliser `rsync`.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    Cette commande compresse et transmet vos données tout en les conservant autant que possible (à l'exception des liens fixes). Elle fournit des informations sur les fichiers transférés, ainsi qu'un bref récapitulatif à la fin.
    {:tip}    
5. Editez `/etc/psa/psa.conf` pour qu'il pointe la valeur `DUMP_D` sur la nouvelle cible.
    - Résultat : `DUMP_D /backup/psa/dumps`.
6. **Facultatif** : En fonction de votre cas d'utilisation et de vos besoins métier particuliers, supprimez l'ancien stockage du serveur et annulez-le sur le compte.
