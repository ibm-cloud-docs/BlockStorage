---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Upgrade di un {{site.data.keyword.blockstorageshort}} esistente a un {{site.data.keyword.blockstorageshort}} migliorato

Il {{site.data.keyword.blockstoragefull}} migliorato è ora disponibile in data center selezionati. per visualizzare l'elenco dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili quali i tassi di IOPS regolabili e i volumi espandibili, fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html). Per ulteriori informazioni sull'archiviazione crittografata gestita dal provider, consulta [Crittografia dei dati inattivi {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i LUN simultaneamente e trasferire i file direttamente da un LUN all'altro. Le specifiche dipendono dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Si presuppone che tu già abbia il tuo LUN non crittografato collegato al tuo host. In caso contrario, per svolgere tale attività, attieniti alle indicazioni adatte al tuo sistema operativo:

- [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html)
- [Connessione ai LUN iSCSI MPIO su CloudLinux](configure-iscsi-cloudlinux.html)
- [Connessione ai LUN iSCSI MPIO su Microsoft Windows](accessing-block-storage-windows.html)

## Creazione di nuovi {{site.data.keyword.blockstorageshort}}

Quando effettui un ordine con la API, specifica il pacchetto "Storage as a Service" per assicurarti che stai ottenendo le funzioni aggiornate con la tua nuova archiviazione.
{:important}

Le seguenti istruzioni servono ad ordinare un LUN migliorato tramite {{site.data.keyword.slportal}}. Il tuo nuovo LUN deve essere di dimensione pari o superiore a quella del volume originale per facilitare la migrazione.

### Ordine di un LUN Endurance

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. Nell'angolo superiore destro, fai clic su **Order {{site.data.keyword.blockstorageshort}}**.
3. Seleziona **Endurance** dall'elenco **Select Storage Type**.
4. Seleziona l'ubicazione (**Location**) (data center) della tua distribuzione.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione del volume precedente.
5. Seleziona la tua opzione di fatturazione. Puoi scegliere tra fatturazione oraria e mensile.
6. Seleziona il livello IOPS.
7. Fai clic su **Select Storage Size** e seleziona la tua dimensione di archiviazione dall'elenco.
8. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione di istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile.
   Per ulteriori informazioni sulle considerazioni sullo spazio dell'istantanea e sui suggerimenti, consulta [Ordinazione di istantanee](ordering-snapshots.html).
   {:tip}
9. Scegli il tuo tipo di sistema operativo (**OS Type**) dall'elenco.
10. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine.
11. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.

### Ordine di un LUN Performance

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. Sulla destra, fai clic su **Order {{site.data.keyword.blockstorageshort}}**.
3. Seleziona **Performance** dall'elenco **Select Storage Type**.
4. Fai clic su **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione degli host che hai ordinato precedentemente.
5. Seleziona la tua opzione di fatturazione. Puoi scegliere tra fatturazione oraria e mensile.
6. Seleziona il valore corretto per **Storage Size**.
7. Immetti l'IOPS nel campo **Specify IOPS**.
8. Fai clic su **Continue**. Ti vengono mostrati gli addebiti mensili e a base proporzionale con una possibilità finale di riesaminare i dettagli dell'ordine. Fai clic su **Previous** se vuoi modificare il tuo ordine.
9. Fai clic sulla casella di spunta **I have read the Master Service Agreement** e fai clic su **Place Order**.

Il provisioning dell'archiviazione viene eseguito in meno di un minuto ed è visibile sulla pagina {{site.data.keyword.blockstorageshort}} del {{site.data.keyword.slportal}}.



## Connessione del nuovo {{site.data.keyword.blockstorageshort}} all'host

Gli host "autorizzati" sono host a cui è stato concesso l'accesso a un volume. Senza l'autorizzazione host, non puoi accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo volume genera il nome utente, la password e l'IQN (iSCSI qualified name) necessari per montare la connessione iSCSI MPIO (multipath I/O).

1. Fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome LUN.
2. Scorri a **Authorized Hosts**.
3. Sulla destra, fai clic su **Authorize Host**. Seleziona gli host che possono accedere al volume.


## Istantanee e replica

Hai delle istantanee e una replica stabilite per il tuo LUN originale? In caso affermativo, devi configurare la replica e lo spazio di istantanea e creare le pianificazioni delle istantanee per il nuovo LUN con le stesse impostazioni del volume originale.

Se non è ancora stato eseguito l'upgrade del data center della destinazione di replica, non puoi stabilire una replica per il nuovo volume finché non viene eseguito l'upgrade di tale data center.


## Migrazione dei tuoi dati

1. Collegati ai tuoi LUN {{site.data.keyword.blockstorageshort}} originali o nuovi.
   Se hai bisogno di assistenza per la connessione dei due LUN al tuo host, apri un caso di supporto.
   {:tip}

2. Considera quale tipo di dati hai sul LUN {{site.data.keyword.blockstorageshort}} originale e il modo migliore in cui puoi copiarli sul tuo nuovo LUN.
  - Se hai dei backup, contenuto statico ed elementi di cui non è prevista una modifica durante la copia, non ci sono considerazioni di particolare importanza da fare.
  - Se stai eseguendo un database o una macchina virtuale sul tuo {{site.data.keyword.blockstorageshort}}, assicurati che i dati sul LUN originale non vengano modificati durante la copia per evitare un loro danneggiamento. Se ha qualche preoccupazione relativa alla larghezza di banda, esegui la migrazione nei periodi non di punta. Se hai bisogno di assistenza con queste considerazioni, apri un ticket di supporto.

3. Copia i tuoi dati.
   - **Microsoft Windows** - per copiare i dati dal tuo LUN {{site.data.keyword.blockstorageshort}} originale al tuo nuovo LUN, formatta la nuova archiviazione e copia i file utilizzando Esplora risorse di Windows.
   - **Linux** - Puoi utilizzare `rsync` per copiare i dati. Questo è un esempio:
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   È una buona idea usare il precedente comando con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, puoi eliminare l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato nella nuova ubicazione dall'inizio.<br/>
   Quando questo comando viene completato senza l'indicatore `--dry-run`, i tuoi dati vengono copiati sul nuovo LUN {{site.data.keyword.blockstorageshort}}. Esegui nuovamente il comando per assicurarti che non sia sfuggito niente. Puoi anche riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.<br/>
   Una volta completata la tua migrazione, puoi spostare la produzione al nuovo LUN. Puoi quindi scollegare ed eliminare il tuo LUN originale dalla tua configurazione. L'eliminazione rimuove anche le eventuali istantanee o repliche sul sito di destinazione che era associato al LUN originale.
