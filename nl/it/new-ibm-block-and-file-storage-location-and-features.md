---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, new features, new locations, Block Storage, mount point changes, select data centers, ISCSI,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Nuove ubicazioni e funzioni
{: #news}

{{site.data.keyword.BluSoftlayer_full}} sta introducendo una nuova versione di {{site.data.keyword.blockstoragefull}}.

La nuova archiviazione è disponibile in data center selezionati ed è supportato da una archiviazione flash a livelli IOPS più elevati con la crittografia a livello di disco per i dati non attivi. Tutta l'archiviazione di cui viene eseguito il provisioning nei data center aggiornati viene automaticamente creata con la nuova versione.

Il punto di montaggio NFS per i nuovi volumi differisce dal punto di montaggio dei volumi non crittografati. Per ulteriori informazioni, consulta la sezione [Nuovo punto di montaggio per i volumi {{site.data.keyword.blockstorageshort}} crittografati](#mountpoints).
{:important}

## Nuove ubicazioni
{: #new-locations}

Il nuovo {{site.data.keyword.blockstorageshort}} è disponibile nei seguenti data center e regioni.
<table role="presentation">
  <tr>
    <td><strong>USA 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Australia</strong></td>
    <td><strong>Canada</strong></td>
    <td><strong>America Latina</strong></td>
    <td><strong>Asia Pacifico</strong></td>
  </tr>
  <tr>
    <td>DAL09<br />
	DAL10<br />
	DAL12<br />
	DAL13<br />
	SJC03<br />
        SJC04<br />
	WDC04<br />
	WDC06<br />
	WDC07<br />
	<br /><br /><br />
    </td>
    <td>AMS01<br />
        AMS03<br />
	FRA02<br />
	FRA04<br />
	FRA05<br />
	LON02<br />
	LON04<br />
	LON05<br />
	LON06<br />
	MIL01<br />
	OSLO1<br />
	PAR01<br />
    </td>
    <td>MEL01<br />
        SYD01<br />
        SYD04<br />
        SYD05<br />
        <br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MON01<br />
        TOR01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>MEX01<br />
        SAO01<br />
	<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
    </td>
    <td>CHE01<br />
        HKG02<br />
	SEO01<br />
	SNG01<br />
        TOK02<br />
	TOK04<br />
	TOK05<br />
	<br /><br /><br /><br /><br />
    </td>
  </tr>
</table>

*La Tabella 1 mostra la nostra disponibilità di data center. Ogni regione ha una propria colonna. Alcune città, come Dallas, San Jose, Washington DC, Amsterdam, Francoforte, Londra e Sydney hanno più data center.*

## Nuove funzioni e funzionalità
{: #features}

- **[Crittografia gestita dal provider per i dati inattivi](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)**.
  Viene eseguito automaticamente il provisioning di tutto il {{site.data.keyword.blockstorageshort}} come crittografato senza costi aggiuntivi.
- **Opzione livello 10 IOPS per GB**.
  Un nuovo livello è disponibile con il {{site.data.keyword.blockstorageshort}} di tipo Endurance per supportare i carichi di lavoro più esigenti.
- **Archiviazione con supporto all-flash.**
  Il provisioning di tutto il {{site.data.keyword.blockstorageshort}} con il tipo Endurance o Performance a 2 IOPS per GB o superiore viene eseguito con un'archiviazione con supporto all-flash.
- Supporto **Istantanea e replica** con {{site.data.keyword.blockstorageshort}}
- L'opzione di **fatturazione oraria** è disponibile per l'archiviazione di cui è pianificato l'utilizzo per meno di un intero mese.
- **Fino a 48.000 IOPS** per {{site.data.keyword.blockstorageshort}} di cui viene eseguito il provisioning con Performance.
- **I tassi di IOPS sono regolabili** per migliorare le prestazioni durante i cambiamenti del carico stagionali. Leggi ulteriori informazioni su questa funzione [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS).
- Crea un clone dei tuoi dati con la **[funzione di duplicazione del volume di {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)**.
- **L'archiviazione è espandibile** in incrementi di GB fino a 12 TB, senza che occorra creare un duplicato o spostare manualmente i dati a un volume più grande. Leggi ulteriori informazioni su questa funzione [qui](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity).

## Nuovo punto di montaggio per i volumi di archiviazione crittografati
{: #mountpoints}

Tutti i volumi di archiviazione migliorati di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Controlla le informazioni sui punti di montaggio nella pagina **Volume Details** nel [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} per assicurarti di stare utilizzando il punto di montaggio corretto. Puoi anche ottenere le informazioni sul punto di montaggio corretto tramite una chiamata API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Per poter accedere a tutte le nuove funzioni, seleziona `Storage-as-a-Service Package 759` quando inserisci il tuo ordine tramite l'API. Per ulteriori informazioni sull'ordine di {{site.data.keyword.blockstorageshort}} tramite l'API, consulta [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
{:important}

Ritorna qui a controllare quando viene eseguito l'upgrade di ulteriori data center e per vedere le nuove funzioni e funzionalità che vengono aggiunte per {{site.data.keyword.blockstorageshort}}.
{:tip}
