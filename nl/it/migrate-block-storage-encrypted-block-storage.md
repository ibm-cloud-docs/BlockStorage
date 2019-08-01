---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Upgrade di un {{site.data.keyword.blockstorageshort}} esistente a un {{site.data.keyword.blockstorageshort}} migliorato
{: #migratestorage}

Il {{site.data.keyword.blockstoragefull}} migliorato è ora disponibile nella maggior parte dei [data center](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i LUN simultaneamente e trasferire i file direttamente da un LUN all'altro. Le specifiche dipendono dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Si presuppone che tu già abbia il tuo LUN non crittografato collegato al tuo host. In caso contrario, per svolgere tale attività, attieniti alle indicazioni adatte al tuo sistema operativo:

- [Connessione dei volumi di archiviazione su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connessione dei volumi di archiviazione su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connessione dei volumi di archiviazione su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

## Creazione di un {{site.data.keyword.blockstorageshort}}

Quando effettui un ordine con la API, specifica il pacchetto "Storage as a Service" per assicurarti che stai ottenendo le funzioni aggiornate con la tua nuova archiviazione.
{:important}

Puoi ordinare un LUN migliorato tramite la console IBM Cloud. Il tuo nuovo LUN deve essere di dimensione pari o superiore a quella del volume originale per facilitare la migrazione.

- [Ordinazione di {{site.data.keyword.blockstorageshort}} con livelli IOPS predefiniti (Endurance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsoleEndurance)
- [Ordinazione di {{site.data.keyword.blockstorageshort}} con IOPS personalizzato (Performance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsolePerformance)

La tua nuova archiviazione è pronta per essere montata in pochi minuti. Puoi visualizzarla nell'elenco risorse e nell'elenco {{site.data.keyword.blockstorageshort}}.

## Connessione del nuovo {{site.data.keyword.blockstorageshort}} all'host

Gli host "autorizzati" sono host a cui è stato concesso l'accesso a un volume. Senza l'autorizzazione host, non puoi accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo volume genera il nome utente, la password e l'IQN (iSCSI qualified name) necessari per montare la connessione iSCSI MPIO (multipath I/O).

1. Accedi alla [console {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}. Dal **menu**, seleziona **Classic Infrastructure**.
2. Fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}**.
3. Individua il nuovo volume e fai clic su **...**.
4. Seleziona **Authorize Host**.
5. Per visualizzare l'elenco di dispositivi o indirizzi IP disponibili, devi prima selezionare se vuoi autorizzare l'accesso basato sui tipi di dispositivo o sulle sottoreti.
   - se scegli Devices, puoi scegliere tra i server bare metal o virtuali.
   - se scegli IP Address, devi prima selezionare la sottorete in cui risiede il tuo host.
6. Dall'elenco filtrato, seleziona uno o più host che possono accedere al volume e fai clic su **Save**.


## Istantanee e replica

Hai delle istantanee e una replica stabilite per il tuo LUN originale? In caso affermativo, devi configurare la replica e lo spazio di istantanea e creare le pianificazioni delle istantanee per il nuovo LUN con le stesse impostazioni del volume originale.

Se non è ancora stato eseguito l'upgrade del data center della destinazione di replica, non puoi stabilire una replica per il nuovo volume finché non viene eseguito l'upgrade di tale data center.
{:note}


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

   Ti consigliamo di usare il precedente comando con l'indicatore `--dry-run` una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se questo processo viene interrotto, puoi eliminare l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato nella nuova ubicazione dall'inizio.<br/>
   Quando questo comando viene completato senza l'indicatore `--dry-run`, i tuoi dati vengono copiati sul nuovo LUN {{site.data.keyword.blockstorageshort}}. Esegui nuovamente il comando per assicurarti che non sia sfuggito niente. Puoi anche riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.<br/>
   Una volta completata la tua migrazione, puoi spostare la produzione al nuovo LUN. Puoi quindi scollegare ed eliminare il tuo LUN originale dalla tua configurazione. L'eliminazione rimuove anche le eventuali istantanee o repliche sul sito di destinazione che era associato al LUN originale.
