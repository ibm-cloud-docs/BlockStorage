---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurazione di {{site.data.keyword.blockstorageshort}} per il backup con cPanel

In questo articolo, intendiamo fornire le istruzioni per la configurazione dei tuoi backup da archiviare in {{site.data.keyword.blockstoragefull}} tramite cPanel. Il presupposto è che siano disponibili SSH root o sudo e un accesso VHM (WebHost Manager) completo. Questo esempio è basato su un host **CentOS 7**.

**Nota**: puoi trovare la documentazione di cPanel per configurare la directory di backup [qui](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Connettiti all'host tramite SSH.

2. Assicurati che esiste una destinazione di punto di montaggio. <br />
   **Nota**: per impostazione predefinita, il sistema cPanel salva i file di backup in locale, nella directory `/backup`. Ai fini di questo documento, presupponiamo che `/backup` esista già e che contenga dei backup, quindi useremo `/backup2` come nuovo punto di montaggio.
   
3. Configura il tuo {{site.data.keyword.blockstorageshort}} come descritto in [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html). Assicurati di montarlo a `/backup2` e di configurarlo in `/etc/fstab` per abilitare il montaggio all'avvio del computer.

4. **Facoltativo**: copia i backup esistenti nella nuova archiviazione. Usa `rsync`; ad esempio:
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Nota**: questo comando trasmette i tuoi dati compressi preservando al tempo stesso tutto quanto è possibile (fatta eccezione per i collegamenti reali) e fornire informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo finale.
    
5.  Accedi a WebHost Manager e passa alla configurazione di backup tramite **Home** > **Backup** > **Backup Configuration**.

6.  Modifica la configurazione per salvare i backup nel nuovo punto di montaggio. 
    - Modifica la directory di backup predefinita immettendo il percorso assoluto alla nuova ubicazioni al posto della directory /backup/. 
    - Seleziona **Enable to mount a backup drive**. Questa impostazione fa in modo che il processo di configurazione del backup controlli il file `/etc/fstab` per rilevare un montaggio di backup (`/backup2`). <br /> **Nota**: se esiste un montaggio con lo stesso nome della directory di staging, il processo di configurazione del backup monta l'unità ed esegue il backup delle informazioni al montaggio. Al suo termine del processo di backup, smonta l'unità. 

7. Applica le modifiche facendo clic su **Save Configuration** in fondo all'interfaccia **Backup Configuration**.

8. **Facoltativo**: come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovi la vecchia archiviazione dal server e annulla dall'account.

