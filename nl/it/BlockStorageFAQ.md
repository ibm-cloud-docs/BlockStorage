---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Domande frequenti di {{site.data.keyword.blockstorageshort}}

## Quante istanze possono condividere l'uso di un volume {{site.data.keyword.blockstorageshort}}?
Il limite predefinito per il numero di autorizzazioni per volume di blocchi è 8. Per aumentare il limite, contatta il tuo rappresentante di vendita.

## L'IOPS allocato viene implementato in base all'istanza o in base al volume?
L'IOPS viene implementato a livello dei volumi. In altre parole, due host connessi a un volume con 6000 IOPS condividono questi 6000 IOPS.

## Come viene misurato l'IOPS?
L'IOPS viene misurato in base a un profilo di caricamento di blocchi da 16 KB con 50 percento di letture e 50 percento di scritture casuali. I carichi di lavoro che differiscono da questo profilo potrebbero riscontrare delle prestazioni inferiori.

## Cosa succede se uso una dimensione blocco più piccola quando misuro le prestazioni?
Quando si utilizzano delle dimensioni blocco più piccole, è comunque possibile ottenere l'IOPS massimo; tuttavia, la velocità effettiva sarà inferiore. Ad esempio, un volume con 6000 IOPS avrà la seguente velocità effettiva alle varie dimensioni blocco:

- 16 KB * 6000 IOPS == ~93,75 MB/sec 
- 8 KB * 6000 IOPS == ~46,88 MB/sec
- 4 KB * 6000 IOPS == ~23,44 MB/sec

## Occorre preriscaldare il volume per ottenere la velocità effettiva prevista?
Non è necessario eseguire un preriscaldamento. Osserverai la velocità effettiva specificata non appena verrà eseguito il provisioning del volume.

## Perché posso eseguire il provisioning di {{site.data.keyword.blockstorageshort}} con un livello Endurance 10 IOPS/GB in alcuni data center e non in altri?
Il livello 10 IOPS/GB di {{site.data.keyword.blockstorageshort}} di tipo Endurance è disponibile solo in data center selezionati; a tale selezione verranno gradualmente aggiunti dei nuovi data center. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](new-ibm-block-and-file-storage-location-and-features.html).

## Come faccio a capire quali dei miei LUN/volumi {{site.data.keyword.blockstorageshort}} sono crittografati?
Quando visualizzi il tuo elenco di {{site.data.keyword.blockstorageshort}} nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, vedrai un icona di lucchetto a destra dei nomi di LUN/volume, nel caso in cui siano crittografati.

## Come faccio a sapere se sto eseguendo il provisioning di {{site.data.keyword.blockstorageshort}} in un data center di cui è stato eseguito l'upgrade?
Quando esegui il provisioning di {{site.data.keyword.blockstorageshort}}, tutti i data center di cui è stato eseguito l'upgrade saranno segnalati da un asterisco (`*`) nel modulo dell'ordine e da un'indicazione che ti avvisa che eseguirai il provisioning dell'archiviazione con la crittografia. Una volta eseguito il provisioning dell'archiviazione, vedrai un'icona nell'elenco archiviazioni che mostra tale archiviazione come crittografata. Il provisioning di tutti i volumi crittografati e di tutti i LUN viene eseguito solo nei data center di cui è stato eseguito l'upgrade. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](new-ibm-block-and-file-storage-location-and-features.html).

## Perché posso eseguire il provisioning di {{site.data.keyword.blockstorageshort}} con un livello Endurance 10 IOPS in alcuni data center e non in altri?
Il livello 10 IOPS/GB del tipo Endurance è disponibile solo in data center selezionati; a tale selezione verranno a breve aggiunti dei nuovi data center. Puoi trovare un elenco completo dei data center di cui è stato eseguito l'upgrade e delle funzioni disponibili [qui](new-ibm-block-and-file-storage-location-and-features.html).

## Posso crittografare il mio {{site.data.keyword.blockstorageshort}} se ho un {{site.data.keyword.blockstorageshort}} non crittografato di cui è stato eseguito il provisioning in un data center di cui è stato eseguito l'upgrade per la crittografia?

Non è possibile crittografare il {{site.data.keyword.blockstorageshort}} di cui viene eseguito il provisioning prima dell'upgrade del data center.
Il nuovo {{site.data.keyword.blockstorageshort}} di cui è stato eseguito il provisioning in data center di cui è stato eseguito l'upgrade viene crittografato automaticamente.
Non c'è alcuna impostazione di crittografia da cui scegliere, l'operazione è automatica.
I dati su un'archiviazione non crittografata in un data center di cui è stato eseguito l'upgrade possono essere crittografati creando un nuovo LUN di blocchi e copiando quindi i dati nel nuovo LUN crittografato con una migrazione basata sull'host. Leggi questo [articolo](migrate-block-storage-encrypted-block-storage.html) per le istruzioni.

## Di quanti volumi possono eseguire il provisioning?

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare i tuoi volumi, contatta il tuo rappresentante di vendita.

## Sarò in grado di raggiungere una velocità effettiva maggiore se uso una connessione Ethernet più rapida?

I limiti di velocità effettiva sono impostati su un livello per volume/LUN; pertanto, l'utilizzo di una connessione Ethernet più veloce non aumenterà tale limite impostato. Tuttavia, con una connessione Ethernet più lenta, la tua larghezza di banda può essere un potenziale collo di bottiglia.

## I firewall/gruppi di sicurezza hanno ripercussioni sulle prestazioni?

Come prassi ottimale, consigliamo di eseguire il traffico di archiviazione su una VLAN che ignora il firewall. L'esecuzione del traffico di archiviazione tramite i firewall software aumenterà la latenza e avrà un impatto negativo sulle prestazioni dell'archiviazione.

## Che latenza delle prestazioni mi posso aspettare dal mio {{site.data.keyword.blockstorageshort}}?   

La latenza di destinazione nell'archiviazione è di <1 ms. La nostra archiviazione è connessa alle istanze di elaborazione su una rete condivisa e, pertanto, la latenza delle prestazioni esatta dipenderà dal traffico di rete entro uno specifico periodo di tempo.

## {{site.data.keyword.blockstorageshort}} supporta la prenotazione permanente SCSI-3 per implementare il fencing I/O per Db2 pureScale?

Sì, {{site.data.keyword.blockstorageshort}} supporta entrambe le prenotazioni permanenti SCSI-2 e SCSI-3.

## Che succede ai miei dati quando i LUN {{site.data.keyword.blockstorageshort}} vengono eliminati?

Quando l'archiviazione viene eliminata, gli eventuali puntatori ai dati su tale volume vengono rimossi e, pertanto, i dati diventano completamente inaccessibili. Se viene eseguito nuovamente il provisioning dell'archiviazione fisica a un altro account, viene assegnato un nuovo set di puntatori. Non vi è alcuna possibilità che il nuovo account acceda ad eventuali dati che potrebbero essere stati sull'archiviazione fisica; il nuovo set di puntatori mostra tutti 0. Quando nel volume/LUN vengono scritti dei nuovi dati, gli eventuali dati inaccessibili ancora esistenti verranno sovrascritti.

## Cosa succede ai driver che vengono disattivati dal data center cloud?

Quando i driver vengono disattivati, IBM li distrugge prima di eliminarli, rendendoli quindi inutilizzabili. Tutti i dati scritti in questi driver diventano inaccessibili.
