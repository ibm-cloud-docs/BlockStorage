---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacità di archiviazione a blocchi espandibile

Con questa nuova funzione, i nostri attuali utenti {{site.data.keyword.blockstoragefull}} possono espandere la dimensione del loro {{site.data.keyword.blockstorageshort}} esistente in incrementi fino a 12 TB senza alcuna interruzione, senza dover creare un duplicato o migrare manualmente i dati a un volume di dimensione maggiore. Non si verificherà alcuna interruzione o mancanza di accesso all'archiviazione, durante l'esecuzione della modifica della dimensione. 

La fatturazione per il volume viene aggiornata automaticamente per aggiungere la differenza, calcolata proporzionalmente (pro-rata), del nuovo prezzo al ciclo di fatturazione corrente e l'intero nuovo ammontare verrà fatturato nel prossimo ciclo di fatturazione.

Questa funzione è disponibile solo in [data center selezionati](new-ibm-block-and-file-storage-location-and-features.html). 

## Perché avvantaggiarsi dell'archiviazione espandibile?

- **Gestione dei costi** – potresti essere consapevole che esiste un potenziale di crescita dei tuoi dati ma, per iniziare, ti serve una quantità di archiviazione più piccola. La possibilità di espansione consente ai nostri clienti di risparmiare sui costi di archiviazione e di crescere in un secondo momento per rispondere alle loro esigenze.  

- **Crescita delle esigenze di archiviazione** - i clienti che registrano una rapida crescita hanno bisogno di un modo per aumentare, in modo rapido e facile, la dimensione della loro archiviazione per gestire tale crescita.

## In che modo l'espansione della capacità di archiviazione si ripercuote sulla replica?

Un'azione di espansione sull'archiviazione primaria determinerà una modifica automatica della dimensione della replica. 

## Ci sono delle limitazioni?

Questa funzione è disponibile solo per l'archiviazione di cui viene eseguito il provisioning nei [data center](new-ibm-block-and-file-storage-location-and-features.html) con funzionalità migliorate. 

L'archiviazione di cui viene eseguito il provisioning sull'archiviazione aggiornata in questi data center prima della release di questa funzione (14 dicembre 2017) può essere aumentata solo di 10x la sua dimensione originale. Tutta l'altra archiviazione di cui viene eseguito il provisioning dopo tale data può essere aumentata a una dimensione massima di 12 TB. 

I limiti di dimensione esistenti per {{site.data.keyword.blockstorageshort}} di cui viene eseguito il provisioning con Endurance continuano a essere validi (fino a 4 TB per un livello 10 IOPS e fino a 12 TB per tutti gli altri livelli).

## Come posso distinguere se la mia archiviazione di cui è stato eseguito il provisioning è espandibile?

L'archiviazione di cui è stato eseguito il provisioning con le funzionalità avanzate è sempre crittografata da inattiva. Puoi distinguere facilmente che la tua archiviazione è idonea se ha un'icona di "blocco" accanto ad essa nell'IU del portale. 

## Come posso modificare la dimensione della mia archiviazione?

1. Dal {{site.data.keyword.slportal}}, fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** OPPURE, dal catalogo {{site.data.keyword.BluSoftlayer_full}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleziona il LUN dall'elenco e fai clic su **Actions** > **Modify LUN**
3. Immetti la nuova dimensione dell'archiviazione in GB.
4. Riesamina la tua selezione e il nuovo prezzo.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione dovrebbe essere disponibile in pochi minuti.
  
