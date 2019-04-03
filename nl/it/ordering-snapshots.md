---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Ordinazione di istantanee
{: #orderingsnapshots}

Per creare le istantanee del tuo volume di archiviazione, in modo automatizzato o manualmente, devi acquistare dello spazio per contenerle. Puoi acquistare capacità fino al tuo ammontare del volume di archiviazione (durante l'acquisto di volume iniziale e successivamente attenendoti alla procedura qui descritta).

## Determinazione di quanto spazio di istantanea ordinare

In linea generale, lo spazio di istantanea viene utilizzato dalle istantanee in base a due fattori chiave:
- La quantità di modifiche del file system attivo nel tempo,
- Per quanto tempo prevedi di conservare le istantanee.  

Il modo per calcolare la quantità di spazio necessaria è **(Frequenza di modifica)** x **(numero di ore/giorni/settimane/mesi di conservazione dei dati)**.

La prima istantanea utilizza una quantità trascurabile di spazio poiché è solo una copia dei metadati (puntatori) che indicano i blocchi di file system attivi.
{:note}

Un volume con molte modifiche e un lungo periodo di conservazione ha bisogno di più spazio rispetto a un volume con meno modifiche e una pianificazione di conservazione moderata. Un esempio per il primo tipo è un database a tasso di modifica elevato. Un esempio per il secondo tipo è un archivio dati VMware.

Se acquisisci 12 istantanee orarie di 500 GB di dati effettivi ed è presente l'1 percento di modifica tra ciascuna istantanea, ti ritrovi alla fine con 60 GB per le istantanee.

*(5-GB di tasso di modifica) x (12 istantanee orarie) = (60 GB di spazio utilizzato)*

Se invece per tali 500 GB di dati effettivi, con 12 istantanee orarie, si verificasse il 10 percento di modifica ogni ora, lo spazio dell'istantanea che viene utilizzato è 600 GB.

*(50-GB di tasso di modifica) x (12 istantanee orarie) = (600 GB di spazio utilizzato)*

Pertanto, quanto determini la quantità di spazio di istantanea di cui hai bisogno, considera attentamente la frequenza di modifica. È un fattore che ha un enorme impatto sulla quantità di spazio di istantanea di cui hai bisogno. Un volume più grande è più probabile che cambi più spesso. Tuttavia, un volume da 500-GB con 5 GB di modifica e un volume di 10-TB con 5 GB di modifica utilizzano la stessa quantità di spazio dell'istantanea.

Inoltre, per la maggior parte dei carichi di lavoro, più grande è un volume e minore è lo spazio da mettere da parte inizialmente. Ciò è dovuto principalmente all'efficienza dei dati sottostanti e alla natura della modalità di funzionamento delle istantanee nell'ambiente.

## Ordinazione dello spazio di istantanea tramite la console {{site.data.keyword.cloud_notm}}

1. Accedi alla [Console {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog){:new_window} e fai clic sull'icona menu in alto a sinistra. Seleziona **Classic Infrastructure**.

   In alternativa, puoi accedere al [{{site.data.keyword.slportal}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://control.softlayer.com/){:new_window}.
2. Accedi al tuo LUN di archiviazione tramite **Storage** >**{{site.data.keyword.blockstorageshort}}**.
2. Fai clic su **Change Snapshot Space** nel frame Snapshots.
3. Seleziona la quantità di spazio che ti serve e il metodo di pagamento.
4. Fai clic su **Continue**.
5. Immetti l'eventuale codice promozionale (**Promo Code**) a tua disposizione e fai clic su **Recalculate**. Gli addebiti (Charges) per questo ordine e il riesame dell'ordine (Order Review) vengono completati per impostazione predefinita.

   Gli sconti vengono applicati quando l'ordine viene elaborato.
   {:note}
6. Seleziona la casella **I have read the Master Service Agreement and agree to the terms therein** e fai clic su **Place Order**. Nel giro di pochi minuti, viene eseguito il provisioning del tuo spazio di istantanea.

## Ordinazione dello spazio di istantanea tramite la CLI SL

```
# slcli block snapshot-order --help
Usage: slcli block snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the block
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
{:codeblock}
