---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation de LUKS dans Red Hat Enterprise Linux 6 pour un chiffrement de disque complet

Linux Unified Key Setup-on-disk-format (LUKS) vous permet de chiffrer des partitions sous Red Hat Enterprise Linux 6 (serveur), ce qui est particulièrement important lorsqu'il s'agit d'ordinateurs portables et de support amovible. LUKS permet à plusieurs clés d'utilisateur de déchiffrer une clé principale utilisée pour le chiffrement en bloc de la partition.

## Opérations possibles avec LUKS

- Chiffrement de la totalité des unités par bloc et parfaite adaptation à la protection du contenu des périphériques mobiles tels qu'un support de stockage amovible ou des unités de disque d'ordinateur portable.
    - Le contenu sous-jacent de l'unité par bloc chiffrée est arbitraire, ce qui le rend utile pour le chiffrement des unités de permutation. Le chiffrement peut également être utile avec certaines bases de données qui utilisent des unités par bloc spécialement formatées pour le stockage des données.
- Utilisation du sous-système de noyaux d'associateur de périphériques existant.
- Renforcement de la sécurité avec une phrase passe, qui fournit une protection contre les ajouts de dictionnaire sous forme de pièces jointes.
- Possibilité pour les utilisateurs d'ajouter des clés de sauvegarde ou des phrases passe car les unités LUKS contiennent plusieurs emplacements de clé.


## Opérations impossibles avec LUKS

- Possibilité pour les applications nécessitant un grand nombre d'utilisateurs (plus de huit) d'avoir des clés distinctes pour accéder aux mêmes unités.
- Utilisation d'applications nécessitant un chiffrement au niveau fichier ([en savoir plus](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}).

## Configuration d'un nouveau volume chiffré LUKS avec Endurance {{site.data.keyword.blockstorageshort}}

Cette procédure suppose que le serveur dispose déjà d'un accès à un nouveau volume {{site.data.keyword.blockstoragefull}}, non chiffré, qui n'a été ni formaté, ni monté. Cliquez [ici](accessing_block_storage_linux.html) pour savoir comment accéder à {{site.data.keyword.blockstorageshort}} avec Linux.

Notez qu'un chiffrement de données crée une charge sur l'hôte, qui risque d'impacter les performances.

1. Saisissez la commande suivante à une invite shell en tant que root pour installer le package requis :   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Obtenez l'ID de disque :<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Localisez votre volume dans la liste.
4. Chiffrez l'unité par bloc : 

   1. Cette commande initialise le volume et vous permet de définir une phrase passe : <br/>
   
      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}
      
   2. Répondez par YES (tout en majuscules).
   
   3. L'unité apparaît maintenant sous forme de volume chiffré : 
   
      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```
      
5. Ouvrez le volume et créez un mappage :   <br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Saisissez le mot de passe fourni précédemment.
7. Vérifiez le mappage et affichez le statut du volume chiffré :   <br/>
   ```
   # cryptsetup -v status cryptData
   /dev/mapper/cryptData is active.
     type:  LUKS1
     cipher:  aes-cbc-essiv:sha256
     keysize: 256 bits
     device:  /dev/mapper/3600a0980383034685624466470446564
     offset:  4096 sectors
     size:    41938944 sectors
     mode:    read/write
     Command successful
   ```
8. Ecrivez des données aléatoires sur l'unité chiffrée /dev/mapper/cryptData. Le monde extérieur les considérera ainsi comme des données aléatoires, ce qui signifie qu'elles seront protégées contre la divulgation des modèles d'utilisation. Sachez que cette étape peut prendre un certain temps.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Formatez le volume :<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Montez le volume :<br/>
   ```
   # mkdir /cryptData
   ```
   {: pre}
   ```
   # mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}
   ```
   # df -H /cryptData
   ```
   {: pre}

### Démontage et fermeture du volume chiffré en toute sécurité
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Remontage et montage d'une partition chiffrée LUKS existante
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   # mount /dev/mapper/cryptData /cryptData
   # df -H /cryptData
   # lsblk
   NAME                                       MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
   xvdb                                       202:16   0    2G  0 disk
   └─xvdb1                                    202:17   0    2G  0 part  [SWAP]
   xvda                                       202:0    0   25G  0 disk
   ├─xvda1                                    202:1    0  256M  0 part  /boot
   └─xvda2                                    202:2    0 24.8G  0 part  /
   sda                                          8:0    0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   └─3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   └─cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   ```
