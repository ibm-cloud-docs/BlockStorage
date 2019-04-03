---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-28"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# Esercitazione introduttiva
{: #getting-started}

{{site.data.keyword.blockstoragefull}} è un'archiviazione iSCSI persistente e ad elevate prestazioni di cui viene eseguito il provisioning e la gestione indipendentemente dalle istanze di elaborazione. I LUN {{site.data.keyword.blockstorageshort}} basati su iSCSI sono connessi ai dispositivi autorizzati tramite connessioni MPIO (multi-path I/O) ridondanti.

{{site.data.keyword.blockstorageshort}} offre livelli all'avanguardia di durabilità e disponibilità con un insieme di funzioni senza paragoni. È sviluppato utilizzando gli standard del settore e le prassi ottimali. {{site.data.keyword.blockstorageshort}} è progettato per proteggere l'integrità dei dati e mantenere la disponibilità durante gli eventi di manutenzione e i malfunzionamenti non pianificati, fornendo al tempo stesso una baseline di prestazioni congruente.
{:shortdesc}

## Prima di iniziare
{: #prereqs}

È possibile eseguire il provisioning dei LUN {{site.data.keyword.blockstorageshort}} da 20 GB a 12 TB con due opzioni: <br/>
- Esegui il provisioning di livelli **Endurance** che offrono livelli di prestazioni predefiniti e altre funzioni quali istantanee e replica.
- Crea un ambiente **Performance** molto potente con IOPS (input/output operations per second) allocato.

Per ulteriori informazioni sull'offerta {{site.data.keyword.blockstorageshort}}, consulta [Informazioni su {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

### Considerazioni sul provisioning

**Dimensione blocco**

IOPS sia per Endurance che per Performance è basato su una dimensione di blocco di 16-KB con un carico di lavoro casuale al 50 percento e con lettura/scrittura al 50/50. Un blocco di 16-KB è l'equivalente di una scrittura sul volume.
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

La velocità della tua connessione Ethernet deve essere più veloce della velocità effettiva massima prevista dal tuo volume. In generale, non prevedi di saturare la connessione Ethernet oltre il 70% della larghezza di banda disponibile. Ad esempio, se hai 6.000 IOPS e stai utilizzando una dimensione del blocco di 16-KB, il volume può gestire una velocità effettiva di circa 94-MBps. Se hai una connessione Ethernet da 1-Gbps al tuo LUN, diventa un collo di bottiglia quando i tuoi server proveranno a utilizzare la velocità effettiva massima disponibile. Ciò è dovuto al fatto che il 70 percento del limite teorico di una connessione Ethernet da 1-Gbps (125 MB al secondo) consentirebbe solo 88 MB al secondo.

Per raggiungere l'IOPS massimo, è necessario che siano implementate delle risorse di rete adeguate. Altre considerazioni includono l'utilizzo della rete privata esternamente al lato archiviazione e host e le regolazioni specifiche per le applicazioni (stack di IP o [profondità di coda](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings) e altre impostazioni).

Il traffico di archiviazione è incluso nell'utilizzo della rete totale dei server virtuali pubblici. Per ulteriori informazioni sui limiti che possono essere imposti dal servizio, consulta la [documentazione di Virtual Server](/docs/vsi?topic=virtual-servers-public-virtual-servers).
{:tip}

## Invio del tuo ordine
{: #submitorder}

Quando sei pronto a inviare il tuo ordine, puoi effettuarlo tramite la [Console](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) o la [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI).

## Connessione alla tua nuova archiviazione
{: #mountingstorage}

Quando la tua richiesta di provisioning è completa, autorizza i tuoi host ad accedere alla nuova archiviazione e configura la tua connessione. A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connessione ai LUN su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connessione ai LUN su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurazione di Block Storage per il backup con cPanell](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurazione di Block Storage per il backup con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Gestione della tua nuova archiviazione

Tramite il portale o la SLCLI puoi gestire molti aspetti del tuo File Storage come ad esempio gli annullamenti e le autorizzazioni host. Per ulteriori informazioni, consulta [Gestione di {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
