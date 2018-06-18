---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Verbindung zu MPIO iSCSI LUNs unter Linux herstellen

Die folgenden Anweisungen gelten für RHEL6/Centos6. Es wurden zwar Hinweise für andere Betriebssysteme hinzugefügt, aber dennoch gilt diese Dokumentation **nicht** für alle Linux-Distributionen. Falls Sie ein anderes Linux-Betriebssystem verwenden, finden Sie Informationen hierzu in der Dokumentation zu Ihrer jeweiligen Distribution; stellen Sie sicher, dass ALUA von Multipath für die Pfadpriorität unterstützt wird. Die Anweisungen für Ubuntu zur Konfiguration des iSCSI-Initiators finden Sie zum Beispiel [hier](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} und die Anweisungen zur Konfiguration von Device-Mapper Multipathing finden Sie [hier](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.

Stellen Sie vor dem Start sicher, dass der Host, der auf den {{site.data.keyword.blockstoragefull}}-Datenträger zugreift, über das [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} autorisiert wurde:

1. Klicken Sie auf der Seite mit der {{site.data.keyword.blockstorageshort}}-Liste auf die Schaltfläche **Aktionen**, die dem neuen Datenträger zugeordnet ist.
2. Klicken Sie auf **Host autorisieren**.
3. Wählen Sie in der Liste den Host oder die Hosts aus, von dem bzw. denen auf den Datenträger zugegriffen werden soll, und klicken Sie auf **Abschicken**.

## {{site.data.keyword.blockstorageshort}}-Datenträger anhängen

Nachfolgend werden die Schritte beschrieben, die zum Herstellen einer Verbindung von einer Linux-basierten {{site.data.keyword.BluSoftlayer_full}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind (MPIO = Multipath Input/Output; iSCSI = Internet Small Computer System Interface; LUN = logical unit number).

**Hinweis:** Der Host-IQN, der Benutzername, das Kennwort und die Zieladresse, auf die in den Anweisungen verwiesen wird, können in der Anzeige **{{site.data.keyword.blockstorageshort}} - Details** im [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} abgerufen werden. 

**Hinweis:** Es empfiehlt sich, Speicherdatenverkehr über ein VLAN auszuführen, das die Firewall umgeht. Eine Ausführung des Speicherdatenverkehrs über Software-Firewalls erhöht die Latenz und beeinträchtigt die Speicherleistung.

1. Installieren Sie die iSCSI- und Multipath-Dienstprogramme auf Ihrem Host:
   - RHEL/CentOS:

   ```
   yum install iscsi-initiator-utils device-mapper-multipath
   ```
   {: pre}

   - Ubuntu/Debian:

   ```
   sudo apt-get update
   sudo apt-get install multipath-tools
   ```
   {: pre}

2. Erstellen oder bearbeiten Sie Ihre Multipath-Konfigurationsdatei.
   - Bearbeiten Sie **/etc/multipath.conf** mit der in den folgenden Befehlen bereitgestellten Mindestkonfiguration. <br /><br /> **Hinweis:** Beachten Sie, dass `multipath.conf` bei RHEL7/CentOS7 leer sein kann, da das Betriebssystem über integrierte Konfigurationen verfügt. Ubuntu verwendet multipath.conf nicht, da es in multipath-tools integriert ist.

   ```
   defaults {
   user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
   # Alle Daten unter Blacklist müssen sich speziell auf Ihr System beziehen.
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

3. Laden Sie das Multipath-Modul, starten Sie die Multipath-Services und legen Sie fest, dass es beim Booten gestartet werden soll.
   - RHEL 6:
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

   - CentOS 7:
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

   - Ubuntu:
     ```
     service multipath-tools start
     ```
     {: pre}

   - Lesen Sie bei anderen Distributionen die Dokumentation des Anbieters des Betriebssystems.

4. Überprüfen Sie, ob Multipath funktioniert.
   - RHEL 6:
     ```
     multipath -l
     ```
     {: pre}

     Eine leere Rückgabe zeigt an, dass es funktioniert.

   - CentOS 7:
     ```
     multipath -ll
     ```
     {: pre}

     Möglicherweise gibt RHEL 7/CentOS 7 kein fc_host-Gerät zurück. Dies kann ignoriert werden.

5. Aktualisieren Sie die Datei **/etc/iscsi/initiatorname.iscsi** mit dem IQN aus dem {{site.data.keyword.slportal}}. Geben Sie den Wert in Kleinbuchstaben ein.
   ```
   InitiatorName=<Wert-aus-Portal>
   ```
   {: pre}
6. Bearbeiten Sie die CHAP-Einstellungen in **/etc/iscsi/iscsid.conf** mit dem Benutzernamen und dem Kennwort aus dem {{site.data.keyword.slportal}}. Geben Sie die CHAP-Namen in Großbuchstaben ein.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Wert-des-Benutzernamens-aus-Portal>
    node.session.auth.password = <Wert-des-Kennworts-aus-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Wert-des-Benutzernamens-aus-Portal>
    discovery.sendtargets.auth.password = <Wert-des-Kennworts-aus-Portal>
   ```
   {: codeblock}

   **Hinweis:** Lassen Sie die anderen CHAP-Einstellungen auf Kommentar. Der {{site.data.keyword.BluSoftlayer_full}}-Speicher verwendet nur unidirektionale Authentifizierung.

7. Legen Sie fest, dass iSCSI beim Booten gestartet wird, und starten Sie das Protokoll dieses Mal.
   - RHEL 6:

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

   - CentOS 7:

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

   - Lesen Sie bei anderen Distributionen die Dokumentation des Anbieters des Betriebssystems.

8. Führen Sie die Erkennung des Geräts mithilfe der aus dem {{site.data.keyword.slportal}} abgerufenen Ziel-IP-Adresse aus.

     a. Führen Sie die Erkennung für das iSCSI-Array aus:
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-Wert-aus-SL-Portal>
     ```
     {: pre}

     b. Legen Sie fest, dass sich der Host automatisch beim iSCSI-Array anmeldet:
     ```
     iscsiadm -m node -L automatic
     ```
     {: pre}

9. Stellen Sie sicher, dass sich Host beim iSCSI-Array angemeldet und seine Sitzungen aufrechterhalten hat.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Damit sollten diesmal die Pfade zurückgemeldet werden.

10. Überprüfen Sie, ob das Gerät verbunden ist.  Standardmäßig wird das Gerät an /dev/mapper/mpathX angehängt, wobei X die generierte ID des verbundenen Geräts ist.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Nun sollte eine ähnliche Meldung wie die folgende angezeigt werden:
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 byte
    ```
  Nun ist der Datenträger angehängt und der Zugriff darauf ist über den Host möglich.

## Dateisystem erstellen (optional)

Nachfolgend finden Sie die Schritte zum Erstellen eines Dateisystems auf einem neu angehängten Datenträger. Ein Dateisystem ist erforderlich, damit die meisten Anwendungen den Datenträger verwenden können. Verwenden Sie **fdisk** für Laufwerke bis zu 2 TB und **parted** für Datenträger ab 2 TB.

### fdisk

1. Rufen Sie den Namen des Datenträgers ab.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   Der zurückgegebene Datenträgername sollte /dev/mapper/XXX ähneln.

2. Erstellen Sie mit fdisk eine Partition auf dem Datenträger.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX steht für den in Schritt 1 zurückgegebenen Datenträgernamen. <br />

   **Hinweis**: Wenn Sie nach unten blättern, finden Sie die in der Fdisk-Befehlstabelle nach diesem Abschnitt aufgeführten Befehlscodes.

3. Erstellen Sie auf der neuen Partition ein Dateisystem.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - Die neue Partition sollte mit dem Datenträger (ähnlich wie XXXlp1), der Größe, dem Typ (83) und Linux aufgeführt werden.
   - Achten Sie auf den Partitionsnamen. Sie werden ihn im nächsten Schritt brauchen. (XXXlp1 steht für den Partitionsnamen.)
   - Erstellen Sie das Dateisystem:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Erstellen Sie einen Mountpunkt für das Dateisystem und hängen Sie es an.
   - Erstellen Sie einen Partitionsnamen, z. B. PerfDisk oder den Namen des Datenträgers, an den Sie das Dateisystem anhängen wollen:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Hängen Sie den Speicher mithilfe des Partitionsnamens an:
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Prüfen Sie, ob Ihr neues Dateisystem in der Liste angezeigt wird:
     ```
     df -h
     ```
     {: pre}

5. Fügen Sie das neue Dateisystem zur Datei **/etc/fstab** des Systems hinzu, um das automatische Anhängen beim Booten zu aktivieren.
   - Hängen Sie an das Ende der Datei **/etc/fstab** die folgende Zeile an (unter Verwendung des Partitionsnamens aus Schritt 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Fdisk-Befehlstabelle



<table border="0" cellpadding="0" cellspacing="0">
  <caption>Die Fdisk-Befehlstabelle enthält die Befehle auf der linken und die erwarteten Ergebnisse auf der rechten Seite.</caption>
    <thead>
	<tr>
		<th style="width:40%;">Befehl</th>
		<th style="width:60%;">Ergebnis</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Befehl: n</code></td>
		<td>Erstellt eine neue Partition. &#42;</td>
	</tr>
	<tr>
		<td><code>Befehlsaktion: p</code></td>
		<td>Macht die Partition zur Primärpartition.</td>
	</tr>
	<tr>
		<td><code>Partitionsnummer (1-4): 1</code></td>
		<td>Wird Partition 1 auf dem Datenträger.</td>
	</tr>
	<tr>
		<td><code>Erster Zylinder (1-8877): 1 (Standardwert)</code></td>
		<td>Start bei Zylinder 1.</td>
	</tr>
	<tr>
		<td><code>Letzter Zylinder, + Zylinder oder + Größe {K, M, G}: 8877 (Standardwert)</code></td>
		<td>Drücken Sie die Eingabetaste, um zum letzten Zylinder zu wechseln.</td>
	</tr>
	<tr>
		<td><code>Befehl: t</code></td>
		<td>Legt den Partitionstyp fest. &#42;</td>
	</tr>
	<tr>
		<td><code>Partition 1 auswählen.</code></td>
		<td>Wählt Partition 1 für die Einrichtung als spezieller Typ aus.</td>
	</tr>
	<tr>
		<td><code>Hexadezimaler Code: 83</code></td>
		<td>Wählt Linux als Typ aus (83 ist der hexadezimale Code für Linux).&#42;&#42;</td>
	 </tr>
	<tr>
		<td><code>Befehl: w</code></td>
		<td>Schreibt die Informationen zur neuen Partition auf den Datenträger. &#42;</td>
	</tr>
   </tbody>
</table>

  (`*`)Geben Sie m für Hilfe ein.

  (`**`)Geben Sie L ein, um die hexadezimalen Codes aufzulisten.

### parted

Bei vielen Linux-Distributionen ist **parted** vorinstalliert. Wenn das Programm nicht in Ihrer Distribution enthalten ist, können Sie es wie folgt installieren:
- Debian/Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL/CentOS:
  ```
  yum install parted  
  ```
  {: pre}


Führen Sie die folgenden Schritte aus, um ein Dateisystem mit **parted** zu erstellen:

1. Führen Sie parted aus.

   ```
   parted
   ```
   {: pre}

2. Erstellen Sie eine Partition auf dem Datenträger.
   1. Wenn nicht anders angegeben, verwendet parted Ihr primäres Laufwerk, in der Regel **/dev/sda**. Wechseln Sie mit dem Befehl **select** zu dem Datenträger, den Sie partitionieren wollen. Ersetzen Sie **XXX** durch den Namen Ihres neuen Geräts.

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. Führen Sie **print** aus, um zu bestätigen, dass Sie sich auf dem richtigen Datenträger befinden.

      ```
      (parted) print
      ```
      {: pre}

   3. Erstellen Sie eine neue GPT-Partitionstabelle.

      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. Mit parted lassen sich primäre und logische Datenträgerpartitionen erstellen. Die Schritte dazu sind identisch. Parted verwendet **mkpart** zum Erstellen einer neuen Partition. Je nach dem zu erstellenden Partitionstyp können Sie zusätzliche Parameter wie **primary** oder **logical** verwenden.
   <br /> **Hinweis**: Standardmäßig stehen die aufgeführten Einheiten für Megabyte (MB). Zum Erstellen einer 10-GB-Partition sollten Sie bei 1 beginnen und bei 10000 aufhören. Sie können die Einheit bei Bedarf auch in Terabyte ändern, indem Sie `(parted) unit TB` eingeben.

      ```
      (parted) mkpart
      ```
      {: pre}

   5. Beenden Sie parted mit **Beenden**.

      ```
      (parted) quit
      ```
      {: pre}

3. Erstellen Sie auf der neuen Partition ein Dateisystem.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **Hinweis**: Es ist wichtig, dass Sie bei der Ausführung des obigen Befehls den richtigen Datenträger und die richtige Partition auswählen!
   Drucken Sie zur Überprüfung des Ergebnisses die Partitionstabelle aus. Unter der Spalte mit den Dateisystemen sollte ext3 angezeigt werden.

4. Erstellen Sie einen Mountpunkt für das Dateisystem und hängen Sie es an.
   - Erstellen Sie einen Partitionsnamen, z. B. PerfDisk oder den Namen des Datenträgers, an den Sie das Dateisystem anhängen wollen:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Hängen Sie den Speicher mithilfe des Partitionsnamens an:

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Prüfen Sie, ob Ihr neues Dateisystem in der Liste angezeigt wird:

     ```
     df -h
     ```
     {: pre}

5. Fügen Sie das neue Dateisystem zur Datei **/etc/fstab** des Systems hinzu, um das automatische Anhängen beim Booten zu aktivieren.
   - Hängen Sie an das Ende der Datei **/etc/fstab** die folgende Zeile an (unter Verwendung des Partitionsnamens aus Schritt 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## Korrekte Konfiguration von MPIO auf *NIX-Betriebssystemen prüfen

Um zu prüfen, ob Multipath die Geräte auswählt, sollten nur die NETAPP-Geräte angezeigt werden, und zwar genau zwei von ihnen.

```
# multipath -l
```
{: pre}

```
root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
7:0:0:101 sde 8:64 active undef running
```

Prüfen Sie, ob die Datenträger vorhanden sind. Außerdem sollten zwei Datenträger mit derselben ID sowie eine /dev/mapper-Liste derselben Größe mit derselben ID vorhanden sein. Bei dem /dev/mapper-Gerät handelt es sich um das Gerät, das Multipath einrichtet:

```
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```

Wenn es nicht korrekt eingerichtet ist, sieht es wie folgt aus:
```
No multipath output root@server:~# multipath -l root@server:~#
```

In einer Blacklist aufgeführte Geräte werden wie folgt angezeigt:
```
# multipath -l -v 3 | grep sd <Datum und Uhrzeit>
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

fdisk zeigt nur die `sd*`-Geräte an, keine `/dev/mapper`-Geräte.

```
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```
