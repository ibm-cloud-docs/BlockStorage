---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:ui-linked}


# Ordinazione di {{site.data.keyword.blockstorageshort}} tramite la console
{: #orderingthroughConsole}

Puoi eseguire il provisioning di {{site.data.keyword.blockstorageshort}} e ottimizzare le capacità per soddisfare le tue esigenze di capacità e IOPS. Ottieni il massimo dalla tua archiviazione con due opzioni per la specifica delle prestazioni.

- Puoi eseguire il provisioning con i livelli **Endurance** che offrono dei livelli di prestazioni predefiniti per soddisfare i carichi di lavoro che non hanno requisiti di prestazioni ben definiti.
- Puoi ottimizzare la tua archiviazione per soddisfare requisiti di prestazione specifici e creare un ambiente **Performance** con prestazioni elevate specificando il numero totale di operazioni di input/output al secondo (IOPS)

## Ordinazione di {{site.data.keyword.blockstorageshort}} con livelli IOPS predefiniti (Endurance)
{: #orderingthroughConsoleEndurance}

1. Accedi al [catalogo {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog){: external} e fai clic su **Archiviazione**. Seleziona quindi **{{site.data.keyword.blockstorageshort}}** e fai clic su **Crea**.
2. Seleziona l'ubicazione (**Location**) (data center) della tua distribuzione.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione degli host di calcolo di cui disponi.
3. Fatturazione. Se hai selezionato un data center con funzionalità migliorate (contrassegnato con un asterisco), puoi scegliere tra fatturazione mensile od oraria.
     1. Con la fatturazione **oraria**, il numero di ore per cui il LUN di blocchi è esistito sull'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione. A seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile per l'archiviazione di cui viene eseguito il provisioning in questi [data center](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un LUN di blocchi viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).

        Il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning nei data center che **non** sono aggiornati con funzionalità migliorate.
        {:important}
4. Immetti la tua dimensione di archiviazione nel campo **New Storage Size**.
5. Seleziona **Endurance (tiered IOPS)** nella sezione **Storage IOPS Options**.
6. Seleziona il livello IOPS necessario per la tua applicazione.
    - **0,25 IOPS per GB** è progettato per carichi di lavori con bassa intensità di I/O. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'ampia percentuale di dati inattivi al momento. Delle applicazioni di esempio includono le caselle di posta di archiviazione o le condizioni di file a livello di reparto.
    - **2 IOPS per GB** è progettato per un utilizzo per finalità più generiche. Delle applicazioni di esempio includono le attività di host di piccoli database a supporto di applicazioni web o le immagini disco della macchina virtuale per un hypervisor.
    - **4 IOPS per GB** è progettato per i carichi di lavoro a maggiore intensità. Questi carichi di lavoro sono di norma caratterizzati dall'avere un'elevata percentuale di dati attivi al momento. Delle applicazioni di esempio includono i database transazionali e altri database sensibili alle prestazioni.
    - **10 IOPS per GB** è progettato per i carichi di lavoro più esigenti quali quelli creati dai database NoSQL e l'elaborazione di dati per l'analisi. Questo livello è disponibile nella [maggior parte dei data center](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC) per l'archiviazione di cui viene eseguito il provisioning fino a 4 TB.
7. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione di istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile. Per le considerazioni e le raccomandazioni relative allo spazio per le istantanee, leggi [Ordinazione di istantanee](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Scegli il tuo tipo di sistema operativo (**OS Type**) dall'elenco.<br/>

   Questa selezione si basa sul sistema operativo su cui è in esecuzione il tuo host e non può essere modificata successivamente. Ad esempio, il tuo server è Ubuntu o RHEL, seleziona Linux. Se il tuo host è un server Windows 2012 o Windows 2016, seleziona l'opzione Windows 2008+ dall'elenco. Per ulteriori informazioni sulle varie opzioni Windows, consulta [FAQ](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).
   {:tip}
9. A destra, controlla il tuo riepilogo degli ordini e applica il tuo codice promozionale se ne hai uno.

   Gli sconti vengono applicati quando l'ordine viene elaborato.
   {:note}
10. Dopo che hai riesaminato i termini e le condizioni, seleziona la casella **Ho letto e accetto gli accordi di servizio di terze parti**.
11. Fai clic su **Create**. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](/docs/infrastructure/BlockStorage?topic=block-storage-faqs)
{:important}

## Ordinazione di {{site.data.keyword.blockstorageshort}} con IOPS personalizzati (Performance)
{: #orderingthroughConsolePerformance}

1. Accedi al [catalogo {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog){: external} e fai clic su **Archiviazione**. Seleziona quindi {{site.data.keyword.blockstorageshort}} e fai clic su **Crea**.
2. Fai clic su **Location** e seleziona il tuo data center.
   - Assicurati che la nuova archiviazione venga aggiunta nella stessa ubicazione degli host di calcolo di cui disponi.
3. Fatturazione. Se hai selezionato un data center con funzionalità migliorate (contrassegnato con un asterisco), puoi scegliere tra fatturazione mensile od oraria.
     1. Con la fatturazione **oraria**, il numero di ore per cui il LUN di blocchi è esistito sull'account viene calcolato quando il LUN viene eliminato oppure alla fine del ciclo di fatturazione. A seconda di quale di queste condizioni si verifichi per prima. La fatturazione oraria è una buona scelta per l'archiviazione utilizzata per qualche giorno o per meno di un mese completo. La fatturazione oraria è disponibile per l'archiviazione di cui viene eseguito il provisioning in questi [data center](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).
     2. Con la fatturazione **mensile**, il calcolo per il prezzo è a base proporzionale dalla data di creazione alla fine del ciclo di fatturazione e viene fatturato immediatamente. Non è previsto alcun rimborso se un LUN di blocchi viene eliminato prima della fine del ciclo di fatturazione. La fatturazione mensile è una buona scelta per l'archiviazione utilizzata nei carichi di lavoro di produzione che usano dati che devono essere archiviati e a cui bisogna accedere per lunghi periodi di tempo (un mese o più).

        Il tipo di fatturazione mensile viene utilizzato per impostazione predefinita per l'archiviazione di cui viene eseguito il provisioning nei data center che **non** sono aggiornati con funzionalità migliorate.
        {:note}
4. Immetti la tua dimensione di archiviazione nel campo **New Storage Size**.
5. Seleziona **Performance (Allocated IOPS)** nella sezione **Storage IOPS Options**.
6. Immetti l'IOPS nel campo **Performance (Allocated IOPS)**.
7. Fai clic su **Specify Snapshot Space Size** e seleziona la dimensione di istantanea dall'elenco. Questo spazio è in aggiunta al tuo spazio utilizzabile. Per le considerazioni e le raccomandazioni relative allo spazio per le istantanee, leggi [Ordinazione di istantanee](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Scegli il tuo tipo di sistema operativo (**OS Type**) dall'elenco.<br/>

   Questa selezione si basa sul sistema operativo su cui è in esecuzione il tuo host e non può essere modificata successivamente. Ad esempio, il tuo server è Ubuntu o RHEL, seleziona Linux. Se il tuo host è un server Windows 2012 o Windows 2016, seleziona l'opzione Windows 2008+ dall'elenco. Per ulteriori informazioni sulle varie opzioni Windows, consulta [FAQ](/docs/infrastructure/BlockStorage?topic=BlockStorage-block-storage-faqs#windowsOStypes).
   {:tip}
9. A destra, controlla il tuo riepilogo degli ordini e applica il tuo codice promozionale se ne hai uno.

   Gli sconti vengono applicati quando l'ordine viene elaborato.
   {:note}
10. Dopo che hai riesaminato i termini e le condizioni, seleziona la casella **Ho letto e accetto gli accordi di servizio di terze parti**.
11. Fai clic su **Create**. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Per aumentare il numero dei tuoi volumi, contatta il tuo rappresentante di vendita. Troverai ulteriori informazioni sull'aumento dei limiti [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Per il limite sulle autorizzazioni simultanee, vedi le [Domande frequenti](/docs/infrastructure/BlockStorage?topic=block-storage-faqs)
{:important}

## Connessione alla tua nuova archiviazione
{: #mountingnewLUN}

Quando la tua richiesta di provisioning è completa, autorizza i tuoi host ad accedere alla nuova archiviazione e configura la tua connessione. A seconda del sistema operativo del tuo host, segui il link appropriato.
- [Connessione ai LUN su Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Connessione ai LUN su CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Connessione ai LUN su Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Montaggio di un LUN in XenServer Shared Storage](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Configurazione di Block Storage per il backup con cPanell](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurazione di Block Storage per il backup con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Considerazioni sul ripristino di emergenza

Per evitare la perdita di dati e per garantire la continuità del business, prendi in considerazione di replicare i tuoi server e l'archiviazione in un altro data center. La replica mantiene i tuoi dati sincronizzati in due diverse ubicazioni in base alla tua pianificazione dell'istantanea. Per ulteriori informazioni, consulta [Replica dei dati](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).

Se vuoi clonare il tuo volume e utilizzarlo in modo indipendente dal volume originale, consulta [Creazione di un volume di blocco duplicato](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Identificazione di {{site.data.keyword.blockstorageshort}} sulla tua fattura

Tutti i LUN vengono visualizzati nella tua fattura come un elemento di riga. Endurance viene visualizzato come “Endurance Storage Service” e Performance come "Performance Storage Service"; la tariffa varia in base al livello di archiviazione. Puoi espandere Endurance o Performance per appurare che sia {{site.data.keyword.blockstorageshort}}.
