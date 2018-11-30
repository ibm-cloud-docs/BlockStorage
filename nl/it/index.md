---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Introduzione a {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} è un'archiviazione iSCSI persistente e ad elevate prestazioni di cui viene eseguito il provisioning e la gestione indipendentemente dalle istanze di elaborazione. I LUN {{site.data.keyword.blockstorageshort}} basati su iSCSI sono connessi ai dispositivi autorizzati tramite connessioni MPIO (multi-path I/O) ridondanti.

{{site.data.keyword.blockstorageshort}} offre livelli all'avanguardia di durabilità e disponibilità con un insieme di funzioni senza paragoni. È sviluppato utilizzando gli standard del settore e le prassi ottimali. {{site.data.keyword.blockstorageshort}} è progettato per proteggere l'integrità dei dati e mantenere la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati, fornendo al tempo stesso una baseline di prestazioni congruente.

## Funzioni principali

Avvaliti delle seguenti funzioni di {{site.data.keyword.blockstorageshort}}:

- **Baseline di prestazione congruente**
   - Fornita tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi.
- **Altamente durevole e resiliente**
   - Protegge l'integrità dei dati e mantiene la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati senza che occorra creare e gestire un array ridondante a livello di sistema operativo di array di dischi indipendenti (RAID).
- **Crittografia dei dati inattivi** ([Disponibile in data center selezionati](new-ibm-block-and-file-storage-location-and-features.html))
   - Crittografia gestita dal provider per i dati inattivi senza costi aggiuntivi.
- **Archiviazione con supporto All-Flash** ([Disponibile in data center selezionati](new-ibm-block-and-file-storage-location-and-features.html))
   - Archiviazione All-Flash per i volumi di cui viene eseguito il provisioning con Endurance o Performance a 2 IOPS/GB o a livelli superiori.
- **Istantanee** ([Disponibile in data center selezionati](new-ibm-block-and-file-storage-location-and-features.html))
   - Acquisisce istantanee di dati con punto temporale in modo non distruttivo.
- **Replica** ([Disponibile in data center selezionati](new-ibm-block-and-file-storage-location-and-features.html))
   - Copia automaticamente le istantanee in un data center {{site.data.keyword.BluSoftlayer_full}} partner.
- **Connettività altamente disponibile**
   - Utilizza le connessioni di rete ridondanti per massimizzare la disponibilità
   - {{site.data.keyword.blockstorageshort}} basato su iSCSI utilizza MPIO (Multipath I/O).
- **Accesso simultaneo**
   - Consente a più host di accedere simultaneamente ai volumi di blocco (fino a otto) per le configurazioni in cluster.
- **Database in cluster**
   - Supporta dei casi d'uso avanzati, quali i database in cluster.

## Fatturazione

Puoi selezionare la fatturazione mensile o oraria per un LUN di blocchi. Il tipo di fatturazione selezionato per un LUN si applica al suo spazio di istantanea e alle sue repliche. Ad esempio, se esegui il provisioning di un LUN con fatturazione oraria, eventuali addebiti di istantanee o replica vengono fatturati in modo orario. Se esegui il provisioning di un LUN con fatturazione mensile, eventuali addebiti di istantanee o replica vengono fatturati in modo mensile.

Con la **fatturazione oraria**, il numero di ore per cui il LUN di blocchi è esistito sull'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione, a seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).

Con la **fatturazione mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un LUN viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).

**Performance**
<table>
  <caption>La tabella 1 che mostra i prezzi per l'archiviazione Performance con fatturazione oraria e mensile.</caption>
  <tr>
   <th>Prezzo mensile</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Prezzo orario</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
</table>

**Endurance**
<table>
  <caption>La tabella 2 che mostra i prezzi per l'archiviazione Endurance per ogni livello con le opzioni fatturazione oraria e mensile.</caption>
  <tr>
   <th>Livello IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Prezzo mensile</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0,20/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Prezzo orario</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0009/GB</td>
  </tr>
</table>



## Provisioning

È possibile eseguire il provisioning dei LUN {{site.data.keyword.blockstorageshort}} da 20 GB a 12 TB con due opzioni: <br/>
- Esegui il provisioning di livelli **Endurance** che offrono livelli di prestazioni predefiniti e altre funzioni quali istantanee e replica.
- Crea un ambiente **Performance** molto potente con IOPS (input/output operations per second) allocato.

### Provisioning con i livelli Endurance

Endurance {{site.data.keyword.blockstorageshort}} è disponibile in quattro livelli di prestazioni IOPS per supportare diverse esigenze applicative. <br />

- **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi in qualsiasi momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.

- **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco della macchina virtuale (VM) per un hypervisor.

- **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi in qualsiasi momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.

- **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html).

Sono disponibili fino a 48.000 IOPS con un volume Endurance di 12 TB.

La scelta del livello Endurance corretto per il tuo carico di lavoro è fondamentale. È ugualmente importante utilizzare la dimensione blocco corretta, la velocità di connessione Ethernet e il numero di host necessari per ottenere le prestazioni massime. Se qualcuna di queste parti non si allinea con le altre, le ripercussioni sulla velocità effettiva risultante potrebbero essere di notevole entità.


### Provisioning con Performance

