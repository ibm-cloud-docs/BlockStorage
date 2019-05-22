---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Upgrade di un {{site.data.keyword.blockstorageshort}} esistente a un {{site.data.keyword.blockstorageshort}} migliorato
{: #migratestorage}

Il {{site.data.keyword.blockstoragefull}} migliorato è ora disponibile in data center selezionati. per visualizzare l'elenco dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili quali i tassi di IOPS regolabili e i volumi espandibili, fai clic [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-news). Per ulteriori informazioni sull'archiviazione crittografata gestita dal provider, consulta [Crittografia dei dati inattivi {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i LUN simultaneamente e trasferire i file direttamente da un LUN all'altro. Le specifiche dipendono dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Si presuppone che tu già abbia il tuo LUN non crittografato collegato al tuo host. In caso contrario, per svolgere tale attività, attieniti alle indicazioni adatte al tuo sistema operativo:

- [Connessione ai LUN su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connessione ai LUN su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connessione ai LUN su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Tutti i volumi {{site.data.keyword.blockstorageshort}} migliorati di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai utilizzando il punto di montaggio corretto per entrambi i volumi di archiviazione, puoi visualizzare le informazioni sui punti di montaggio nella pagina **Volume Details** nella console. Puoi anche accedere al punto di montaggio corretto tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

## Creazione di un {{site.data.keyword.blockstorageshort}}

Quando effettui un ordine con la API, specifica il pacchetto "Storage as a Service" per assicurarti che stai ottenendo le funzioni aggiornate con la tua nuova archiviazione.
{:important}

Puoi ordinare un LUN migliorato tramite la console IBM Cloud e il {{site.data.keyword.slportal}}. Il tuo nuovo LUN deve essere di dimensione pari o superiore a quella del volume originale per facilitare la migrazione.

- [Ordinazione di {{site.data.keyword.blockstorageshort}} con livelli IOPS predefiniti (Endurance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [Ordinazione di {{site.data.keyword.blockstorageshort}} con IOPS personalizzato (Performance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

La tua nuova archiviazione è pronta per essere montata in pochi minuti. Puoi visualizzarla nell'elenco risorse e nell'elenco {{site.data.keyword.blockstorageshort}}.

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
  - Se hai dei backup, contenuto statico ed elementi di cui non è prevista una modifica durante la copia, non ti devi preoccupare troppo.
  - Se stai eseguendo un database o una macchina virtuale sul tuo {{site.data.keyword.blockstorageshort}}, assicurati che i dati sul LUN originale non vengano modificati durante la copia per evitare un loro danneggiamento.
  - Se ha qualche preoccupazione relativa alla larghezza di banda, esegui la migrazione nei periodi non di punta.
  - Se hai bisogno di assistenza con queste considerazioni, apri un caso di supporto.

3. Copia i tuoi dati.
   - Per **Microsoft Windows**, formatta la nuova archiviazione e copia i dati dal tuo LUN {{site.data.keyword.blockstorageshort}} originale al tuo nuovo LUN utilizzando Windows Explorer.
   - Per **Linux**, puoi utilizzare `rsync` per copiare i dati.
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   È una buona idea usare il precedente comando con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, puoi eliminare l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato nella nuova ubicazione dall'inizio.<br/>
   Quando questo comando viene completato senza l'indicatore `--dry-run`, i tuoi dati vengono copiati sul nuovo LUN {{site.data.keyword.blockstorageshort}}. Esegui nuovamente il comando per assicurarti che non sia sfuggito niente. Puoi anche riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.<br/>
   Una volta completata la tua migrazione, puoi spostare la produzione al nuovo LUN. Puoi quindi scollegare ed eliminare il tuo LUN originale dalla tua configurazione. L'eliminazione rimuove anche le eventuali istantanee o repliche sul sito di destinazione che era associato al LUN originale.
