---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Connexion à des numéros d'unité logique (LUN) MPIO iSCSI sous Linux

Ces instructions s'appliquent principalement à RHEL6 et Centos6. Des remarques pour les autres systèmes d'exploitation ont été ajoutées, mais cette documentation **NE COUVRE PAS** toutes les distributions Linux. Si vous utilisez d'autres systèmes d'exploitation Linux, consultez la documentation de votre distribution spécifique et vérifiez que le multi-accès prend en charge ALUA pour la priorité des chemins.
{:note}

Par exemple, vous pouvez trouver les instructions d'Ubuntu pour la configuration de l'initiateur iSCSI [ici](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} et la configuration DM-Multipath [ici](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.
{:tip}

Avant de commencer, assurez-vous que les droits d'accès nécessaires ont été affectés via le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} à l'hôte qui accède au volume {{site.data.keyword.blockstoragefull}}.
{:important}

1. Sur la page de liste {{site.data.keyword.blockstorageshort}}, repérez le nouveau volume et cliquez sur **Actions**.
2. Cliquez sur **Hôte autorisé**.
3. Dans la liste, sélectionnez l'hôte ou les hôtes qui peuvent accéder au volume et cliquez sur **Soumettre**.

## Montage de volumes {{site.data.keyword.blockstorageshort}}

Vous trouverez ci-dessous la procédure requise pour connecter une instance de calcul {{site.data.keyword.BluSoftlayer_full}} basée sur Linux à un numéro d'unité logique (LUN) d'E-S multi-accès (MPIO) d'interface SCSI (iSCSI).

Le nom qualifié iSCSI hôte, le nom d'utilisateur, le mot de passe et l'adresse cible qui sont référencés dans les instructions peuvent être obtenus à partir de l'écran **Détails {{site.data.keyword.blockstorageshort}}** dans le portail [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
{: tip}

Il est recommandé d'exécuter le trafic de stockage sur un réseau local virtuel qui ignore le pare-feu. L'exécution du trafic de stockage via des pare-feu logiciels augmente le temps d'attente et a un impact négatif sur les performances de stockage.
{:important}

1. Installez les utilitaires iSCSI et multi-accès sur votre hôte.
  - RHEL et CentOS
     ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu et Debian

    ```
    sudo apt-get update
   sudo apt-get install multipath-tools
    ```
    {: pre}

2. Créez ou éditez votre fichier de configuration multi-accès s'il est requis.
  - RHEL 6 et CENTOS 6
    * Editez **/etc/multipath.conf** avec la configuration minimale suivante :

      ```
      defaults {
      user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
      # All data under blacklist must be specific to your system.
      blacklist {
      wwid "SAdaptec*"
   devnode "^hd[a-z]"
   devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]*"
      devnode "^cciss.*"  
      }
      devices {
      device {
      vendor "NETAPP"
   product "LUN"
   path_grouping_policy group_by_prio
   features "3 queue_if_no_path pg_init_retries 50"
   prio "alua"
   path_checker tur
   failback immediate
   path_selector "round-robin 0"
   hardware_handler "1 alua"
   rr_weight uniform
   rr_min_io 128
   }
      }
      ```
      {: codeblock}

    - Redémarrez les services iscsi et iscsid de sorte que les modifications prennent effet. 

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - Pour RHEL7 et CentOS7, `multipath.conf` peut être vierge car le système d'exploitation contient des configurations intégrées. 
  - Ubuntu n'utilise pas `multipath.conf` car il est intégré dans `multipath-tools`.

3. Chargez le module multi-accès, démarrez les services multi-accès et définissez-le pour qu'il démarre à l'amorçage.
  - RHEL 6
    ```
    modprobe dm-multipath
    ```
    {: pre}

    ```
    service multipathd start
    ```
    {: pre}

    ```
    chkconfig multipathd on
    ```
    {: pre}

  - CentOS 7
    ```
    modprobe dm-multipath
    ```
    {: pre}

    ```
    systemctl start multipathd
    ```
    {: pre}

    ```
    systemctl enable multipathd
    ```
    {: pre}

  - Ubuntu
    ```
    service multipath-tools start
    ```
    {: pre}

  - Pour les autres distributions, consultez la documentation du fournisseur du système d'exploitation.

4. Vérifiez que le multi-accès fonctionne.
  - RHEL 6
    ```
    multipath -l
    ```
    {: pre}

    Si aucune donnée n'est renvoyée, cela signifie qu'il fonctionne.
  - CentOS 7
    ```
    multipath -ll
    ```
    {: pre}

    RHEL 7 et CentOS 7 peuvent renvoyer le message No fc_host device, qui peut être ignoré. 

5. Mettez à jour le fichier `/etc/iscsi/initiatorname.iscsi` avec le nom qualifié iSCSI provenant du portail {{site.data.keyword.slportal}}. Saisissez la valeur en minuscules.
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. Editez les paramètres CHAP dans le fichier `/etc/iscsi/iscsid.conf` à l'aide du nom d'utilisateur et du mot de passe provenant du portail {{site.data.keyword.slportal}}. Utilisez des majuscules pour les noms CHAP.
   ```
   node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   Laissez les autres paramètres CHAP en commentaire. Le stockage {{site.data.keyword.BluSoftlayer_full}} utilise uniquement l'authentification unidirectionnelle. N'activez pas l'authentification CHAP mutuelle.
   {:important}

7. Définissez iSCSI pour qu'il démarre à l'amorçage et démarrez-le maintenant.
  - RHEL 6
    ```
    chkconfig iscsi on
    ```
    {: pre}

    ```
    chkconfig iscsid on
    ```
    {: pre}

    ```
    service iscsi start
    ```
    {: pre}

    ```
    service iscsid start
    ```
    {: pre}

  - CentOS 7
    ```
    systemctl enable iscsi
    ```
    {: pre}

    ```
    systemctl enable iscsid
    ```
    {: pre}

    ```
    systemctl start iscsi
    ```
    {: pre}

    ```
    systemctl start iscsid
    ```
    {: pre}

   - Autres distributions : consultez la documentation du fournisseur du système d'exploitation.

8. Reconnaissez le périphérique à l'aide de l'adresse IP cible obtenue à partir du portail {{site.data.keyword.slportal}}.

   A. Exécutez la reconnaissance sur la grappe iSCSI.
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
    ```
    {: pre}

   B. Définissez l'hôte pour qu'il se connecte automatiquement à la grappe iSCSI.
    ```
    iscsiadm -m node -L automatic
    ```
    {: pre}

9. Vérifiez que l'hôte est connecté à la grappe iSCSI et a conservé ses sessions.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Cette commande affiche les chemins d'accès.

10. Vérifiez que le périphérique est connecté. Par défaut, le périphérique se connecte à `/dev/mapper/mpathX`, où X correspond à l'ID généré du périphérique connecté.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Cette commande génère une sortie semblable à l'exemple ci-après.
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```
  Le volume est maintenant monté et accessible sur l'hôte.

## Création d'un système de fichiers (facultatif)

Procédez comme indiqué ci-après pour créer un système de fichiers sur le volume récemment monté. Un système de fichiers est nécessaire pour permettre à la plupart des applications d'utiliser le volume. Utilisez `fdisk` pour les unités inférieures à 2 To et `parted` pour un disque supérieur à 2 To.

### Création d'un système de fichiers avec `fdisk`

1. Obtenez le nom du disque.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   Le nom de disque renvoyé doit être similaire à `/dev/mapper/XXX`.

2. Créez une partition sur le disque.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX représente le nom de disque renvoyé à l'étape 1. <br />

   Faites défiler l'écran vers le bas pour afficher les codes de commande répertoriés dans le tableau de la commande `fdisk`.{: tip}

3. Créez un système de fichiers sur la nouvelle partition.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - La nouvelle partition est répertoriée avec le disque, semblable à `XXXlp1`, suivie de la taille, du type (83) et de Linux.
   - Notez le nom de la partition, car vous en aurez besoin à l'étape suivante. (XXXlp1 représente le nom de la partition.)
   - Créez le système de fichiers :

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Créez un point de montage pour le système de fichiers et montez-le.
   - Créez un nom de partition `PerfDisk` ou un emplacement où monter le système de fichiers.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Montez le stockage avec le nom de partition.
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Vérifiez que votre nouveau système de fichiers est répertorié.
     ```
     df -h
     ```
     {: pre}

5. Ajoutez le nouveau système de fichiers au fichier `/etc/fstab` du système pour activer le montage automatique à l'amorçage.
   - Ajoutez la ligne suivante à la fin du fichier `/etc/fstab` (en utilisant le nom de partition provenant de l'étape 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Tableau de la commande `fdisk`

<table border="0" cellpadding="0" cellspacing="0">
	<caption>Le tableau de la commande <code>fdisk</code> contient les commandes à gauche et les résultats attendus à droite.</caption>
    <thead>
	<tr>
		<th style="width:40%;">Commande</th>
		<th style="width:60%;">Résultat</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Command: n</code></td>
		<td>Crée une partition. &#42;</td>
	</tr>
	<tr>
		<td><code>Command action: p</code></td>
		<td>En fait la partition principale.</td>
	</tr>
	<tr>
		<td><code>Partition number (1-4): 1</code></td>
		<td>Devient la partition 1 sur le disque.</td>
	</tr>
	<tr>
		<td><code>First cylinder (1-8877): 1 (default)</code></td>
		<td>Démarre au cylindre 1.</td>
	</tr>
	<tr>
		<td><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></td>
		<td>Appuyez sur Entrée pour aller au dernier cylindre.</td>
	</tr>
	<tr>
		<td><code>Command: t</code></td>
		<td>Configure le type de partition. &#42;</td>
	</tr>
	<tr>
		<td><code>Select partition 1.</code></td>
		<td>Sélectionne la partition 1 pour la configurer en tant que type spécifique.</td>
	</tr>
	<tr>
		<td><code>Hex code: 83</code></td>
		<td>Sélectionne Linux comme type (83 est le code hexadécimal pour Linux).&#42;&#42;</td>
	 </tr>
	<tr>
		<td><code>Command: w</code></td>
		<td>Ecrit les informations de la nouvelle partition sur le disque. &#42;</td>
	</tr>
   </tbody>
</table>

  (`*`)Saisissez m pour obtenir de l'aide.

  (`**`)Saisissez L pour obtenir la liste des codes hexadécimaux.

### Création d'un système de fichiers avec `parted`

Sur de nombreuses distributions Linux, `parted` est préinstallé. S'il n'est pas inclus dans votre distribution, vous pouvez l'installer avec :
- Debian et Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL et CentOS
  ```
  yum install parted  
  ```
  {: pre}


Pour créer un système de fichiers avec `parted`, procédez comme suit :

1. Exécutez `parted`.

   ```
   parted
   ```
   {: pre}

2. Créez une partition sur le disque.
   1. Sauf indication contraire, `parted` utilise votre unité principale, qui est dans la plupart des cas `/dev/sda`. Basculez vers le disque que vous souhaitez partitionner à l'aide de la commande **select**. Remplacez **XXX** par le nom de votre nouveau périphérique.

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Exécutez `print` pour confirmer que vous êtes sur le disque approprié.

      ```
      print
      ```
      {: pre}

   3. Créez une table de partition GPT.

      ```
      mklabel gpt
      ```
      {: pre}

   4. `Parted` peut être utilisé pour créer des partitions de disque logiques et primaires, car les procédures sont identiques. Pour créer une partition, `Parted` utilise `mkpart`. Vous pouvez indiquer des paramètres supplémentaires de type **primaire** ou **logique** en fonction du type de partition que vous souhaitez créer.<br />

   Les unités répertoriées étant exprimées par défaut en mégaoctets (Mo), pour créer une partition de 10 Go, vous devez commencer à 1 et terminer à 10 000. Vous pouvez également modifier les unités de dimensionnement en téraoctets en saisissant `unit TB` si vous le souhaitez.{: tip}

      ```
      mkpart
      ```
      {: pre}

   5. Quittez `Parted` à l'aide de la commande `quit`.

      ```
      quit
      ```
      {: pre}

3. Créez un système de fichiers sur la nouvelle partition.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   Il est important de sélectionner le disque et la partition corrects lorsque vous exécutez cette commande.
   <br />Vérifiez le résultat en imprimant la table de partition. ext3 est affiché dans la colonne du système de fichiers.
   {:important}

4. Créez un point de montage pour le système de fichiers et montez-le.
   - Créez un nom de partition `PerfDisk` ou un emplacement où monter le système de fichiers.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Montez le stockage avec le nom de partition.

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Vérifiez que votre nouveau système de fichiers est répertorié.

     ```
     df -h
     ```
     {: pre}

5. Ajoutez le nouveau système de fichiers au fichier `/etc/fstab` du système pour activer le montage automatique à l'amorçage.
   - Ajoutez la ligne suivante à la fin du fichier `/etc/fstab` (en utilisant le nom de partition provenant de l'étape 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## Vérification que MPIO est correctement configuré dans les systèmes d'exploitation `*NIX`

1. Pour vérifier si le multi-accès sélectionne les périphériques, affichez la liste des périphériques. Si la configuration est correcte, seuls deux périphériques NETAPP sont affichés.

  ```
  multipath -l
  ```
  {: pre}

  ```
  root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
  6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
  7:0:0:101 sde 8:64 active undef running
  ```

2. Assurez-vous que les disques sont présents. Vous devez avoir deux disques portent le même identificateur et une liste `/dev/mapper` de même taille existe avec le même identificateur. Le périphérique `/dev/mapper` est celui qui est configuré par le multi-accès. 
  ```
  fdisk -l | grep Disk
  ```
  {: pre}
  
  - Exemple de sortie d'une configuration correcte :

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - Exemple de sortie d'une configuration incorrecte :
    
    ```
    No multipath output root@server:~# multipath -l root@server:~#
    ```
    
    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. Confirmez que les disques locaux ne sont pas inclus dans les périphériques multi-accès. La commande suivante affiche les périphériques qui sont sur liste noire :
   ```
   multipath -l -v 3 | grep sd <date and time>
   ```
   {: pre}
 
   ```
   root@server:~# multipath -l -v 3 | grep sd Feb 17 19:55:02
| sda: device node name blacklisted Feb 17 19:55:02
| sdb: device node name blacklisted Feb 17 19:55:02
| sdc: device node name blacklisted Feb 17 19:55:02
| sdd: device node name blacklisted Feb 17 19:55:02
| sde: device node name blacklisted Feb 17 19:55:02
   ```

## Démontage des volumes {{site.data.keyword.blockstorageshort}}

1. Démontez le système de fichiers. 
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. Si vous n'avez pas d'autre volumes dans ce portail cible, vous pouvez vous déconnecter de la cible.
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}
   
3. Si vous n'avez pas d'autre volumes dans ce portail cible, supprimez l'enregistrement de portail cible pour empêcher les futures tentatives de connexion. 
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}
  
   Pour plus d'informations, voir [man page of iscsiadm](https://linux.die.net/man/8/iscsiadm).
   {:tip}
