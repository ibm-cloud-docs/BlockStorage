---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Espansione della capacità di Block Storage
{: #expandingcapacity}

Con questa nuova funzione, gli attuali utenti {{site.data.keyword.blockstoragefull}} possono espandere la dimensione del loro {{site.data.keyword.blockstorageshort}} esistente in incrementi fino a 12 TB immediatamente. Non hanno bisogno di creare un duplicato o migrare manualmente i dati a un volume di dimensione maggiore. Non si verificherà alcuna interruzione o mancanza di accesso all'archiviazione, durante l'esecuzione della modifica della dimensione.

La fatturazione per il volume viene aggiornata automaticamente per aggiungere la differenza calcolata proporzionalmente del nuovo prezzo al ciclo di fatturazione corrente. L'intero nuovo ammontare viene quindi fatturato nel prossimo ciclo di fatturazione.

Questa funzione è disponibile in [data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

## Vantaggi dell'archiviazione espandibile

- **Gestione dei costi** – potresti essere consapevole che esiste un potenziale di crescita dei tuoi dati ma, per iniziare, ti serve una quantità di archiviazione più piccola. La possibilità di espansione consente ai nostri clienti di risparmiare sui costi di archiviazione e di crescere in un secondo momento per rispondere alle loro esigenze.  

- **Crescita delle esigenze di archiviazione** - i clienti che registrano una rapida crescita dei dati hanno bisogno di un modo per aumentare, in modo rapido e facile, la dimensione della loro archiviazione per gestire tale crescita.

## Effetti dell'espansione della capacità di archiviazione sulla replica

L'azione di espansione sull'archiviazione primaria determina una modifica automatica della dimensione della replica.

## Limitazioni
{: #limitsofexpandingstorage}

Questa funzione è disponibile per l'archiviazione di cui viene eseguito il provisioning in [data center selezionati](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

L'archiviazione di cui era stato eseguito il provisioning in questi data center prima della release di questa funzione, tra l'**aprile 2017 e il 14 dicembre 2017**, può essere aumentata fino a 10 volte la sua dimensione originale e non oltre. L'archiviazione di cui è stato eseguito il provisioning dopo il **14 dicembre 2017** può essere aumentata fino a 12 TB.

I limiti di dimensione esistenti per {{site.data.keyword.blockstorageshort}} di cui viene eseguito il provisioning con Endurance continuano a essere validi (fino a 4 TB per un livello 10 IOPS e fino a 12 TB per tutti gli altri livelli).

## Ridimensionamento dell'archiviazione
{: #resizingsteps}

1. Dal {{site.data.keyword.slportal}}, fai clic su **Storage** > **{{site.data.keyword.blockstorageshort}}** OPPURE dalla console {{site.data.keyword.cloud}}, fai clic su **Infrastructure** > **Storage** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleziona il LUN dall'elenco e fai clic su **Actions** > **Modify LUN**
3. Immetti la nuova dimensione dell'archiviazione in GB.
4. Riesamina la tua selezione e la nuova determinazione del prezzo.
5. Fai clic sulla casella di spunta **I have read the Master Service Agreement...** e fai clic su **Place Order**.
6. La tua nuova allocazione di archiviazione è disponibile in pochi minuti.

In alternativa, puoi ridimensionare il tuo volume tramite la CLI SL.

```
# slcli block volume-modify --help
Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help      Show this message and exit.
```
{:codeblock}

Per ulteriori informazioni sull'espansione del filesystem (e delle eventuali partizioni) sul volume per utilizzare il nuovo spazio, consulta la documentazione del sistema operativo da te utilizzato.
{:tip}
