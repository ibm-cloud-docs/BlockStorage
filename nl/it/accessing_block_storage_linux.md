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

# Connessione ai LUN iSCSI MPIO su Linux

Queste istruzioni sono relative a RHEL6/Centos6. Abbiamo aggiunto delle note per altri sistemi operativi ma questa documentazione **non** copre tutte le distribuzioni di Linux. Se stai utilizzando altri sistemi operativi Linux, fai riferimento alla documentazione della tua specifica distribuzione e assicurati che il multipath supporti ALUA per la priorità di percorso. Ad esempio, puoi trovare le istruzioni di Ubuntu per la configurazione dell'iniziatore iSCSI [qui](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} e per la configurazione di DM-Multipath [qui](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.

Prima di iniziare, assicurati che l'host che accede al volume {{site.data.keyword.blockstoragefull}} sia stato autorizzato tramite il [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. Dalla pagina di elenco {{site.data.keyword.blockstorageshort}}, fai clic su **Actions** associato al nuovo volume.
2. Fai clic su **Authorize Host**.
3. Dall'elenco, seleziona l'host o gli host che devono poter accedere al volume e fai clic su **Submit**.

## Montaggio di volumi {{site.data.keyword.blockstorageshort}}

Viene qui di seguito indicata la procedura necessaria per connettere un'istanza di elaborazione {{site.data.keyword.BluSoftlayer_full}} basata su Linux a un LUN (logical unit number) iCSCI (Internet Small Computer System Interface) MPIO (multipath input/output).

**Nota:** l'IQN host, il nome utente, la password e l'indirizzo di destinazione a cui si fa riferimento nelle istruzioni possono essere ottenuti dalla schermata **{{site.data.keyword.blockstorageshort}}** Details nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Nota:** consigliamo di eseguire il traffico di archiviazione su una VLAN che ignora il firewall. L'esecuzione del traffico di archiviazione tramite i firewall software aumenterà la latenza e avrà un impatto negativo sulle prestazioni dell'archiviazione.

1. Installa iSCSI e i programmi di utilità multipath sul tuo host:
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

2. Crea o modifica il file di configurazione multipath.
   - Modifica **/etc/multipath.conf** con la configurazione minima fornita nei seguenti comandi. <br /><br /> **Nota:** tieni presente che, per RHEL7/CentOS7, `multipath.conf` può essere vuoto perché il sistema operativo ha delle configurazioni integrate. Ubuntu non usa multipath.conf poiché è integrato in multipath-tools.

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

3. Carica il modulo multipath, avvia i servizi multipath ed impostane l'avvio all'avvio del computer.
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

   - Per altre distribuzioni, consulta la documentazione del fornitore del sistema operativo.

4. Verifica che multipath stia funzionando.
   - RHEL 6:
     ```
     multipath -l
     ```
     {: pre}

     Se ora restituisce un valore vuoto, sta funzionando.

   - CentOS 7:
     ```
     multipath -ll
     ```
     {: pre}

     RHEL 7/CentOS 7 potrebbe restituire "No fc_host device", che può essere ignorato.

5. Aggiorna il file **/etc/iscsi/initiatorname.iscsi** con l'IQN dal {{site.data.keyword.slportal}}. Immetti il valore in minuscole.
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. Modifica le impostazioni CHAP in **/etc/iscsi/iscsid.conf** utilizzando il nome utente e la password dal {{site.data.keyword.slportal}}. Usa le maiuscole per i nomi CHAP.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   **Nota:** lascia le altre impostazioni CHAP come commenti. L'archiviazione {{site.data.keyword.BluSoftlayer_full}} utilizza solo un'autenticazione unidirezionale.

7. Imposta iSCSI per l'avvio all'avvio del computer e avvialo ora.
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

   - Altre distribuzioni: consulta la documentazione del fornitore del sistema operativo.

8. Rileva il dispositivo utilizzando l'indirizzo IP di destinazione ottenuto dal {{site.data.keyword.slportal}}.

     a. Esegui il rilevamento sull'array iSCSI:
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     b. Imposta l'host in modo che esegua automaticamente l'accesso all'array iSCSI:
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

   In questo modo, dovrebbero ora essere riportati i percorsi.

