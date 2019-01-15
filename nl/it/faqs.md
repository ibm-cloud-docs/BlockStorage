---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# Domande frequenti

## Quante istanze possono condividere l'uso di un volume {{site.data.keyword.blockstorageshort}}?
{: faq}

Il limite predefinito per il numero di autorizzazioni per volume di blocchi è otto. Questo significa che è possibile autorizzare l'accesso alla LUN Block Storage per un massimo di 8 host. Per richiedere un aumento del limite, contatta il tuo rappresentante di vendita.

## Quanti volumi possono essere ordinati?
{: faq}

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il tuo limite di volumi, contatta il rappresentante di vendita. Per ulteriori informazioni, vedi [Gestione dei limiti di archiviazione](managing-storage-limits.html).

## Quanti volumi {{site.data.keyword.blockstorageshort}} possono essere montati su un host?
{: faq}

Dipende da quello che il sistema operativo dell'host è in grado di gestire; non è qualcosa che viene limitato da {{site.data.keyword.BluSoftlayer_full}}. Fai riferimento alla documentazione del tuo sistema operativo per i limiti relativi al numero di volumi che è possibile montare.

## Quale versione di Windows devo scegliere per il mio LUN Block Storage?
{: faq}

Quando si crea un LUN, devi specificare il tipo di SO. Il tipo di SO deve essere basato sul sistema operativo utilizzato dagli host che accedono al LUN. Il tipo di SO non può essere modificato dopo la creazione del LUN. La dimensione effettiva del LUN potrebbe variare leggermente in base al tipo di SO del LUN.

**Windows 2008+**
- Il LUN archivia i dati di Windows per Windows 2008 e versioni successive. Utilizza questa opzione del SO se il tuo sistema operativo host è Windows Server 2008, Windows Server 2012, Windows Server 2016. Sono supportati i metodi di partizionamento MBR e GPT.

**Windows 2003**
- Il LUN archivia un tipo di disco non elaborato in un disco Windows a partizione singola che utilizza lo stile di partizionamento MBR (Master Boot Record). Utilizza questa opzione solo se il tuo sistema operativo host è Windows 2000 Server, Windows XP o Windows Server 2003 che utilizza il metodo di partizionamento MBR.

**Windows GPT**
-  Il LUN archivia i dati Windows utilizzando lo stile di partizionamento GPT (GUID Partition Type). Utilizza questa opzione se vuoi utilizzare il metodo di partizionamento GPT e il tuo host può utilizzarlo. Windows Server 2003, Service Pack 1 e successivi possono utilizzare il metodo di partizionamento GPT e tutte le versioni a 64-bit di Windows lo supportano.

## Il limite dell'IOPS allocato viene implementato in base all'istanza o in base al volume?
{: faq}

L'IOPS viene implementato a livello dei volumi. In altre parole, due host connessi a un volume con 6000 IOPS condividono questi 6000 IOPS.

## Misurazione dell'IOPS
{: faq}

L'IOPS viene misurato in base a un profilo di caricamento di blocchi da 16-KB con 50 percento di letture e 50 percento di scritture casuali. I carichi di lavoro che differiscono da questo profilo potrebbero riscontrare delle prestazioni inferiori.

## Cosa succede quando una dimensione di blocco più piccola viene utilizzata per misurare le prestazioni?
{: faq}

Quando utilizzi delle dimensioni blocco più piccole, è comunque possibile ottenere l'IOPS massimo. Tuttavia, la velocità effettiva sarà inferiore. Ad esempio, un volume con 6000 IOPS avrà la seguente velocità effettiva alle varie dimensioni blocco:

- 16 KB * 6000 IOPS == ~93,75 MB/sec
- 8 KB * 6000 IOPS == ~46,88 MB/sec
- 4 KB * 6000 IOPS == ~23,44 MB/sec

## Occorre preriscaldare il volume per ottenere la velocità effettiva prevista?
{: faq}

Non è necessario eseguire un preriscaldamento. Puoi osservare la velocità effettiva specificata non appena viene eseguito il provisioning del volume.

## È possibile ottenere più velocità effettiva utilizzando una connessione Ethernet più veloce?
{: faq}

I limiti di velocità effettiva sono impostati su un livello per LUN; pertanto, l'utilizzo di una connessione Ethernet più veloce non aumenta tale limite impostato. Tuttavia, con una connessione Ethernet più lenta, la tua larghezza di banda può essere un potenziale collo di bottiglia.

