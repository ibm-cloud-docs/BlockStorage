---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-02"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Connessione ai LUN iSCSI MPIO su CloudLinux

Segui queste istruzioni per installare la tua LUN iSCSI con multipath su CloudLinux Server release 6.10.

Prima di iniziare, assicurarti che l'host che sta accedendo al volume {{site.data.keyword.blockstoragefull}} sia stato precedentemente autorizzato tramite [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Accedi al [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. 
2. Dalla pagina di elenco {{site.data.keyword.blockstorageshort}}, individua il nuovo volume e fai clic su **Actions**.
3. Fai clic su **Authorize Host**.
4. Dall'elenco, seleziona l'host o gli host che può accedere al volume e fai clic su **Submit**.
5. Prendi nota dell'IQN host, del nome utente, della password e dell'indirizzo di destinazione. 

**Nota:** consigliamo di eseguire il traffico di archiviazione su una VLAN che ignora il firewall. L'esecuzione del traffico di archiviazione tramite i firewall software aumenta la latenza e ha un impatto negativo sulle prestazioni dell'archiviazione.

## Montaggio di volumi {{site.data.keyword.blockstorageshort}}

1. Installa iSCSI e i programmi di utilità multipath sul tuo host e attivali.
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

2. Crea o modifica i tuoi file di configurazione.
   - Aggiorna il tuo '/etc/multipath.conf'. <br/>**Nota** - Devono essere specificati tutti i dati nella blacklist nel tuo sistema.
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

   - Aggiorna le tue impostazioni CHAP `/etc/iscsi/iscsid.conf` aggiungendo il nome utente e la password.
   
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
   
     **Nota** - Usa le maiuscole per i nomi CHAP. Lascia le altre impostazioni CHAP come commenti. L'archiviazione {{site.data.keyword.BluSoftlayer_full}} utilizza solo un'autenticazione unidirezionale. Non abilitare Mutual CHAP.


3. Riavvia i servizi `iscsi` e `multipathd`.
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}
   
   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}
 
4. Rileva il dispositivo utilizzando l'indirizzo IP di destinazione ottenuto dal {{site.data.keyword.slportal}}.

     A. Esegui il rilevamento sull'array iSCSI.
       ```
       iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
       ```
       {: pre}
     
        Output di esempio
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. Imposta l'host in modo che esegua automaticamente l'accesso all'array iSCSI.
       ```
       iscsiadm -m node -L automatic
       ```
       {: pre}

5. Verifica che l'host abbia eseguito l'accesso all'array iSCSI e mantenuto le sue sessioni.
   ```
   iscsiadm -m session
   ```
   {: pre}
   
   Output di esempio 
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. Verifica che il dispositivo sia connesso.
   ```
   fdisk -l 
   ```
   {: pre}
    
   Output di esempio 
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
    
   Il volume è ora montato e accessibile sull'host.

7. Verificare se MPIO sia configurato correttamente elencando i dispositivi. Se la configurazione è corretta, vengono visualizzati solo due dispositivi NETAPP.

   ```
   # multipath -l
   ```
   {: pre}
   
   Output di esempio 
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
