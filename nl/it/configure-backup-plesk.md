---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurazione di {{site.data.keyword.blockstorageshort}} per il backup con Plesk

Puoi utilizzare le seguenti istruzioni per configurare {{site.data.keyword.blockstoragefull}} per i tuoi backup in Plesk. Il presupposto è che siano disponibili SSH root o sudo e un accesso a Plesk a livello di amministrazione completo. Queste istruzioni sono basate su un host CentOS7.

Per ulteriori informazioni, consulta la [documentazione di Plesk per il backup e il ripristino ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.
{:tip}

1. Connettiti all'host tramite SSH.
2. Assicurati che esista una destinazione di punto di montaggio.

   Plesk ha due opzioni per archiviare i backup. Un'opzione è l'archiviazione Plesk interna (un'archiviazione di backup sul tuo server Plesk). L'altra opzione è un'archiviazione FTP esterna (un'archiviazione di backup su qualche server esterno nel web o nella tua rete locale). Di norma, sui box Plesk, i backup interni sono archiviati in `/var/lib/psa/dumps` e utilizzano `/tmp` come directory temporanea. In questo esempio, la directory temporanea viene tenuta locale ma spostiamo la directory dumps alla destinazione {{site.data.keyword.blockstorageshort}} (`/backup/psa/dumps`). Non sono necessarie credenziali utente FTP.
   {:note}   
3. Configura il tuo {{site.data.keyword.blockstorageshort}} come descritto in [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html). Monta {{site.data.keyword.blockstorageshort}} in `/backup` e configura `/etc/fstab` per abilitare il montaggio all'avvio del computer.
4. **Facoltativo**: copia i backup esistenti nella nuova archiviazione. Puoi utilizzare `rsync`.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    Questo comando comprime e trasmette i tuoi dati preservando al tempo stesso tutto quanto è possibile, fatta eccezione per i collegamenti reali. Fornisce informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo alla fine.
    {:tip}    
5. Modifica `/etc/psa/psa.conf` in modo che punti al valore `DUMP_D` sulla nuova destinazione.
    - Viene presentata come: `DUMP_D /backup/psa/dumps`.
6. **Facoltativo**: come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovere la vecchia archiviazione dal server e annulla dall'account.
