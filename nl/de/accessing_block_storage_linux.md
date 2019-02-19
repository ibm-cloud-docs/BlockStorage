---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"
---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Verbindung zu iSCSI-LUNs unter Linux herstellen
{: #mountingLinux}

Diese Anweisungen gelten hauptsächlich für RHEL6 und Centos6. Es wurden zwar Hinweise für andere Betriebssysteme hinzugefügt, aber dennoch gilt diese Dokumentation **nicht** für alle Linux-Distributionen. Falls Sie ein anderes Linux-Betriebssystem verwenden, finden Sie Informationen hierzu in der Dokumentation zu Ihrer jeweiligen Distribution; stellen Sie sicher, dass ALUA von Multipath für die Pfadpriorität unterstützt wird.
{:note}

Die Anweisungen für Ubuntu zur Konfiguration des iSCSI-Initiators finden Sie zum Beispiel [hier ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} und die Anweisungen zur Konfiguration von Device-Mapper Multipathing finden Sie [hier ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.
{: tip}

Stellen Sie vor dem Start sicher, dass der Host, von dem auf das {{site.data.keyword.blockstoragefull}}-Laufwerk zugegriffen wird, im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} zuvor autorisiert wurde.
{:important}

1. Suchen Sie auf der Seite mit der {{site.data.keyword.blockstorageshort}}-Liste den neuen Datenträger und klicken Sie auf **Aktionen**.
2. Klicken Sie auf **Host autorisieren**.
3. Wählen Sie in der Liste den Host oder die Hosts aus, der bzw. die auf den Datenträger zugreifen kann bzw. können, und klicken Sie auf **Abschicken**.

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

## {{site.data.keyword.blockstorageshort}}-Datenträger anhängen

Nachfolgend werden die Schritte beschrieben, die zum Herstellen einer Verbindung von einer Linux-basierten {{site.data.keyword.BluSoftlayer_full}}-Recheninstanz zu einer MPIO-iSCSI-LUN erforderlich sind (MPIO = Multipath Input/Output; iSCSI = internet Small Computer System Interface; LUN = Logical Unit Number).

Der Host-IQN, der Benutzername, das Kennwort und die Zieladresse, auf die in den Anweisungen verwiesen wird, können in der Anzeige **{{site.data.keyword.blockstorageshort}} Details** im [{{site.data.keyword.slportal}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://control.softlayer.com/){:new_window} abgelesen werden.
{: tip}

Es wird empfohlen, den Speicherdatenverkehr über ein VLAN auszuführen, das die Firewall umgeht. Eine Ausführung des Speicherdatenverkehrs über Software-Firewalls erhöht die Latenz und beeinträchtigt die Speicherleistung.
{:important}

1. Installieren Sie die iSCSI- und Multipath-Dienstprogramme auf Ihrem Host.
  - RHEL und CentOS
     ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu und Debian

    ```
    sudo apt-get update
   sudo apt-get install multipath-tools
    ```
    {: pre}

2. Erstellen oder bearbeiten Sie ggf. Ihre Multipath-Konfigurationsdatei.
  - RHEL 6 und CENTOS 6
    * Bearbeiten Sie die Datei **/etc/multipath.conf** mit der folgenden Mindestkonfiguration.

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

    - Starten Sie die Services `iscsi` und `iscsid` erneut, damit die Änderungen wirksam werden.

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - Die Datei `multipath.conf` kann bei RHEL7/CentOS7 leer sein, da das Betriebssystem über integrierte Konfigurationen verfügt.
  - Ubuntu verwendet `multipath.conf` nicht, da es in `multipath-tools` integriert ist.

3. Laden Sie das Multipath-Modul, starten Sie die Multipath-Services und legen Sie fest, dass es beim Booten gestartet werden soll.
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

  - Lesen Sie bei anderen Distributionen die Dokumentation des Anbieters des Betriebssystems.

4. Überprüfen Sie, ob Multipath funktioniert.
  - RHEL 6
    ```
    multipath -l
    ```
    {: pre}

    Eine leere Rückgabe gibt an, dass es funktioniert.
  - CentOS 7
    ```
    multipath -ll
    ```
    {: pre}

    Möglicherweise geben RHEL 7 und CentOS 7 die Nachricht zurück, dass kein fc_host-Gerät vorhanden ist. Dies kann ignoriert werden.

5. Aktualisieren Sie die Datei `/etc/iscsi/initiatorname.iscsi` mit dem IQN aus dem {{site.data.keyword.slportal}}. Geben Sie den Wert in Kleinbuchstaben ein.
   ```
   InitiatorName=<Wert-aus-Portal>
   ```
   {: pre}
6. Bearbeiten Sie die CHAP-Einstellungen in `/etc/iscsi/iscsid.conf` mit dem Benutzernamen und dem Kennwort aus dem {{site.data.keyword.slportal}}. Geben Sie die CHAP-Namen in Großbuchstaben ein.
   ```
   node.session.auth.authmethod = CHAP
    node.session.auth.username = <Wert-des-Benutzernamens-aus-Portal>
    node.session.auth.password = <Wert-des-Kennworts-aus-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Wert-des-Benutzernamens-aus-Portal>
    discovery.sendtargets.auth.password = <Wert-des-Kennworts-aus-Portal>
   ```
   {: codeblock}

   Lassen Sie die anderen CHAP-Einstellungen auf Kommentar. Der {{site.data.keyword.BluSoftlayer_full}}-Speicher verwendet nur unidirektionale Authentifizierung. Aktivieren Sie nicht die gemeinsame Nutzung von CHAP (Mutual CHAP).
   {:important}

   Für Ubuntu-Benutzer: Überprüfen Sie in der Datei `iscsid.conf`, ob die Einstellung für `node.startup` 'manuell' oder 'automatisch' lautet. Lautet sie 'manuell', ändern Sie sie in 'automatisch'.
   {:tip}

7. Legen Sie fest, dass iSCSI beim Booten gestartet wird, und starten Sie das Protokoll jetzt.
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

   - Lesen Sie bei anderen Distributionen die Dokumentation des Anbieters des Betriebssystems.

8. Führen Sie die Erkennung des Geräts mithilfe der aus dem {{site.data.keyword.slportal}} abgerufenen Ziel-IP-Adresse aus.

   A. Führen Sie die Erkennung für das iSCSI-Array aus.
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-Wert-aus-SL-Portal>
    ```
    {: pre}

   B. Legen Sie fest, dass sich der Host automatisch beim iSCSI-Array anmeldet.
    ```
    iscsiadm -m node -L automatic
    ```
    {: pre}

9. Stellen Sie sicher, dass der Host am iSCSI-Array angemeldet ist und seine Sitzungen aufrechterhalten werden.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Von diesem Befehl werden die Pfade gemeldet.

10. Überprüfen Sie, ob das Gerät verbunden ist. Standardmäßig wird das Gerät an `/dev/mapper/mpathX` angehängt, wobei X die generierte ID des verbundenen Geräts ist.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Dieser Befehl meldet Ähnliches wie im folgenden Beispiel.
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```
  Nun ist der Datenträger angehängt und der Zugriff darauf ist über den Host möglich.

## Dateisystem erstellen (optional)

Führen Sie die folgenden Schritte aus, um ein Dateisystem auf dem neu angehängten Datenträger zu erstellen. Ein Dateisystem ist erforderlich, damit die meisten Anwendungen den Datenträger verwenden können. Verwenden Sie [`fdisk` für Laufwerke mit weniger als 2 TB](#fdisk) und [`parted` für eine Platte mit mehr als 2 TB](#parted).

### Dateisystem mit `fdisk` erstellen
{: #fdisk}

1. Rufen Sie den Namen des Datenträgers ab.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   Der zurückgegebene Plattenname ähnelt `/dev/mapper/XXX`.

2. Erstellen Sie eine Partition auf dem Datenträger.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX steht für den in Schritt 1 zurückgegebenen Datenträgernamen. <br />

   Blättern Sie weiter nach unten zu den Befehlscodes, die in der Befehlstabelle `fdisk` aufgelistet sind.
   {: tip}

3. Erstellen Sie ein Dateisystem auf der neuen Partition.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - Die neue Partition wird mit dem Datenträger (ähnlich wie `XXXlp1`), der Größe, dem Typ (83) und Linux aufgeführt.
   - Achten Sie auf den Partitionsnamen; Sie werden ihn im nächsten Schritt brauchen. (XXXlp1 steht für den Partitionsnamen.)
   - Erstellen Sie das Dateisystem:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Erstellen Sie einen Mountpunkt für das Dateisystem und hängen Sie es an.
   - Erstellen Sie den Partitionsnamen `PerfDisk` oder den Namen des Datenträgers, an den Sie das Dateisystem anhängen wollen.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Hängen Sie den Speicher mit dem Partitionsnamen an.
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Prüfen Sie, ob Ihr neues Dateisystem in der Liste angezeigt wird.
     ```
     df -h
     ```
     {: pre}

5. Fügen Sie das neue Dateisystem zur Datei `/etc/fstab` des Systems hinzu, um das automatische Anhängen beim Booten zu aktivieren.
   - Hängen Sie an das Ende der Datei `/etc/fstab` die folgende Zeile an (mit dem Partitionsnamen aus Schritt 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Die Befehlstabelle `fdisk`

<table border="0" cellpadding="0" cellspacing="0">
	<caption>Die Befehlstabelle <code>fdisk</code> enthält die Befehle auf der linken und die erwarteten Ergebnisse auf der rechten Seite.</caption>
    <thead>
	<tr>
		<th style="width:40%;">Befehl</th>
		<th style="width:60%;">Ergebnis</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Befehl: n</code></td>
		<td>Erstellt eine Partition. &#42;</td>
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

### Dateisystem mit `parted` erstellen
{: #parted}

Bei vielen Linux-Distributionen ist `parted` vorinstalliert. Wenn das Programm nicht in Ihrer Distribution enthalten ist, können Sie es wie folgt installieren:
- Debian und Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL und CentOS
  ```
  yum install parted  
  ```
  {: pre}


Führen Sie die folgenden Schritte aus, um ein Dateisystem mit `parted` zu erstellen.

1. Führen Sie `parted` aus.

   ```
   parted
   ```
   {: pre}

2. Erstellen Sie eine Partition auf dem Datenträger.
   1. Wenn nicht anders angegeben, wird von `parted` das primäre Laufwerk verwendet, in der Regel `/dev/sda`. Wechseln Sie mit dem Befehl **select** zu dem Datenträger, den Sie partitionieren möchten. Ersetzen Sie **XXX** durch den Namen Ihres neuen Geräts.

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Führen Sie `print` aus, um zu bestätigen, dass Sie sich auf der richtigen Platte befinden.

      ```
      print
      ```
      {: pre}

   3. Erstellen Sie eine GPT-Partitionstabelle.

      ```
      mklabel gpt
      ```
      {: pre}

   4. Mit `parted` können primäre und logische Datenträgerpartitionen erstellt werden; die Schritte dazu sind identisch. Von `parted` wird `mkpart` zum Erstellen einer Partition verwendet. Je nach dem zu erstellenden Partitionstyp können Sie zusätzliche Parameter wie **primary** oder **logical** verwenden.<br />

   Die aufgelisteten Einheiten werden standardmäßig in Megabyte (MB) angegeben. Um eine Partition mit 10 GB zu erstellen, beginnen Sie mit 1 und enden bei 10000. Sie können die Dimensionierungseinheit auch in Terabyte ändern, indem Sie `unit TB` eingeben, wenn Sie dies wünschen.
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. Beenden Sie `parted` mit `quit`.

      ```
      quit
      ```
      {: pre}

3. Erstellen Sie ein Dateisystem auf der neuen Partition.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   Es ist wichtig, die richtige Platte und Partition auszuwählen, wenn Sie diesen Befehl ausführen.<br />Drucken Sie zur Überprüfung des Ergebnisses die Partitionstabelle aus. Unter der Spalte mit den Dateisystemen wird 'ext3' angezeigt.
   {:important}

4. Erstellen Sie einen Mountpunkt für das Dateisystem und hängen Sie es an.
   - Erstellen Sie den Partitionsnamen `PerfDisk` oder den Namen des Datenträgers, an den Sie das Dateisystem anhängen wollen.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Hängen Sie den Speicher mit dem Partitionsnamen an.

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Prüfen Sie, ob das neue Dateisystem in der Liste aufgeführt wird.

     ```
     df -h
     ```
     {: pre}

5. Fügen Sie das neue Dateisystem zur Datei `/etc/fstab` des Systems hinzu, um das automatische Anhängen beim Booten zu aktivieren.
   - Hängen Sie an das Ende der Datei `/etc/fstab` die folgende Zeile an (mit dem Partitionsnamen aus Schritt 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## MPIO-Konfiguration überprüfen
{: #verifyMPIOLinux}

1. Listen Sie nur die Geräte auf, um zu überprüfen, ob die Geräte von Multipath ausgewählt werden. Wenn die Konfiguration korrekt ist, werden nur zwei NETAPP-Geräte angezeigt.

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

2. Überprüfen Sie, ob die Platten vorhanden sind. Es müssen zwei Platten mit derselben Kennung und eine `/dev/mapper`-Auflistung mit derselben Größe und mit derselben ID vorhanden sein. Die Einheit `/dev/mapper` ist die Einheit, die Multipath konfiguriert.
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - Beispielausgabe einer korrekten Konfiguration.

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - Beispielausgaben einer fehlerhaften Konfiguration.

    ```
    No multipath output root@server:~# multipath -l root@server:~#
    ```

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. Bestätigen Sie, dass lokale Platten nicht in Multipath-Einheiten enthalten sind. Mit dem folgenden Befehl werden die Einheiten angezeigt, die auf die schwarze Liste stehen.
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

## {{site.data.keyword.blockstorageshort}}-Datenträger abhängen
{: #unmounting}

1. Hängen Sie das Dateisystem ab.
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. Wenn Sie über keine anderen Datenträger in diesem Zielportal verfügen, können Sie sich von dem Ziel abmelden.
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. Wenn Sie über keine anderen Datenträger in diesem Zielportal verfügen, löschen Sie den Zielportaldatensatz, um künftige Anmeldeversuche zu verhindern.
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   Weitere Informationen finden Sie im Handbuch zu [`iscsiadm` ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://linux.die.net/man/8/iscsiadm).
   {:tip}
