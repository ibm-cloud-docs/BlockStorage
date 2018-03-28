---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Migrazione di {{site.data.keyword.blockstorageshort}} a {{site.data.keyword.blockstorageshort}} crittografato

{{site.data.keyword.blockstoragefull}} crittografato per Endurance o Performance è ora disponibile in data center selezionati. Qui di seguito troverai le informazioni su come migrare il tuo {{site.data.keyword.blockstorageshort}} da non crittografato a crittografato. Per ulteriori informazioni sull'archiviazione crittografata gestita dal provider, leggi l'[articolo sulla crittografia dei dati inattivi di {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html). Per visualizzare un elenco dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili, fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html).

Il percorso di migrazione preferito consiste nello stabilire una connessione a entrambi i LUN simultaneamente e trasferire i file direttamente da un LUN di file all'altro. Le specifiche dipenderanno dal tuo sistema operativo e dalla previsione di possibili modifiche dei dati durante l'operazione di copia.

Per tua comodità, sono stati indicati gli scenari più comuni. Si presuppone che tu già abbia il tuo LUN di file non crittografato collegato al tuo host. In caso contrario, per svolgere tale compito, attieniti alle indicazioni qui di seguito che meglio rispondono al sistema operativo che stai eseguendo.

- [Accesso a {{site.data.keyword.blockstorageshort}} su Linux](accessing_block_storage_linux.html)

- [Accesso a {{site.data.keyword.blockstorageshort}} su Windows](accessing-block-storage-windows.html)

 
## Crea un LUN crittografato

Attieniti alla seguente procedura per creare un LUN, di dimensione pari o superiori, che sia crittografato per facilitare il processo di migrazione.
Ordina un LUN di archiviazione Endurance crittografato

1. Fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** dalla home page di [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OPPURE fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}** nel catalogo {{site.data.keyword.BluSoftlayer_full}}.

2. Fai clic sul link **Order {{site.data.keyword.blockstorageshort}}** nella pagina {{site.data.keyword.blockstorageshort}}.

3. Seleziona **Endurance**.

4. Seleziona il data center dove si trova il tuo LUN originale. <br/> **Nota**: la crittografia è disponibile solo in data center selezionati.

5. Seleziona il livello IOPS desiderato.

6. Seleziona la quantità desiderata di spazio di archiviazione in GB. Per TB, 1 TB equivale a 1.000 GB e 12 TB equivalgono a 12.000 GB.

7. Immetti la quantità desiderata di spazio di archiviazione in GB per le istantanee.

8. Seleziona il sistema operativo VMware dall'elenco a discesa.

9. Invia l'ordine.

## Ordina un LUN di archiviazione Performance crittografato

1. Fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** dalla home page di [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OPPURE fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}** nel catalogo {{site.data.keyword.BluSoftlayer_full}}.

2. Fai clic su **Order {{site.data.keyword.blockstorageshort}}**.

3. Seleziona **Performance**.

4. Seleziona il data center dove si trova il tuo LUN originale. Nota: la crittografia è disponibile solo nei data center con un asterisco (`*`).

5. Seleziona la quantità desiderata di spazio di archiviazione in GB. Per TB, 1 TB equivale a 1.000 GB e 12 TB equivalgono a 12.000 GB.

6. Immetti la quantità desiderata di IOPS in intervalli di 100.

7. Seleziona il sistema operativo VMware dall'elenco a discesa.

8. Invia l'ordine.

Il provisioning dello storage verrà eseguito in meno di un minuto e sarà visibile sulla pagina {{site.data.keyword.blockstorageshort}} del {{site.data.keyword.slportal}}.

 
## Connetti un nuovo volume all'host

Gli host "autorizzati" sono host a cui sono stati concessi i diritti di accesso a un volume. Senza l'autorizzazione host, non potrai accedere all'archiviazione dal tuo sistema né utilizzarla. Autorizzare un host ad accedere al tuo volume genera il nome utente, la password e l'IQN (iSCSI qualified name) necessari per montare la connessione iSCSI MPIO (multipath I/O).

1. Fai clic su **Storage**  > **{{site.data.keyword.blockstorageshort}}** e fai clic sul tuo nome LUN.

2. Scorri alla sezione **Authorized Hosts** della pagina

3. Fai clic sul link **Authorize Host** sul lato destro della pagina. Seleziona gli host che possono accedere al volume.

 
## Istantanee e replica

Hai delle istantanee e una replica stabilite per il tuo LUN originale? In caso affermativo, dovrai configurare la replica e lo spazio di istantanea e creare le pianificazioni delle istantanee per il nuovo LUN crittografato con le stesse impostazioni del volume originale. 

Nota: se è stato eseguito l'upgrade del data center della destinazione di replica per la crittografia, non sarai in grado di stabilire una replica per il volume crittografato finché non verrà eseguito l'upgrade di tale data center.

 
## Migra i tuoi dati

Devi essere connesso sia al tuo LUN {{site.data.keyword.blockstorageshort}} originale che a quello crittografato. 
- In caso contrario, assicurati di esserti attenuto correttamente alla procedura sia indicata sopra che richiamata in altri post. Apri un ticket di supporto per assistenza con la connessione dei due LUN.

### Considerazioni sui dati

A questo punto, dovresti valutare quale tipo di dati hai sul LUN {{site.data.keyword.blockstorageshort}} originale e il modo migliore in cui puoi copiarli sul tuo LUN crittografato. Se hai dei backup, contenuto statico ed elementi di cui non è prevista una modifica durante la copia, non ci sono considerazioni di particolare importanza da fare.

Se stai eseguendo un database o una macchina virtuale sul tuo {{site.data.keyword.blockstorageshort}}, assicurati che i dati sul LUN originale non siano modificati durante la copia in modo che non si verifichi alcun danneggiamento. Se ha qualche preoccupazione relativa alla larghezza di banda, devi eseguire la migrazione nei periodi non di punta. Se hai bisogno di assistenza con queste considerazioni, non esitare ad aprire un ticket di supporto.
 
### Microsoft Windows

Per copiare i dati dal tuo LUN {{site.data.keyword.blockstorageshort}} originale al tuo LUN crittografato, formatta la nuova archiviazione e copia i file utilizzando Esplora risorse di Windows.

 
### Linux

Puoi prendere in considerazione l'utilizzo di rsync per copiare i dati. Di seguito è riportato un comando di esempio:

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

Ti consigliamo di usare il comando sopra indicato con l'indicatore --dry-run una volta per assicurarti che l'allineamento dei percorsi sia corretto. Se il processo viene interrotto, sarebbe opportuno che eliminassi l'ultimo file di destinazione di cui era in corso la copia per assicurarti che venga copiato dall'inizio alla nuova ubicazione.

Dopo che questo comando è stato completato senza l'indicatore --dry-run, i tuoi dati dovrebbero essere stati copiati sul LUN {{site.data.keyword.blockstorageshort}} crittografato. Devi scorrere verso l'alto ed eseguire nuovamente il comando per assicurarti che non sia sfuggito niente. Puoi anche riesaminare manualmente entrambe le ubicazioni per cercare eventuali elementi che potrebbero essere sfuggiti.

Una volta completata la migrazione, sarai in grado di trasferire la produzione al LUN crittografato e scollegare ed eliminare il tuo LUN originale dalla tua configurazione. Nota: l'eliminazione rimuoverà anche le eventuali istantanee o repliche sul sito di destinazione che era associato al LUN originale.
