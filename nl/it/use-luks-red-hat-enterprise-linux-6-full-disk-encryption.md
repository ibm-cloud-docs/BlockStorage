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

# Ottenere la crittografia totale del disco con LUKS in Red Hat Enterprise Linux

Puoi crittografare le partizioni sul tuo server Red Hat Enterprise Linux 6 con il formato su disco LUKS (Linux Unified Key Setup), cosa importante nel caso di computer mobili e supporti rimovibili. LUKS consente a più chiavi utente di decrittografare una chiave master che viene utilizzata per la crittografia di massa della partizione.

Questa procedura presuppone che il server abbia accesso a un nuovo volume {{site.data.keyword.blockstoragefull}} non crittografato che non è stato formattato o montato. Per ulteriori informazioni sulla connessione di {{site.data.keyword.blockstorageshort}} a un host Linux, consulta [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html).

Il {site.data.keyword.blockstorageshort}} di cui è stato eseguito il provisioning in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html) viene automaticamente fornito con la crittografia dei dati inattivi gestita dal provider. Per ulteriori informazioni, consulta [Protezione dei tuoi dati - crittografia dei dati inattivi gestita dal provider](block-file-storage-encryption-rest.html).
{:note}

## Cosa fa LUKS

- Crittografa interi dispositivi a blocchi ed è pertanto adatto per proteggere i contenuti di dispositivi mobili quali i supporti di archiviazione rimovibili o le unità disco dei notebook.
- I contenuti sottostanti del dispositivo a blocchi crittografato sono arbitrari, rendendolo utile per la crittografia di dispositivi di swap. La crittografia può anche essere utile con specifici database che usano dispositivi a blocchi con una formattazione speciale per l'archiviazione di dati.
- Utilizza il sottosistema kernel di device mapper esistente.
- Fornisce il rafforzamento delle passphrase, che protegge dagli attacchi di dizionario.
- Consente agli utenti di aggiungere delle chiavi di backup o delle passphrase perché i dispositivi LUKS contengono più slot della chiave.


## Cosa non fa LUKS

- Consente alle applicazioni che richiedono molti utenti (più di otto) di avere delle chiavi di accesso distinte agli stessi dispositivi.
- Lavora con le applicazioni che richiedono la crittografia a livello di file. Per ulteriori informazioni, consulta il manuale [RHEL Security Guide ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}.

## Configurazione di un volume crittografato LUKS con {{site.data.keyword.blockstorageshort}} Endurance

L'elaborazione della crittografia dei dati crea un carico sull'host che potrebbe, potenzialmente, avere un impatto sulle prestazioni.
{:note}

1. Immetti il seguente comando in un prompt della shell come root per installare il pacchetto richiesto:   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Ottieni l'ID disco:<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Individua il tuo volume nell'elenco.
4. Crittografa il dispositivo a blocchi:

   1. Questo comando inizializza il volume e puoi impostare una passphrase. <br/>

      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}

   2. Rispondi con YES (tutto in lettere maiuscole).

   3. Il dispositivo viene ora visualizzato come un volume crittografato:

      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```

5. Apri il volume e crea un'associazione.<br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Immetti la passphrase.
7. Verificare l'associazione e lo stato della vista del volume crittografato.   <br/>
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
8. Scrivi dati casuali in `/dev/mapper/cryptData` sul dispositivo crittografato. Questa azione garantisce che il mondo esterno li vede come dati casuali; questo significa che sono protetti dalla diffusione di modelli di utilizzo. Questo passo può richiedere del tempo.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. Formatta il volume.<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Monta il volume.<br/>
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

### Smontaggio e chiusura del volume crittografato in modo sicuro
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Rimontaggio e montaggio di una partizione con crittografia LUKS esistente
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
