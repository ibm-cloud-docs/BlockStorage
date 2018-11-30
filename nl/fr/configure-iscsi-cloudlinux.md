---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Connexion à des numéros d'unité logique MPIO iSCSI sous CloudLinux

Suivez ces instructions pour installer votre numéro d'unité logique iSCSI avec multi-accès sur CloudLinux Server 6.10.

Avant de commencer, assurez-vous que les droits d'accès nécessaires ont été affectés via le portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window} à l'hôte qui accède au volume {{site.data.keyword.blockstoragefull}}.
{:tip}

1. Connectez-vous au portail [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
2. Sur la page de liste {{site.data.keyword.blockstorageshort}}, repérez le nouveau volume et cliquez sur **Actions**.
3. Cliquez sur **Hôte autorisé**.
4. Dans la liste, sélectionnez l'hôte ou les hôtes qui peuvent accéder au volume et cliquez sur **Soumettre**.
5. Notez le nom qualifié iSCSI hôte, le nom d'utilisateur, le mot de passe et l'adresse cible.

Il est recommandé d'exécuter le trafic de stockage sur un réseau local virtuel qui ignore le pare-feu. L'exécution du trafic de stockage via des pare-feu logiciels augmente le temps d'attente et a un impact négatif sur les performances de stockage.
{:important}

## Montage de volumes {{site.data.keyword.blockstorageshort}}

1. Installez les utilitaires iSCSI et multi-accès sur votre hôte et activez-les.
   ```
   yum install iscsi-initiator-utils
   ```
   {: pre}

   ```
   yum install multipath-tools

   ```
   {: pre}

   ```
   chkconfig multipathd on
   ```
   {: pre}

   ```
   chkconfig iscsid on
   ```
   {: pre}

2. Créez ou éditez vos fichier de configuration.
   - Update your '/etc/multipath.conf'. <br/>**Remarque** : toutes les données répertoriées sous blacklist doivent être propres à votre système.
     ```
     defaults {
        user_friendly_names no
        flush_on_last_del       yes
        queue_without_daemon    no
        dev_loss_tmo            infinity
        fast_io_fail_tmo        5
     }
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

   - Mettez à niveau vos paramètres CHAP `/etc/iscsi/iscsid.conf` en ajoutant le nom d'utilisateur et le mot de passe.

     ```
     iscsid.startup = /etc/rc.d/init.d/iscsid force-start
     node.startup = automatic
     node.leading_login = No
     node.session.auth.authmethod = CHAP
     node.session.auth.username = <USER NAME VALUE FROM PORTAL>
     node.session.auth.password = <PASSWORD VALUE FROM PORTAL>
     discovery.sendtargets.auth.authmethod = CHAP
     discovery.sendtargets.auth.username = <USER NAME VALUE FROM PORTAL>
     discovery.sendtargets.auth.password = <PASSWORD VALUE FROM PORTAL>
     ```
     {: codeblock}

     Utilisez des majuscules pour les noms CHAP. Laissez les autres paramètres CHAP en commentaire. Le stockage {{site.data.keyword.BluSoftlayer_full}} utilise uniquement l'authentification unidirectionnelle. N'activez pas l'authentification CHAP mutuelle.
     {:important}


3. Redémarrez les services `iscsi` et `multipathd`.
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}

   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}

4. Reconnaissez le périphérique à l'aide de l'adresse IP cible obtenue à partir du portail {{site.data.keyword.slportal}}.

     A. Exécutez la reconnaissance sur la grappe iSCSI.
       ```
       iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
       ```
       {: pre}

        Exemple de sortie
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. Définissez l'hôte pour qu'il se connecte automatiquement à la grappe iSCSI.
       ```
       iscsiadm -m node -L automatic
       ```
       {: pre}

5. Vérifiez que l'hôte est connecté à la grappe iSCSI et a conservé ses sessions.
   ```
   iscsiadm -m session
   ```
   {: pre}

   Exemple de sortie
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. Vérifiez que le périphérique est connecté.
   ```
   fdisk -l
   ```
   {: pre}

   Exemple de sortie
   ```
   Disk /dev/sda: 999.7 GB, 999653638144 bytes
   255 heads, 63 sectors/track, 121534 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 262144 bytes / 262144 bytes
   Disk identifier: 0x00040f06

   Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1          33      262144   83  Linux
   Partition 1 does not end on cylinder boundary.
   /dev/sda2              33         164     1048576   82  Linux swap / Solaris
   Partition 2 does not end on cylinder boundary.
   /dev/sda3             164      121535   974912512   83  Linux

   Disk /dev/sdb: 21.5 GB, 21474836480 bytes
   64 heads, 32 sectors/track, 20480 cylinders
   Units = cylinders of 2048 * 512 = 1048576 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   Disk identifier: 0x00000000

   Disk /dev/sdc: 21.5 GB, 21474836480 bytes
   64 heads, 32 sectors/track, 20480 cylinders
   Units = cylinders of 2048 * 512 = 1048576 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   Disk identifier: 0x00000000
   ```

   Le volume est maintenant monté et accessible sur l'hôte.

7. Vérifiez si MPIO est correctement configuré en affichant la liste des périphériques. Si la configuration est correcte, seuls deux périphériques NETAPP sont affichés.

   ```
   # multipath -l
   ```
   {: pre}

   Exemple de sortie
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
