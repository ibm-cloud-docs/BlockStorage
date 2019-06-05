---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-24"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestione dei limiti di archiviazione
{: #managingstoragelimits}

Per impostazione predefinita, puoi eseguire il provisioning di un totale combinato di 250 volumi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} a livello globale.

Se non sei sicuro di quanti volumi disponi, puoi elencare i tuoi volumi per ciascun data center utilizzando il seguente comando `slcli`.
```
# slcli block volume-count --help
Usage: slcli block volume-count [OPTIONS]

Options:
  -d, --datacenter TEXT  Datacenter shortname
  --sortby TEXT          Column to sort by
  -h, --help             Show this message and exit.
```

Puoi richiedere un aumento del limite inoltrando un caso di supporto nel [portale](https://cloud.ibm.com/unifiedsupport/cases/add){: external}. Una volta approvata la richiesta, ottieni un limite di volumi impostato per uno specifico data center.  

Per richiedere un aumento del limite, apri un caso e indirizzalo al tuo rappresentante di vendita.

Nel caso, fornisci le seguenti informazioni:

- **Oggetto del ticket** (Ticket Subject): richiedi di aumentare il limite di archiviazione del conteggio di volumi del data center

- **Qual è il caso d'uso per la richiesta di volumi aggiuntivi?** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a un nuovo archivio dati VMware, un nuovo ambiente di sviluppo e test (dev/test), un database SQL o la registrazione.*

- **Quanti volumi di blocchi sono necessari in base al tipo, alla dimensione, all'IOPS e all'ubicazione?** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "25x Endurance 2 TB @ 4 IOPS in DAL09" o "25x Performance 4 TB @ 2 IOPS in WDC04".*

- **Quanti volumi file aggiuntivi sono necessari in base al tipo, alla dimensione, all'IOPS e all'ubicazione?** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "25x Performance 20 GB @ 10 IOPS in DAL09" o "50x Endurance 2 TB @ 0,25 IOPS in SJC03".*

- **Fornisci una stima di quando prevedi o pianifichi di eseguire il provisioning di tutto l'aumento di volumi richiesto.** <br />
 "*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "90 giorni".*

- **Fornisci una previsione a 90 giorni dell'utilizzo della capacità medio previsto di questi volumi.** <br />
*Ad esempio, la tua risposta potrebbe essere qualcosa di simile a "prevedo un utilizzo al 25 percento entro 30 giorni, al 50 percento entro 60 giorni e al 75 percento entro 90 giorni".*

Rispondi a tutte le domande e istruzioni nella tua richiesta. Sono necessarie per l'elaborazione e l'approvazione.
{:important}

Sarai informato dell'aggiornamento dei tuoi limiti tramite il processo del caso.