## I firewall e i gruppi di sicurezza hanno ripercussioni sulle prestazioni?
{: faq}

Consigliamo di eseguire il traffico di archiviazione su una VLAN che ignora il firewall. L'esecuzione del traffico di archiviazione tramite i firewall software aumenta la latenza e ha un impatto negativo sulle prestazioni dell'archiviazione.

## Quale latenza è prevista per {{site.data.keyword.blockstorageshort}}?   
{: faq}

La latenza di destinazione nell'archiviazione è di <1 ms. L'archiviazione è connessa alle istanze di elaborazione su una rete condivisa e, pertanto, la latenza delle prestazioni esatta dipende dal traffico di rete durante l'operazione.

## Perché posso ordinare {{site.data.keyword.blockstorageshort}} con un livello Endurance 10 IOPS/GB in alcuni data center e non in altri?
{: faq}

Il livello 10 IOPS/GB di {{site.data.keyword.blockstorageshort}} di tipo Endurance è disponibile solo in data center selezionati; a tale selezione verranno gradualmente aggiunti dei nuovi data center. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](new-ibm-block-and-file-storage-location-and-features.html).

## Come faccio a capire quali volumi {{site.data.keyword.blockstorageshort}} sono crittografati?
{: faq}

Quando visualizzi il tuo elenco di {{site.data.keyword.blockstorageshort}} nel [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}, puoi vedere un'icona di lucchetto accanto al nome del volume per i LUN che sono codificati.

## Come facciamo a sapere se stiamo eseguendo il provisioning di {{site.data.keyword.blockstorageshort}} in un data center di cui è stato eseguito l'upgrade?
{: faq}

Quando ordini {{site.data.keyword.blockstorageshort}}, tutti i data center di cui è stato eseguito l'upgrade sono segnalati da un asterisco (`*`) nel modulo dell'ordine e da un'indicazione che ti avvisa che stai eseguendo il provisioning dell'archiviazione con la crittografia. Una volta eseguito il provisioning dell'archiviazione, puoi vedere un'icona nell'elenco archiviazioni che mostra tale archiviazione come crittografata. Il provisioning di tutti i volumi crittografati e di tutti i LUN viene eseguito solo nei data center di cui è stato eseguito l'upgrade. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](new-ibm-block-and-file-storage-location-and-features.html).

## Se siamo i proprietari di {{site.data.keyword.blockstorageshort}} non crittografato in un data center di cui è stato appena eseguito l'upgrade, possiamo crittografare tale {{site.data.keyword.blockstorageshort}}?
{: faq}

Non è possibile crittografare il {{site.data.keyword.blockstorageshort}} di cui viene eseguito il provisioning prima dell'upgrade del data center.
Il nuovo {{site.data.keyword.blockstorageshort}} di cui è stato eseguito il provisioning in data center di cui è stato eseguito l'upgrade, viene crittografato automaticamente. Non c'è alcuna impostazione di crittografia da cui scegliere, l'operazione è automatica.
I dati su un'archiviazione non crittografata in un data center di cui è stato eseguito l'upgrade possono essere crittografati creando un nuovo LUN di blocchi e copiando quindi i dati nel nuovo LUN crittografato con una migrazione basata sull'host. Fai clic [qui](migrate-block-storage-encrypted-block-storage.html) per le istruzioni.

## {{site.data.keyword.blockstorageshort}} supporta la prenotazione permanente SCSI-3 per implementare il fencing I/O per Db2 pureScale?
{: faq}

Sì, {{site.data.keyword.blockstorageshort}} supporta entrambe le prenotazioni permanenti SCSI-2 e SCSI-3.

## Che succede ai dati quando i LUN {{site.data.keyword.blockstorageshort}} vengono eliminati?
{: faq}

{{site.data.keyword.blockstoragefull}} presenta i volumi a blocchi ai clienti sull'archiviazione fisica da cui vengono cancellati tutti i dati prima di qualsiasi riutilizzo. I clienti con requisiti speciali per la conformità quali le Guidelines for Media Sanitization (direttive sulla pulizia dei supporti) NIST 800-88 devono eseguire la procedura di ripulitura dei dati prima di eliminare la loro archiviazione.

## Cosa succede ai driver che vengono disattivati dal data center cloud?
{: faq}

Quando i driver vengono disattivati, IBM li distrugge prima di eliminarli. I driver diventano inutilizzabili. Tutti i dati scritti in questi driver diventano inaccessibili.
