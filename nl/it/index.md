---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-22"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Informazioni su {{site.data.keyword.blockstorageshort}}
{: #About}

{{site.data.keyword.cloud}} {{site.data.keyword.blockstorageshort}} è un'archiviazione iSCSI persistente e ad elevate prestazioni di cui viene eseguito il provisioning e la gestione indipendentemente dalle istanze di elaborazione. I LUN {{site.data.keyword.blockstorageshort}} basati su iSCSI sono connessi ai dispositivi autorizzati tramite connessioni MPIO (multi-path I/O) ridondanti.

{{site.data.keyword.blockstorageshort}} offre livelli all'avanguardia di durabilità e disponibilità con un insieme di funzioni senza paragoni. È sviluppato utilizzando gli standard del settore e le prassi ottimali. {{site.data.keyword.blockstorageshort}} è progettato per proteggere l'integrità dei dati e mantenere la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati, fornendo al tempo stesso una baseline di prestazioni congruente.

## Funzioni principali
{: #corefeatures}

Avvaliti delle seguenti funzioni di {{site.data.keyword.blockstorageshort}}:

- **Baseline di prestazione congruente**
   - Fornita tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi.
- **Altamente durevole e resiliente**
   - Protegge l'integrità dei dati e mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati senza che occorra creare e gestire un array ridondante a livello di sistema operativo di array di dischi indipendenti (RAID).
- **Crittografia dei dati inattivi** ([Disponibile in data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Crittografia gestita dal provider per i dati inattivi senza costi aggiuntivi.
- **Archiviazione con supporto All-Flash** ([Disponibile in data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Archiviazione All-Flash per i volumi di cui viene eseguito il provisioning con Endurance o Performance a 2 IOPS/GB o a livelli superiori.
- **Istantanee** ([Disponibile in data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Acquisisce istantanee di dati con punto temporale in modo non distruttivo.
- **Replica** ([Disponibile in data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Copia automaticamente le istantanee in un data center {{site.data.keyword.cloud}} partner.
- **Connettività altamente disponibile**
   - Utilizza le connessioni di rete ridondanti per massimizzare la disponibilità
   - {{site.data.keyword.blockstorageshort}} basato su iSCSI utilizza MPIO (Multipath I/O).
- **Accesso simultaneo**
   - Consente a più host di accedere simultaneamente ai volumi di blocco (fino a otto) per le configurazioni in cluster.
- **Database in cluster**
   - Supporta dei casi d'uso avanzati, quali i database in cluster.


## Provisioning
{: #provisioning}

È possibile eseguire il provisioning dei LUN {{site.data.keyword.blockstorageshort}} da 20 GB a 12 TB con due opzioni: <br/> 
- Esegui il provisioning di livelli **Endurance** che offrono livelli di prestazioni predefiniti e altre funzioni quali istantanee e replica.
- Crea un ambiente **Performance** molto potente con IOPS (input/output operations per second) allocato.


### Provisioning con i livelli Endurance
{: #provendurance}

Endurance {{site.data.keyword.blockstorageshort}} è disponibile in quattro livelli di prestazioni IOPS per supportare diverse esigenze applicative. <br />

- **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in qualsiasi momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.

- **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco della macchina virtuale (VM) per un hypervisor.

- **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in qualsiasi momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.

- **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB in [data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

Sono disponibili fino a 48.000 IOPS con un volume Endurance di 12-TB.

La scelta del livello Endurance corretto per il tuo carico di lavoro è fondamentale. È ugualmente importante utilizzare la dimensione blocco corretta, la velocità di connessione Ethernet e il numero di host necessari per ottenere le prestazioni massime. Se qualcuna di queste parti non si allinea con le altre, le ripercussioni sulla velocità effettiva risultante potrebbero essere di notevole entità.


### Provisioning con Performance
{: #provperformance}

Performance è una classe di {{site.data.keyword.blockstorageshort}} progettata per supportare applicazioni a elevato I/O con requisiti di prestazioni chiari che mal si adattano in un livello Endurance. Delle prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi. È possibile eseguire il provisioning di vari tassi di IOPS compresi nell'intervallo 100 - 48.000 con delle dimensioni dell'archiviazione che vanno da 20 GB a 12 TB.

Performance per {{site.data.keyword.blockstorageshort}} è accessibile e montato attraverso una connessione iSCSI (internet Small Computer System Interface) MPIO (Multipath I/O). {{site.data.keyword.blockstorageshort}} viene di norma utilizzato quando si accede al volume da un solo server. Più volumi possono essere montati a un host e messi in stripe insieme per raggiungere volumi più grandi e conteggi IOPS più elevati. I volumi Performance possono essere ordinati in base alle dimensioni e ai tassi dell'IOPS nella Tabella 3 per i sistemi operativi Linux, XEN e Windows.

| Dimensione (GB) | Numero minimo di IOPS | Numero massimo di IOPS
|-----|-----|-----|
| 20 | 100 | 1.000 |
| 40 | 100 | 2.000  |
| 80 | 100 | 4.000 |
| 100 | 100 | 6.000 |
| 250 | 100 | 6.000 |
| 500 | 100  | 6.000 o 10.000 |
| 1.000 | 100 | 6.000 o 20.000 ![Footnote](/images/numberone.png) |
| 2.000 | 200 | 6.000 o 40.000 ![Footnote](/images/numberone.png) |
| 3.000-7.000 | 300 | 6.000 o 48.000 ![Footnote](/images/numberone.png) |
| 8.000-9.000 | 500 | 6.000 o 48.000 ![Footnote](/images/numberone.png) |
| 10.000-12.000 | 1.000 | 6.000 o 48.000 ![Footnote](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabella di comparazione" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![Footnote](/images/numberone.png) * i limiti IOPS al di sopra di 6.000 sono disponibili in data center selezionati.* 

I volumi Performance sono progettati per offrire prestazioni congruentemente prossime al livello IOPS di cui viene eseguito il provisioning. La congruenza semplifica l'impostazione della dimensione e dello scaling degli ambienti applicativi a uno specifico livello di prestazioni. Inoltre, è possibile ottimizzare un ambiente creando un volume con un rapporto ideale prezzo/prestazioni.

## Fatturazione
{: #billing}

Puoi selezionare la fatturazione mensile o oraria per un LUN di blocchi. Il tipo di fatturazione selezionato per un LUN si applica al suo spazio di istantanea e alle sue repliche. Ad esempio, se esegui il provisioning di un LUN con fatturazione oraria, eventuali addebiti di istantanee o replica vengono fatturati in modo orario. Se esegui il provisioning di un LUN con fatturazione mensile, eventuali addebiti di istantanee o replica vengono fatturati in modo mensile.

 * Con la **fatturazione oraria**, il numero di ore per cui il LUN di blocchi è esistito sull'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in [data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

 * Con la **fatturazione mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un LUN viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).

### Endurance
{: #pricing-comparison-endurance}

| Opzioni di prezzi per i livelli IOPS predefiniti | 0,25 IOPS | 2 IOPS/GB | 4 IOPS/GB | 10 IOPS/GB |
|-----|-----|-----|-----|-----|
| Prezzo mensile | $0,06/GB | $0,15/GB | $0,20/GB | $0,58/GB |
| Prezzo orario |$0,0001/GB| $0,0002/GB | $0,0003/GB | $0,0009/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabella di comparazione" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Performance
{: #pricing-comparison-performance}

| Opzioni di prezzi di IOPS personalizzati | Calcolo dei prezzi |
|-----|-----|
| Prezzo mensile | $0,10/GB + $0,07/IOP |
| Prezzo orario | $0,0001/GB + $0,0002/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="Tabella di comparazione" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