10. Verifica che il dispositivo sia connesso.  Per impostazione predefinito, il dispositivo si collegherà a /dev/mapper/mpathX, dove X è l'ID generato del dispositivo connesso.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Dovrebbe riportare qualcosa di simile a quanto segue,
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 byte
    ```
  Il volume è ora montato e accessibile sull'host.

## Crea un file system (facoltativo)

Viene di seguito riportata la procedura per creare un file system sul volume appena montato. Un file system è necessario perché la maggior parte delle applicazioni utilizzi il volume. Usa **fdisk** per le unità inferiori a 2 TB e **parted** per un disco di dimensione superiore a 2 TB.

### fdisk

1. Ottieni il nome del disco.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   Il nome del disco restituito sarà simile a /dev/mapper/XXX.

2. Crea una partizione sul disco utilizzando fdisk.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX rappresenta il nome del disco restituito nel passo 1. <br />

   **Nota**: scorri ancora più in basso per i codici dei comandi elencati nella Tabella dei comandi fdisk sotto questa sezione.

3. Crea un file system sulla nuova partizione.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - La nuova partizione dovrebbe essere elencata con il disco, simile a XXXlp1, seguita dalla dimensione, dal tipo (Type) (83) e da Linux.
   - Prendi nota del nome della partizione; ti servirà nel passo successivo. (XXXlp1 rappresenta il nome della partizione).
   - Crea il file system:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Crea un punto di montaggio per il file system e montalo.
   - Crea un nome partizione PerfDisk oppure dove vuoi montare il file system:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Utilizzando il nome partizione, monta l'archiviazione:
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifica che vedi elencato il tuo nuovo file system:
     ```
     df -h
     ```
     {: pre}

5. Aggiungi il nuovo filesystem al file **/etc/fstab** del sistema per abilitare il montaggio automatico all'avvio del computer.
   - Accoda la seguente riga in fondo a **/etc/fstab** (utilizzando il nome partizione dal passo 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Tabella dei comandi fdisk



<table border="0" cellpadding="0" cellspacing="0">
  <caption>La tabella comandi fdisk contiene i comandi sulla sinistra e i risultati previsti sulla destra.</caption>
    <thead>
	<tr>
		<th style="width:40%;">Comando</th>
		<th style="width:60%;">Risultato</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Command: n</code></td>
		<td>Crea una nuova partizione. &#42;</td>
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
		<td>Premere Invio per andare all'ultimo cilindro.</td>
	</tr>
	<tr>
		<td><code>Comando: t</code></td>
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
		<td><code>Comando: w</code></td>
		<td>Scrive le informazioni sulla nuova partizione sul disco. &#42;</td>
	</tr>
   </tbody>
</table>

  (`*`)Type m for Help.

  (`**`)Type L to list the hex codes

### parted

Per molte distribuzioni Linux, **parted** è preinstallato. Se non è incluso nel tuo distro, puoi installarlo con:
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


Per creare un file system con **parted** attieniti alla seguente procedura:

1. Esegui parted.

   ```
   parted
   ```
   {: pre}

2. Crea una partizione sul disco.
   1. Se non specificato diversamente, parted utilizzerà la tua unità primaria che, nella maggior parte dei casi, è **/dev/sda**. Passa al disco che vuoi partizionare utilizzando il comando **select**. Sostituisci **XXX** con il tuo nuovo nome dispositivo.

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. Esegui **print** per confermare che sei sul disco corretto.

      ```
      (parted) print
      ```
      {: pre}

   3. Crea una nuova tabella partizioni GPT.

      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. Puoi usare parted per creare partizioni disco primarie e logiche; la procedura prevista è la stessa. Per creare una nuova partizione, parted utilizza **mkpart**. Puoi dargli dei parametri aggiuntivi come **primary** oppure **logical**, a seconda del tipo di partizione che vuoi creare.
   <br /> **Nota**: le unità elencate hanno come valore predefinito i megabyte (MB); per creare una partizione da 10 GB devi iniziare da 1 e finire a 10000. Puoi anche modificare le unità di dimensione in terabyte immettendo `(parted) unit TB`, se vuoi.

      ```
      (parted) mkpart
      ```
      {: pre}

   5. Esci da parted con **quit**.

      ```
      (parted) quit
      ```
      {: pre}

3. Crea un file system sulla nuova partizione.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **Nota**: è importante selezionare il disco e la partizione corretti quando esegui il comando sopra indicato!
   Verifica il risultato stampando la tabella partizioni. Nella colonna del file system, dovresti vedere ext3.

4. Crea un punto di montaggio per il file system e montalo.
   - Crea un nome partizione PerfDisk oppure dove vuoi montare il file system:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Utilizzando il nome partizione, monta l'archiviazione:

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifica che vedi elencato il tuo nuovo file system:

     ```
     df -h
     ```
     {: pre}

5. Aggiungi il nuovo filesystem al file **/etc/fstab** del sistema per abilitare il montaggio automatico all'avvio del computer.
   - Accoda la seguente riga in fondo a **/etc/fstab** (utilizzando il nome partizione dal passo 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## Come Verificare se MPIO è configurato correttamente nei sistemi operativi *NIX

Per controllare se multipath sta rilevando i dispositivi, dovrebbero essere visualizzati solo i dispositivi NETAPP e ce ne dovrebbero essere due.

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

Controlla che i dischi siano presenti; ci dovrebbero essere due dischi con lo stesso identificativo e un elenco /dev/mapper della stessa dimensione con lo stesso identificativo. Il dispositivo /dev/mapper è il solo che viene configurato da multipath:

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

Se non è configurato correttamente, apparirà in questo modo:
```
No multipath output root@server:~# multipath -l root@server:~#
```

Questo visualizzerà i dispositivi inseriti nella blacklist:
```
# multipath -l -v 3 | grep sd <date and time>
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

fdisk visualizza solo i dispositivi `sd*` e nessun `/dev/mapper`

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
