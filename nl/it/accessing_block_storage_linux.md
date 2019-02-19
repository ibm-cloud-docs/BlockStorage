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


# Connessione ai LUN iSCSI su Linux
{: #mountingLinux}

Queste istruzioni sono principalmente per RHEL6 e Centos6. Sono state aggiunte delle note per altri sistemi operativi ma questa documentazione **non** copre tutte le distribuzioni di Linux. Se stai utilizzando altri sistemi operativi Linux, fai riferimento alla documentazione della tua specifica distribuzione e assicurati che il multipath supporti ALUA per la priorità di percorso.
{:note}

Ad esempio, puoi trovare le istruzioni di Ubuntu per la configurazione dell'iniziatore iSCSI [qui ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} e per la configurazione di DM-Multipath [qui ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.
{: tip}

Prima di iniziare, assicurati che l'host che sta accedendo al volume {{site.data.keyword.blockstoragefull}} sia stato precedentemente autorizzato tramite il [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.
{:important}

1. Dalla pagina di elenco {{site.data.keyword.blockstorageshort}}, individua il nuovo volume e fai clic su **Actions**.
2. Fai clic su **Authorize Host**.
3. Dall'elenco, seleziona l'host o gli host che possono accedere al volume e fai clic su **Submit**.

In alternativa, puoi autorizzare l'host tramite la CLI SL.
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```
{:codeblock}

## Montaggio di volumi {{site.data.keyword.blockstorageshort}}

Viene qui di seguito indicata la procedura necessaria per connettere un'istanza di elaborazione {{site.data.keyword.BluSoftlayer_full}} basata su Linux a un LUN (logical unit number) iCSCI (internet Small Computer System Interface) MPIO (multipath input/output).

L'IQN host, il nome utente, la password e l'indirizzo di destinazione a cui si fa riferimento nelle istruzioni possono essere ottenuti dalla schermata **{{site.data.keyword.blockstorageshort}} Details** nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.
{: tip}

Consigliamo di eseguire il traffico di archiviazione su una VLAN che ignora il firewall. L'esecuzione del traffico di archiviazione tramite i firewall software aumenta la latenza e ha un impatto negativo sulle prestazioni dell'archiviazione.
{:important}

1. Installa iSCSI e i programmi di utilità multipath sul tuo host.
  - RHEL e CentOS
     ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu e Debian

    ```
    sudo apt-get update
   sudo apt-get install multipath-tools
    ```
    {: pre}

2. Crea o modifica il file di configurazione multipath se necessario.
  - RHEL 6 e CENTOS 6
    * Modifica **/etc/multipath.conf** con la seguente configurazione minima.

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

    - Riavvia i servizi `iscsi` e `iscsid` in modo che le modifiche abbiano effetto.

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - RHEL7 e CentOS7, `multipath.conf` può essere vuoto perché il sistema operativo ha delle configurazioni integrate.
  - Ubuntu non utilizza `multipath.conf` poiché è integrato in `multipath-tools`.

3. Carica il modulo multipath, avvia i servizi multipath ed impostane l'avvio all'avvio del computer.
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

  - Per altre distribuzioni, controlla la documentazione del fornitore del sistema operativo.

4. Verifica che multipath stia funzionando.
  - RHEL 6
    ```
    multipath -l
    ```
    {: pre}

    Se restituisce un valore vuoto, sta funzionando.
  - CentOS 7
    ```
    multipath -ll
    ```
    {: pre}

    RHEL 7 e CentOS 7 potrebbero restituire "No fc_host device", che può essere ignorato.

5. Aggiorna il file `/etc/iscsi/initiatorname.iscsi` con l'IQN dal {{site.data.keyword.slportal}}. Immetti il valore con lettere minuscole.
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. Modifica le impostazioni CHAP in `/etc/iscsi/iscsid.conf` utilizzando il nome utente e la password dal {{site.data.keyword.slportal}}. Usa le maiuscole per i nomi CHAP.
   ```
   node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   Lascia le altre impostazioni CHAP come commenti. L'archiviazione {{site.data.keyword.BluSoftlayer_full}} utilizza solo un'autenticazione unidirezionale. Non abilitare Mutual CHAP.
   {:important}

   Nota per gli utenti di Ubuntu: mentre state guardando il file `iscsid.conf`, verificate se l'impostazione `node.startup` è manual (manuale) o automatic (automatica). Se è manual, modificatela in automatic.
   {:tip}

7. Imposta iSCSI per l'avvio all'avvio del computer e avvialo ora.
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

   - Per altre distribuzioni, controlla la documentazione del fornitore del sistema operativo.

8. Rileva il dispositivo utilizzando l'indirizzo IP di destinazione ottenuto dal {{site.data.keyword.slportal}}.

   A. Esegui il rilevamento sull'array iSCSI.
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
    ```
    {: pre}

   B. Imposta l'host in modo che esegua automaticamente l'accesso all'array iSCSI.
    ```
    iscsiadm -m node -L automatic
    ```
    {: pre}

9. Verifica che l'host abbia eseguito l'accesso all'array iSCSI e mantenuto le sue sessioni.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Questo comando riporta i percorsi.

10. Verifica che il dispositivo sia connesso. Per impostazione predefinita, il dispositivo si collega a `/dev/mapper/mpathX` dove X è l'ID generato del dispositivo connesso.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Questo comando restituisce qualcosa simile a questo esempio.
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```
  Il volume è ora montato e accessibile sull'host.

## Creazione di un file system (facoltativo)

Segui questa procedura per creare un file system sul volume appena montato. Un file system è necessario perché la maggior parte delle applicazioni utilizzi il volume. Utilizza [`fdisk` per le unità inferiori a 2 TB](#fdisk) e [`parted` per un disco di dimensioni superiori a 2 TB](#parted).

### Creazione di un file system con `fdisk`
{: #fdisk}

1. Ottieni il nome del disco.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   Il nome del disco restituito è simile a `/dev/mapper/XXX`.

2. Crea una partizione sul disco.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX rappresenta il nome del disco restituito nel passo 1. <br />

   Scorri ancora più in basso per i codici dei comandi elencati nella Tabella dei comandi `fdisk`.
   {: tip}

3. Crea un file system sulla nuova partizione.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - La nuova partizione viene elencata con il disco, simile a `XXXlp1`, seguita dalla dimensione, dal tipo (Type) (83) e da Linux.
   - Prendi nota del nome della partizione; ti serve nel passo successivo. (XXXlp1 rappresenta il nome della partizione).
   - Crea il file system:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Crea un punto di montaggio per il file system e montalo.
   - Crea un nome partizione `PerfDisk` oppure dove vuoi montare il file system.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Monta la memoria con il nome della partizione.
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifica che vedi elencato il tuo nuovo file system.
     ```
     df -h
     ```
     {: pre}

5. Aggiungi il nuovo file system al file `/etc/fstab` del sistema per abilitare il montaggio automatico all'avvio del computer.
   - Accoda la seguente riga alla fine di `/etc/fstab` (con il nome partizione dal passo 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### La tabella dei comandi `fdisk`

<table border="0" cellpadding="0" cellspacing="0">
	<caption>La tabella comandi <code>fdisk</code> contiene i comandi sulla sinistra e i risultati previsti sulla destra.</caption>
    <thead>
	<tr>
		<th style="width:40%;">Comando</th>
		<th style="width:60%;">Risultato</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Command: n</code></td>
		<td>Crea una partizione. &#42;</td>
	</tr>
	<tr>
		<td><code>Command action: p</code></td>
		<td>Rende primaria la partizione.</td>
	</tr>
	<tr>
		<td><code>Partition number (1-4): 1</code></td>
		<td>Diventa la partizione 1 sul disco.</td>
	</tr>
	<tr>
		<td><code>First cylinder (1-8877): 1 (default)</code></td>
		<td>Inizia al cilindro 1.</td>
	</tr>
	<tr>
		<td><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></td>
		<td>Premi Invio per andare all'ultimo cilindro.</td>
	</tr>
	<tr>
		<td><code>Command: t</code></td>
		<td>Configura il tipo di partizione. &#42;</td>
	</tr>
	<tr>
		<td><code>Select partition 1.</code></td>
		<td>Seleziona la partizione 1 da configurare come un tipo specifico.</td>
	</tr>
	<tr>
		<td><code>Codice esadecimale: 83</code></td>
		<td>Seleziona Linux come tipo (Type) (83 è il codice esadecimale per Linux).&#42;&#42;</td>
	 </tr>
	<tr>
		<td><code>Command: w</code></td>
		<td>Scrive le informazioni sulla nuova partizione sul disco. &#42;</td>
	</tr>
   </tbody>
</table>

  (`*`)Type m for Help.

  (`**`)Type L to list the hex codes

### Creazione di un file system con `parted`
{: #parted}

Per molte distribuzioni Linux, `parted` è preinstallato. Se non è incluso nel tuo distro, puoi installarlo con:
- Debian e Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL e CentOS
  ```
  yum install parted  
  ```
  {: pre}


Per creare un file system con `parted`, attieniti alla seguente procedura.

1. Esegui `parted`.

   ```
   parted
   ```
   {: pre}

2. Crea una partizione sul disco.
   1. Se non specificato diversamente, `parted` utilizza la tua unità primaria che, nella maggior parte dei casi, è `/dev/sda`. Passa al disco che vuoi partizionare utilizzando il comando **select**. Sostituisci **XXX** con il tuo nuovo nome dispositivo.

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Esegui `print` per confermare che sei sul disco corretto.

      ```
      print
      ```
      {: pre}

   3. Crea una tabella partizioni GPT.

      ```
      mklabel gpt
      ```
      {: pre}

   4. Puoi usare `Parted` per creare partizioni disco primarie e logiche; la procedura prevista è la stessa. Per creare una partizione, `parted` utilizza `mkpart`. Puoi dargli dei parametri aggiuntivi come **primary** oppure **logical**, a seconda del tipo di partizione che vuoi creare.<br />

   Le unità elencate hanno come valore predefinito i megabyte (MB). Per creare una partizione da 10-GB, inizia da 1 e finisci a 10000. Puoi anche modificare le unità di dimensione in terabyte immettendo `unit TB`, se vuoi.
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. Esci da `parted` con `quit`.

      ```
      quit
      ```
      {: pre}

3. Crea un file system sulla nuova partizione.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   È importante selezionare il disco e la partizione corretti quando esegui questo comando.<br />Verifica il risultato stampando la tabella partizioni. Nella colonna del file system, puoi vedere ext3.
   {:important}

4. Crea un punto di montaggio per il file system e montalo.
   - Crea un nome partizione `PerfDisk` oppure dove vuoi montare il file system.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Monta la memoria con il nome della partizione.

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifica che vedi elencato il tuo nuovo file system.

     ```
     df -h
     ```
     {: pre}

5. Aggiungi il nuovo file system al file `/etc/fstab` del sistema per abilitare il montaggio automatico all'avvio del computer.
   - Accoda la seguente riga alla fine di `/etc/fstab` (utilizzando il nome partizione dal passo 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## Verifica della configurazione MPIO
{: #verifyMPIOLinux}

1. Per controllare se multipath sta rilevando i dispositivi, elencali. Se è stato configurato correttamente, vengono visualizzati solo due dispositivi NETAPP.

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

2. Controlla che siano presenti i dischi. Ci devono essere due dischi con lo stesso identificativo e un elenco `/dev/mapper` della stessa dimensione con lo stesso identificativo. Il dispositivo `/dev/mapper` è il solo che viene configurato da multipath.
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - Output di esempio di una configurazione corretta.

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - Output di esempio di una configurazione non corretta.

    ```
    No multipath output root@server:~# multipath -l root@server:~#
    ```

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. Conferma che i dischi locali non sono inclusi nei dispositivi a più percorsi. Il seguente comando mostra i dispositivi inseriti nella blacklist.
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

## Smontaggio dei volumi {{site.data.keyword.blockstorageshort}}
{: #unmounting}

1. Smonta il file system.
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. Se non disponi di altri volumi in quel portale di destinazione, puoi scollegarti dalla destinazione.
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. Se non disponi di altri volumi in quel portale di destinazione, elimina il record del portale di destinazione per impedire tentativi di login futuri.
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   Per ulteriori informazioni, consulta il [manuale di `iscsiadm` ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://linux.die.net/man/8/iscsiadm).
   {:tip}
