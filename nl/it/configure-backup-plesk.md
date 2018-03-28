---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurazione di {{site.data.keyword.blockstorageshort}} per il backup con Plesk

In questo articolo, intendiamo fornire le istruzioni per la configurazione di {{site.data.keyword.blockstoragefull}} per i backup in Plesk. Il presupposto è che siano disponibili SSH root o sudo e un accesso a Plesk a livello di amministrazione completo. Questo esempio è basato su un host CentOS7.

**Nota**: puoi trovare la documentazione di Plesk per il backup e il ripristino [qui](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Connettiti all'host tramite SSH.

2. Assicurati che esiste una destinazione di punto di montaggio. <br />
   **Nota**: Plesk ha due opzioni per l'archiviazione dei backup; una è l'archiviazione Plesk interna, ossia un'archiviazione di backup che si trova sul tuo server Plesk, e l'altra è un'archiviazione FTP interna, ossia un'archiviazione di backup che si trova su qualche server esterno nel web o nella tua rete locale. Di norma, sui box Plesk, i backup interni sono archiviati in `/var/lib/psa/dumps` e utilizzano `/tmp` come directory temporanea. Nel nostro esempio, teniamo la directory temporanea locale ma spostiamo la directory di dump alla destinazione STaas (`/backup/psa/dumps`). Non sono necessarie credenziali utente FTP.
   
3. Configura il tuo {{site.data.keyword.blockstorageshort}} come descritto in [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html). Monta {{site.data.keyword.blockstorageshort}} in `/backup` e configura `/etc/fstab` per abilitare il montaggio all'avvio del computer.

4. **Facoltativo**: copia i backup esistenti nella nuova archiviazione. Usa `rsync`; ad esempio:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Nota**: questo comando trasmette i tuoi dati compressi preservando al tempo stesso tutto quanto è possibile (fatta eccezione per i collegamenti reali) e fornire informazioni su quali file sono in fase di trasferimento, nonché un breve riepilogo finale.
    
5. Modifica `/etc/psa/psa.conf` in modo che punti al valore `DUMP_D` sulla nuova destinazione. 
    -  Dovrebbe presentarsi come: `DUMP_D /backup/psa/dumps`. 

6. **Facoltativo**: come dettato dal tuo specifico caso d'uso e dalle tue esigenze aziendali, rimuovi la vecchia archiviazione dal server e annulla dall'account.


