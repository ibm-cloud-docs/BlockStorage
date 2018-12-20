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

# Vollständige Plattenverschlüsselung mit LUKS in Red Hat Enterprise Linux erzielen

Sie können Partitionen auf dem Red Hat Enterprise Linux 6-Server im LUKS-Plattenformat (LUKS - Linux Unified Key Setup-on-disk-format) verschlüsseln, was für tragbare Computer und und Wechseldatenträger von Bedeutung ist. Mithilfe von LUKS können mehrere Benutzerschlüssel einen Masterschlüssel entschlüsseln, der zur Massenverschlüsselung der Partition verwendet wird.

Bei diesen Schritten wird angenommen, dass vom Server auf einen neuen, nicht verschlüsselten {{site.data.keyword.blockstoragefull}}-Datenträger zugegriffen werden kann, der nicht formatiert oder angehängt wurde. Weitere Informationen zum Herstellen einer Verbindung von {{site.data.keyword.blockstorageshort}} zu einem Linux-Host finden Sie unter [Verbindung zu MPIO-iSCSI-LUNs unter Linux herstellen](accessing_block_storage_linux.html).

{site.data.keyword.blockstorageshort}} wird in [ausgewählten Rechenzentren](new-ibm-block-and-file-storage-location-and-features.html) automatisch durch providerseits verwaltete Verschlüsselung ruhender Daten bereitgestellt. Weitere Informationen finden Sie im Abschnitt [Daten schützen - durch providerseits verwaltete Verschlüsselung ruhender Daten](block-file-storage-encryption-rest.html).{:note}

## Möglichkeiten bei Verwendung von LUKS

- Es verschlüsselt ganze Blockgeräte und eignet sich daher gut für den Schutz der Inhalte von mobilen Geräten wie Wechselspeichermedien oder Plattenlaufwerken von Notebooks.
- Der zugrunde liegende Inhalt des verschlüsselten Blockgeräts ist beliebig. Daher ist es hilfreich beim Verschlüsseln von Auslagerungseinheiten. Die Verschlüsselung kann auch bei bestimmten Datenbanken hilfreich sein, die zur Datenspeicherung speziell formatierte Blockgeräte verwenden.
- Es nutzt das vorhandene Subsystem des Geräte-Mapper-Kernels.
- Es stellt eine Kennphrasenstärkung bereit, die Schutz vor dem Anhängen von Wörterbüchern bietet.
- Es ermöglicht Benutzern, Sicherungsschlüssel oder Kennphrasen hinzuzufügen, da LUKS-Geräte mehrere Schlüsselslots enthalten.


## Was LUKS nicht bietet

- Bereitstellung unterschiedlicher Zugriffsschlüssel für dieselben Geräte in Anwendungen, in denen dies für viele Benutzer (mehr als acht) erforderlich ist
- Arbeit mit Anwendungen, für die eine Verschlüsselung auf Dateiebene erforderlich ist, [weitere Informationen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}.

## Mit LUKS verschlüsselten Datenträger mit Endurance für {{site.data.keyword.blockstorageshort}} konfigurieren

Der Prozess der Datenverschlüsselung führt zu einer Belastung des Hosts, die möglicherweise die Leistung beeinträchtigt.
{:note}

1. Geben Sie den folgenden Befehl an einer Shelleingabeaufforderung als Root ein, um das erforderliche Paket zu installieren:   <br/>
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

   1. Dieser Befehl initialisiert den Datenträger und Sie können eine Kennphrase festlegen. <br/>

      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. Antworten Sie mit YES (in Großbuchstaben).

   3. Das Gerät wird jetzt als verschlüsselter Datenträger angezeigt:

      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```

5. Öffnen Sie den Datenträger und erstellen Sie eine Zuordnung.<br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Geben Sie die Kennphrase ein.
7. Überprüfen Sie die Zuordnung und zeigen Sie den Status des verschlüsselten Datenträgers an.   <br/>
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
8. Schreiben Sie Zufallsdaten auf das verschlüsselt Gerät in `/dev/mapper/cryptData`. Durch diese Aktion wird sichergestellt, dass diese Daten als Zufallsdaten angesehen werden, was bedeutet, dass sie gegen die Weitergabe von Verwendungsmustern geschützt sind. Dieser Schritt kann eine Weile dauern.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Formatieren Sie den Datenträger.<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Hängen Sie den Datenträger an.<br/>
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

### Verschlüsselten Datenträger sicher abhängen und schließen
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Vorhandene verschlüsselte LUKS-Partition erneut anhängen und anhängen
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