Performance è una classe di {{site.data.keyword.blockstorageshort}} progettata per supportare applicazioni a elevato I/O con requisiti di prestazioni chiari che mal si adattano in un livello Endurance. Delle prestazioni prevedibili si raggiungono tramite l'allocazione di IOPS a livello di protocollo ai singoli volumi. È possibile eseguire il provisioning di vari tassi di IOPS compresi nell'intervallo 100 - 48.000 con delle dimensioni dell'archiviazione che vanno da 20 GB a 12 TB.

Performance per {{site.data.keyword.blockstorageshort}} è accessibile e montato attraverso una connessione iSCSI (internet Small Computer System Interface) MPIO (Multipath I/O). {{site.data.keyword.blockstorageshort}} viene di norma utilizzato quando si accede al volume da un solo server. Più volumi possono essere montati a un host e messi in stripe insieme per raggiungere volumi più grandi e conteggi IOPS più elevati. I volumi Performance possono essere ordinati in base alle dimensioni e ai tassi dell'IOPS nella Tabella 3 per i sistemi operativi Linux, XEN e Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>La tabella 3 che mostra le combinazioni di dimensione e IOPS per l'archiviazione Performance.<br/><sup><img src="/images/numberone.png" alt="Footnote" /></sup> il limite IOPS al di sopra di 6.000 è disponibile in data center selezionati.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Dimensione (GB)</th>
            <th>Numero minimo di IOPS</th>
            <th>Numero massimo di IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 o 10.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 o 20.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 o 40.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Footnote" /></sup></td>
          </tr>
</table>


I volumi Performance sono progettati per offrire prestazioni congruentemente prossime al livello IOPS di cui viene eseguito il provisioning. La congruenza semplifica l'impostazione della dimensione e dello scaling degli ambienti applicativi a uno specifico livello di prestazioni. Inoltre, è possibile ottimizzare un ambiente creando un volume con un rapporto ideale prezzo/prestazioni.

### Considerazioni sul provisioning

**Dimensione blocco**

IOPS sia per Endurance che per Performance è basato su una dimensione di blocco di 16 KB con un carico di lavoro casuale al 50 percento e con lettura/scrittura al 50/50. Un blocco di 16 KB è l'equivalente di una scrittura sul volume.
{:important}

La dimensione del blocco utilizzata dalla tua applicazione influisce direttamente sulle prestazioni di archiviazione. Se la dimensione del blocco utilizzata dalla tua applicazione è inferiore a 16 KB, il limite IOPS viene realizzato prima del limite di velocità effettiva. Viceversa, se la dimensione del blocco utilizzata dalla tua applicazione è superiore a 16 KB, il limite di velocità effettiva viene realizzato prima del limite IOPS.

<table>
  <caption>La tabella 4 mostra degli esempi su come la dimensione blocco e IOPS influisce sulla velocità effettiva.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Dimensione blocco (KB)</th>
            <th>IOPS</th>
            <th>Velocità effettiva (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (tipica per Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (tipica per Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (tipica per SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**Host autorizzati**

Un altro fattore da considerare è il numero di host che sta utilizzando il tuo volume. Se c'è un singolo host che sta accedendo al volume, può essere difficile realizzare l'IOPS massimo disponibile, soprattutto a conteggi IOPS estremi (nell'ordine delle decine di migliaia). Se il tuo carico di lavoro richiede una velocità effettiva elevata, sarebbe meglio configurare almeno un paio server che accedono al tuo volume per evitare un collo di bottiglia di un singolo server.

**Connessione di rete**

La velocità della tua connessione Ethernet deve essere più veloce della velocità effettiva massima prevista dal tuo volume. In generale, non prevedi di saturare la connessione Ethernet oltre il 70% della larghezza di banda disponibile. Ad esempio, se hai 6.000 IOPS e stai utilizzando una dimensione del blocco di 16KB, il volume può gestire una velocità effettiva di circa 94 MBps. Se hai una connessione Ethernet da 1 Gbps al tuo LUN, diventa un collo di bottiglia quando i tuoi server proveranno a utilizzare la velocità effettiva massima disponibile. Ciò è dovuto al fatto che il 70 percento del limite teorico di una connessione Ethernet da 1 Gbps (125 MB al secondo) consentirebbe solo 88 MB al secondo.

Per raggiungere l'IOPS massimo, è necessario che siano implementate delle risorse di rete adeguate. Altre considerazioni includono l'utilizzo della rete privata esternamente al lato archiviazione e host e le regolazioni specifiche per le applicazioni (stack di IP o [profondità di coda](set-host-queue-depth-settings-performance-and-endurance-storage.html) e altre impostazioni).

Il traffico di archiviazione è incluso nell'utilizzo della rete totale dei server virtuali pubblici. Consulta la [documentazione dei server virtuali](https://{DomainName}/docs/vsi/vsi_public.html#public-virtual-servers) per comprendere i limiti che potrebbero essere imposti dal servizio.
{:tip}

## Invio del tuo ordine

Quando sei pronto ad inviare il tuo ordine, segui le istruzioni [qui](provisioning-block_storage.html).

## Connessione alla tua nuova archiviazione

Quando la tua richiesta di provisioning è completa, autorizza i tuoi host ad accedere alla nuova archiviazione e configura la tua connessione. A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html)
- [Connessione ai LUN iSCSI MPIO su CloudLinux](configure-iscsi-cloudlinux.html)
- [Connessione ai LUN iSCSI MPIO su Microsoft Windows](accessing-block-storage-windows.html)
- [Configurazione di Block Storage per il backup con cPanell](configure-backup-cpanel.html)
- [Configurazione di Block Storage per il backup con Plesk](configure-backup-plesk.html)
