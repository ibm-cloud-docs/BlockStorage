---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configuration de {{site.data.keyword.blockstorageshort}} pour une sauvegarde avec Plesk

Cet article a pour but de vous fournir des instructions pour configurer {{site.data.keyword.blockstoragefull}} pour des sauvegardes dans Plesk. Nous supposons ici que root ou sudo SSH et un accès complet au niveau administrateur de Plesk sont disponibles. Cet exemple se fonde sur un hôte CentOS 7. 

**Remarque** : Vous trouverez la documentation relative à Plesk pour la sauvegarde et la restauration [ici](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}. 

1. Connectez-vous à l'hôte via SSH. 

2. Vérifiez qu'il existe un point de montage cible. <br />
   **Remarque** : Plesk dispose de deux options pour stocker les sauvegardes : l'une est le stockage Plesk interne - un stockage de sauvegarde situé sur votre serveur Plesk, tandis que l'autre est un stockage FTP externe - un stockage de sauvegarde situé sur un serveur externe sur le Web ou votre réseau local. En général, dans les environnements Plesk, les sauvegardes internes sont stockées dans `/var/lib/psa/dumps` et utilisent `/tmp` comme répertoire temporaire. Dans notre exemple, nous conservons le répertoire temporaire local, mais déplaçons le répertoire de vidage vers la cible STaaS (`/backup/psa/dumps`). Aucune donnée d'identification utilisateur FTP n'est requise. 
   
3. Configurez {{site.data.keyword.blockstorageshort}} comme décrit dans [Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux](accessing_block_storage_linux.html). Montez {{site.data.keyword.blockstorageshort}} vers `/backup` et configurez `/etc/fstab` pour autoriser le montage à l'amorçage. 

4. **Facultatif** : Copiez les sauvegardes existantes dans le nouveau stockage. Utilisez `rsync` par exemple :
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Remarque** : Cette commande transmet vos données compressées tout en les conservant autant que possible (à l'exception des liens fixes), et fournit des informations sur les fichiers transférés, ainsi qu'un bref récapitulatif à la fin. 
    
5. Editez `/etc/psa/psa.conf` pour qu'il pointe la valeur `DUMP_D` sur la nouvelle cible.  
    -  Il doit se présenter comme suit : `DUMP_D /backup/psa/dumps`.  

6. **Facultatif** : En fonction de votre cas d'utilisation et de vos besoins métier particuliers, supprimez l'ancien stockage du serveur et annulez-le sur le compte. 


