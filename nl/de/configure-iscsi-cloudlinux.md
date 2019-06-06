---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: IBM Block Storage, MPIO, iSCSI, LUN, mount secondary storage, mount storage in CloudLinux

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Verbindung zu iSCSI-LUNs unter CloudLinux herstellen
{: #mountingCloudLinux}

Führen Sie die folgenden Anweisungen aus, um die iSCSI-LUN mit Multipath auf CloudLinux Server Release 6.10 zu installieren.

Stellen Sie vor Beginn sicher, dass der Host, von dem auf den {{site.data.keyword.blockstoragefull}}-Datenträger zugegriffen wird, im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} zuvor autorisiert wurde.
{:tip}

1. Melden Sie sich am [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} an.
2. Suchen Sie auf der Seite mit der {{site.data.keyword.blockstorageshort}}-Liste den neuen Datenträger und klicken Sie auf **Aktionen**.
3. Klicken Sie auf **Host autorisieren**.
4. Wählen Sie in der Liste den Host oder die Hosts aus, der bzw. die auf den Datenträger zugreifen kann bzw. können, und klicken Sie auf **Abschicken**.
5. Notieren Sie Host-IQN, Benutzername, Kennwort und Zieladresse.

Alternativ dazu können Sie den Host auch über die SL-CLI berechtigen.
```
# slcli block access-authorize --help
Syntax: slcli block access-authorize [OPTIONEN] DATENTRÄGER_ID

Optionen:
  -h, --hardware-id TEXT    ID einer SoftLayer-Hardware zur Berechtigung
  -v, --virtual-id TEXT     ID eines virtuellen SoftLayer-Gastsystems zur Berechtigung
  -i, --ip-address-id TEXT  ID der Teilnetz-IP-Adresse eines SoftLayer-Netzes
                            zur Berechtigung
  --ip-address TEXT         IP-Adresse zur Berechtigung
  --help                    Diese Nachricht anzeigen und Ausführung beenden.
```
{:codeblock}

Es wird empfohlen, den Speicherdatenverkehr über ein VLAN auszuführen, das die Firewall umgeht. Eine Ausführung des Speicherdatenverkehrs über Software-Firewalls erhöht die Latenz und beeinträchtigt die Speicherleistung.
{:important}

## {{site.data.keyword.blockstorageshort}}-Datenträger anhängen
{: #mountingCloudLin}

1. Installieren Sie die iSCSI- und Multipath-Dienstprogramme auf dem Host und aktivieren Sie sie.
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

2. Erstellen oder bearbeiten Sie die Konfigurationsdateien.
   - Aktualisieren Sie '/etc/multipath.conf'. <br/>**Hinweis:** Alle Daten unter Blacklist müssen sich speziell auf Ihr System beziehen.
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

   - Aktualisieren Sie die CHAP-Einstellungen `/etc/iscsi/iscsid.conf` durch Hinzufügen von Benutzername und Kennwort.

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

     Geben Sie die CHAP-Namen in Großbuchstaben ein. Lassen Sie die anderen CHAP-Einstellungen auf Kommentar. Der {{site.data.keyword.cloud}}-Speicher verwendet nur unidirektionale Authentifizierung. Aktivieren Sie nicht die gemeinsame Nutzung von CHAP (Mutual CHAP).
     {:important}


3. Starten Sie die Services `iscsi` und `multipathd` erneut.
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}

   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}

4. Führen Sie die Erkennung des Geräts mithilfe der aus dem {{site.data.keyword.slportal}} abgerufenen Ziel-IP-Adresse aus.

     A. Führen Sie die Erkennung für das iSCSI-Array aus.
       ```
       iscsiadm -m discovery -t sendtargets -p <ip-Wert-aus-SL-Portal>
       ```
       {: pre}

        Beispielausgabe
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. Legen Sie fest, dass sich der Host automatisch beim iSCSI-Array anmeldet.
       ```
       iscsiadm -m node -L automatic
       ```
       {: pre}

5. Stellen Sie sicher, dass der Host am iSCSI-Array angemeldet ist und seine Sitzungen aufrechterhalten werden.
   ```
   iscsiadm -m session
   ```
   {: pre}

   Beispielausgabe
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. Überprüfen Sie, ob das Gerät verbunden ist.
   ```
   fdisk -l
   ```
   {: pre}

   Beispielausgabe
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

   Nun ist der Datenträger angehängt und der Zugriff darauf ist über den Host möglich.

7. Überprüfen Sie durch Auflisten der Geräte, ob MPIO ordnungsgemäß konfiguriert ist. Wenn die Konfiguration korrekt ist, werden nur zwei NETAPP-Geräte angezeigt.

   ```
   # multipath -l
   ```
   {: pre}

   Beispielausgabe
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+- policy='round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
