---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-26"

---
{:new_window: target="_blank"}

# Ordinazione di {{site.data.keyword.blockstorageshort}}

Puoi eseguire il provisioning di {{site.data.keyword.blockstorageshort}} Storage e ottimizzare le capacità per soddisfare le tue esigenze di capacità e IOPS. Ottieni il massimo dalla tua archiviazione con due opzioni per la specifica delle prestazioni.

- Puoi scegliere tra i livelli IOPS Endurance che caratterizzano i livelli di prestazioni predefiniti per soddisfare i carichi di lavoro che non hanno requisiti di prestazioni ben definiti. 
- Puoi ottimizzare l'archiviazione per soddisfare requisiti di prestazione molto specifici specificando il numero totale di IOPS con Performance.

## Ordine di {{site.data.keyword.blockstorageshort}} con livelli IOPS predefiniti (Endurance)

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In alto a destra, fai clic su **Order {{site.data.keyword.blockstorageshort}}**.
3. Seleziona l'ubicazione (**Location**) (data center) della tua distribuzione.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione degli host di calcolo di cui disponi.
4. Fatturazione. Se hai selezionato un data center con funzionalità migliorate (contrassegnato con un asterisco), puoi scegliere tra fatturazione mensile od oraria. 
     1. Con la fatturazione **oraria**, il numero di ore per cui il LUN di blocchi è esistito sull'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione. A seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un LUN di blocchi viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).
        >**NOTA** - Il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning nei data center che **non** sono aggiornati con funzionalità migliorate.
5. Immetti la tua dimensione di archiviazione nel campo **New Storage Size**.
6. Seleziona **Endurance (tiered IOPS)** nella sezione **Storage IOPS Options**.
7. Seleziona il livello IOPS necessario per la tua applicazione.
    - **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi al momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.
    - **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco della macchina virtuale per un hypervisor.
    - **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi al momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.
    - **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html) per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB.
8. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione di istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile. Per le considerazioni e le raccomandazioni relative allo spazio per le istantanee, leggi [Ordinazione di istantanee](ordering-snapshots.html).
9. Scegli il tuo tipo di sistema operativo (**OS Type**) dall'elenco.
10. Seleziona le caselle di spunta **Terms and Conditions** e fai clic su **Place Order**.
11. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

>**Nota** - Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).<br/><br/>Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](BlockStorageFAQ.html)
 
## Ordine di {{site.data.keyword.blockstorageshort}} con IOPS personalizzati (Performance)

1. Dal [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure > Storage > {{site.data.keyword.blockstorageshort}}**.
2. In alto a destra, fai clic su **Order {{site.data.keyword.blockstorageshort}}**.
3. Fai clic su **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione degli host di calcolo di cui disponi.
4. Fatturazione. Se hai selezionato un data center con funzionalità migliorate (contrassegnato con un asterisco), puoi scegliere tra fatturazione mensile od oraria.
     1. Con la fatturazione **oraria**, il numero di ore per cui il LUN di blocchi è esistito sull'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione. A seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile solo per l'archiviazione di cui viene eseguito il provisioning in questi [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un LUN di blocchi viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).
        >**NOTA** - Il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning nei data center che **non** sono aggiornati con funzionalità migliorate.
5. Immetti la tua dimensione di archiviazione nel campo **New Storage Size**.
6. Seleziona **Performance (Allocated IOPS)** nella sezione **Storage IOPS Options**.
7. Immetti l'IOPS nel campo **Performance (Allocated IOPS)**.
8. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione di istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile. Per le considerazioni e le raccomandazioni relative allo spazio per le istantanee, leggi [Ordinazione di istantanee](ordering-snapshots.html).
9. Scegli il tuo tipo di sistema operativo (**OS Type**) dall'elenco.
10. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

>**Nota** - Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](managing-storage-limits.html).<br/><br/>Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](BlockStorageFAQ.html)

## Connessione alla tua nuova archiviazione

Quando la tua richiesta di provisioning è completa, autorizza i tuoi host ad accedere alla nuova archiviazione e configura la tua connessione. A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN iSCSI MPIO su Linux](accessing_block_storage_linux.html)
- [Connessione ai LUN iSCSI MPIO su CloudLinux](configure-iscsi-cloudlinux.html)
- [Connessione ai LUN iSCSI MPIO su Microsoft Windows](accessing-block-storage-windows.html)
- [Configurazione di Block Storage per il backup con cPanell](configure-backup-cpanel.html)
- [Configurazione di Block Storage per il backup con Plesk](configure-backup-plesk.html)

## Identificazione di {{site.data.keyword.blockstorageshort}} sulla tua fattura

Tutti i LUN vengono visualizzati nella tua fattura come un elemento di riga. Endurance viene visualizzato come “Endurance Storage Service” e Performance come "Performance Storage Service"; la tariffa varia in base al livello di archiviazione. Puoi espandere Endurance o Performance per appurare che sia {{site.data.keyword.blockstorageshort}}.
