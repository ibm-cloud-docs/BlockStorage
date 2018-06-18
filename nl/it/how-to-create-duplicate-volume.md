---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Creazione di un volume di blocco duplicato

{{site.data.keyword.BluSoftlayer_full}} consente di creare un duplicato di un {{site.data.keyword.blockstoragefull}} esistente. Il volume duplicato eredita per impostazione predefinita le opzioni di capacità e prestazioni del LUN/volume originale e ha una copia dei dati che arriva fino al punto temporale di un'istantanea.   

Poiché il duplicato è basato sui dati in un'istantanea a un punto temporale, è necessario lo spazio di istantanea sul volume originale, prima che tu possa creare un duplicato. Per ulteriori informazioni sulle istantanee e su come ordinare lo spazio di istantanea, consulta la [documentazione delle istantanee](snapshots.html).  

I duplicati possono essere creati sia dal volume primario che da quello di replica. Il nuovo duplicato viene creato nello stesso data center del volume originale. Ad esempio, se crei un duplicato da un volume di replica, il nuovo volume verrà creato nello stesso data center del volume di replica.    

I volumi duplicati solo accessibili da un host per la lettura/scrittura non appena viene seguito il provisioning dell'archiviazione. Tuttavia, le istantanee e le repliche saranno consentite solo dopo il completamento della copia dei dati dall'originale al duplicato. 

Una volta completata la copia dei dati, il duplicato può essere gestito e utilizzato come un volume del tutto indipendente. 

Questa funzione è disponibile solo per l'archiviazione di cui viene eseguito il provisioning con la crittografia. Fai clic [qui](new-ibm-block-and-file-storage-location-and-features.html) per l'elenco dei data center disponibili. 

Alcuni utilizzi comuni per un volume duplicato:
- **Esecuzione di test di ripristini di emergenza**: Crea un duplicato del tuo volume di copia per verificare che i dati siano intatti e che possano essere utilizzati in caso di emergenza, senza interrompere la replica. 
- **Copia di versione finale**: usa un volume di archiviazione come copia di versione finale da cui puoi creare più istanze per vari usi. 
- **Aggiornamento dei dati**: crea una copia dei tuoi dati di produzione da montare al tuo ambiente non di produzione per l'esecuzione di test. 
- **Ripristino da un'istantanea**: ripristina i dati sul volume originale con file/data specifici da un'istantanea senza sovrascrivere l'intero volume originale con la funzione di ripristino di istantanea. 
- **Sviluppo/test**: crea fino a quattro duplicati simultanei per volta di un volume per creare dati duplicati per attività di sviluppo e test. 
- **Modifica delle dimensioni dell'archiviazione**: crea un volume con la nuova dimensione, il nuovo tasso di IOPS o entrambi senza dover eseguire una migrazione basata sull'host dei tuoi dati.  
	

Servendoti del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puoi creare un volume duplicato in un paio di modi: 

## Come creare un duplicato da un volume specifico nell'elenco archiviazioni

Vai al tuo elenco di {{site.data.keyword.blockstorageshort}}:

1. Dal portale clienti, fai clic su **Storage**, **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure->Storage>{{site.data.keyword.blockstorageshort}}**. 
2. Seleziona un LUN dall'elenco e fai clic su **Actions** -> **Duplicate LUN (Volume)** 
3. Scegli la tua opzione di istantanea: 
    - In caso di ordinazione da un volume non di replica:
      - Seleziona **Create from new snapshot** - questo creerà una istantanea che verrà utilizzata per il duplicato. Usa questa opzione se non ci sono attualmente istantanee per il tuo volume o se vuoi creare un duplicato in quel preciso momento.
    
      OPPURE 
      - Seleziona **Create from latest snapshot** - questo creerà un duplicato dall'istantanea più recente che esiste per questo volume. 
    - In caso da ordinazione da un volume di replica - la sola opzione per l'istantanea consiste nell'utilizzare l'istantanea più recente disponibile. 
4. Il tipo di archiviazione (Endurance o Performance) e l'ubicazione rimarranno gli stessi del volume originale.
5. Fatturazione oraria o mensile - puoi scegliere di eseguire il provisioning del LUN duplicato con fatturazione oraria o mensile. Il tipo di fatturazione per il volume originale viene selezionato automaticamente. Se vuoi scegliere un tipo di fatturazione differente per l'archiviazione duplicata, puoi operare tale selezione qui. 
5. Volendo, puoi specificare l'IOPS o il livello IOPS per il nuovo volume. La designazione degli IOPS del volume originale è impostata per impostazione predefinita.Verranno visualizzate le combinazioni di Performance e dimensioni disponibili.
    - Se il tuo volume originale è al livello Endurance 0,25 IOPS, non potrai operare una nuova selezione. 
    - Se il tuo volume originale è al livello Endurance 2, 4 o 10 IOPS, puoi spostarti dovunque tra questi livelli per il nuovo volume. 
6. Puoi aggiornare la dimensione del nuovo volume in modo che sia più grande dell'originale. La dimensione del volume originale è impostata per impostazione predefinita. 
    - **Nota**: la dimensione di {{site.data.keyword.blockstorageshort}} può essere modificata solo a 10 volte la dimensione originale del volume. 
7. Puoi aggiornare lo spazio di istantanea per il nuovo volume per aggiungere più, meno o zero spazio di istantanea. Lo spazio di istantanea del volume originale viene impostato per impostazione predefinita. 
8. Fai clic su **Continue** per effettuare il tuo ordine. 



## Come creare un duplicato da un'istantanea specifica

Accedi al tuo elenco di {{site.data.keyword.blockstorageshort}}:

1. Fai clic su un **LUN/volume** dall'elenco per visualizzare la pagina dei dettagli. (Può essere un volume di replica o non di replica). 
2. Scorri verso il basso e selezioni un'istantanea esistente dall'elenco nella pagina dei dettagli e fai clic su **Actions -> Duplicate**.   
3. Il tipo di archiviazione (Endurance o Performance) e l'ubicazione rimarranno gli stessi del volume originale. 
4. Vengono visualizzate le combinazioni di Performance e dimensioni disponibili.La designazione degli IOPS del volume originale è impostata per impostazione predefinita. Puoi specificare l'IOPS o il livello IOPS per il nuovo volume.  
    - Se il tuo volume originale è al livello Endurance 0,25 IOPS, non potrai operare una nuova selezione. 
    - Se il tuo volume originale è al livello Endurance 2, 4 o 10 IOPS, puoi spostarti dovunque tra questi livelli per il nuovo volume. 
5. Puoi aggiornare la dimensione del nuovo volume in modo che sia più grande dell'originale. La dimensione del volume originale è impostata per impostazione predefinita. 
    - **Nota**: la dimensione di {{site.data.keyword.blockstorageshort}} può essere modificata solo a 10 volte la dimensione originale del volume. 
6. Puoi aggiornare lo spazio di istantanea per il nuovo volume per aggiungere più, meno o zero spazio di istantanea. Lo spazio di istantanea del volume originale viene impostato per impostazione predefinita. 
7. Fai clic su **Continue** per effettuare il tuo ordine per il duplicato. 


## Come gestire il tuo volume duplicato

Mentre i dati vengono copiati dal volume originale al duplicato, vedrai un stato nella pagina dei dettagli che indica che la duplicazione è in corso. Durante questo lasso di tempo, puoi collegarti a un host e leggere/scrivere sul volume ma non puoi creare pianificazioni delle istantanee. Una volta completato il processo di duplicazione, il nuovo volume sarà completamente indipendente e può essere gestito con le istantanee e la replica normalmente. 
