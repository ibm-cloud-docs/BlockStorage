/---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

parole chiave: Block storage, cPanel, backup, punto di montaggio, ISCSI

sottoraccolta: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurazione di {{site.data.keyword.blockstorageshort}} per il backup con cPanel
{: #cPanelBackups}

Puoi utilizzare le seguenti istruzioni per configurare i tuoi backup in cPanel da archiviare in {{site.data.keyword.blockstoragefull}}. Il presupposto è che siano disponibili SSH root o sudo e un accesso VHM (WebHost Manager) completo. Queste istruzioni sono basate su un host **CentOS 7**.

Per ulteriori informazioni, vedi [cPanel - Configuring backup directory](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external}.
{:tip}

1. Connettiti all'host tramite SSH.

2. Assicurati che esista una destinazione di punto di montaggio. <br />
   Per impostazione predefinita, il sistema cPanel salva i file di backup in locale, nella directory `/backup`. Ai fini di questo documento, si presuppone che `/backup` esista e che contenga dei backup e viene utilizzato `/backup2` come nuovo punto di montaggio.
   {:note}

3. Configura il tuo {{site.data.keyword.blockstorageshort}} come descritto in [Connessione ai LUN iSCSI su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Assicurati di montarlo a `/backup2` e di configurarlo in `/etc/fstab` per abilitare il montaggio all'avvio del computer.

4. **Facoltativo**: copia i backup esistenti nella nuova archiviazione. Puoi utilizzare `rsync`.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Questo comando comprime e trasmette i tuoi dati preservando al tempo stesso tutto quanto è possibile, fatta eccezione per i collegamenti reali. Fornisce informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo alla fine.
    {:tip}

5. Accedi a WHM e passa alla configurazione di backup facendo clic su **Home** > **Backup** > **Backup Configuration**.

6. Modifica la configurazione per salvare i backup nel nuovo punto di montaggio.
    - Modifica la directory di backup predefinita immettendo il percorso assoluto alla nuova ubicazioni al posto della directory /backup/.
    - Seleziona **Enable to mount a backup drive**. Questa impostazione fa in modo che il processo di configurazione del backup controlli il file `/etc/fstab` per rilevare un montaggio di backup (`/backup2`). <br />

    Se esiste un montaggio con lo stesso nome della directory di staging, il processo di configurazione del backup monta l'unità e ne esegue il backup delle informazioni. Al suo termine del processo di backup, smonta l'unità.
    {:note}

7. Applica le modifiche facendo clic su **Save Configuration**.

8. **Facoltativo**: come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovere la vecchia archiviazione dal server e annulla dall'account.
