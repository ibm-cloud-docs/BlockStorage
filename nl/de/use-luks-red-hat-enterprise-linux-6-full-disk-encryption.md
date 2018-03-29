---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# LUKS in Red Hat Enterprise Linux für die vollständige Plattenverschlüsselung verwenden

Linux Unified Key Setup-on-disk-format (LUKS) gibt Ihnen die Möglichkeit, Partitionen auf Ihrem Red Hat Enterprise Linux 6 (Server) zu verschlüsseln. Dies ist bei tragbaren Computern und Wechseldatenträgern besonders wichtig. Mithilfe von LUKS können mehrere Benutzerschlüssel einen Masterschlüssel entschlüsseln, der zur Massenverschlüsselung der Partition verwendet wird.

## LUKS bietet folgende Möglichkeiten:

- Es verschlüsselt ganze Blockeinheiten und eignet sich daher gut für den Schutz der Inhalte von mobilen Geräten wie Wechselspeichermedien oder Plattenlaufwerken von Laptops.
    - Der zugrunde liegende Inhalt des verschlüsselten Blockgeräts ist beliebig. Daher ist es hilfreich beim Verschlüsseln von Auslagerungseinheiten. Die Verschlüsselung kann auch bei bestimmten Datenbanken hilfreich sein, die zur Datenspeicherung speziell formatierte Blockgeräte verwenden.
- Es nutzt das vorhandene Gerätemapperkernel-Subsystem.
- Es stellt eine Kennphrasestärkung bereit, die Schutz vor dem Anhängen von Wörterbüchern bietet.
- Es ermöglicht Benutzern, Sicherungsschlüssel oder Kennphrasen hinzuzufügen, da LUKS-Geräte mehrere Schlüsselslots enthalten.


## LUKS bietet folgende Möglichkeiten nicht:

- Es stellt für Anwendungen, die viele Benutzer (mehr als acht) erfordern, keine unterschiedlichen Zugriffsschlüssel auf dieselben Geräte bereit.
- Arbeit mit Anwendungen, für die eine Verschlüsselung auf Dateiebene erforderlich ist [weitere Informationen](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}.

## Informationen zum Einrichten neuer mit LUKS verschlüsselter Datenträger mit Endurance-{{site.data.keyword.blockstorageshort}}

Bei diesen Schritten wird angenommen, dass der Server bereits Zugriff auf einen neuen, nicht verschlüsselten {{site.data.keyword.blockstoragefull}}-Datenträger hat, der nicht formatiert oder angehängt wurde. Klicken Sie [hier](accessing_block_storage_linux.html), um Informationen zum Zugriff auf {{site.data.keyword.blockstorageshort}} mit Linux zu erhalten.

Beachten Sie, dass bei der Durchführung von Datenverschlüsselung eine Belastung auf dem Host erzeugt wird, die zu Leistungseinbußen führen kann.

1. Machen Sie an einer Shelleingabeaufforderung als Root die folgenden Eingaben, um das erforderliche Paket zu installieren:   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Rufen Sie die Platten-ID ab:<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Suchen Sie Ihren Datenträger in der Liste.
4. Verschlüsseln Sie das Blockgerät. 
      1. Dieser Befehl initialisiert den Datenträger und gibt Ihnen die Möglichkeit, eine Kennphrase festzulegen:<br/>
      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}
      
      2. Antworten Sie mit YES (in Großbuchstaben).
      
      3. Nun wird das Gerät als verschlüsselter Datenträger angezeigt: 
      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```
      {: pre}
      
5. Öffnen Sie den Datenträger und erstellen Sie eine Zuordnung:   <br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Geben Sie das zuvor bereitgestellte Kennwort ein.
7. Überprüfen Sie die Zuordnung und zeigen Sie den Status des verschlüsselten Datenträgers an:   <br/>
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
8. Schreiben Sie Zufallsdaten an das verschlüsselte Gerät unter /dev/mapper/cryptData. Damit wird sichergestellt, dass externe Benutzer sehen, dass dies Zufallsdaten sind und das Gerät somit gegen die Weitergabe von Verwendungsmustern geschützt ist. Beachten Sie, dass dieser Schritt eine Weile dauern kann.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Formatieren Sie den Datenträger:<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Hängen Sie den Datenträger an:<br/>
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

### Vorgehensweise zum sicheren Abhängen und Schließen des verschlüsselten Datenträgers
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Vorgehensweise zum erneuten Anhängen und Anhängen einer vorhandenen mit LUKS verschlüsselten Partition
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Geben Sie das zuvor bereitgestellte Kennwort ein.
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
