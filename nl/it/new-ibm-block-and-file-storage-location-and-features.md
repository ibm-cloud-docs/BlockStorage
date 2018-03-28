---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Nuove ubicazioni e funzioni di {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} sta introducendo una nuova versione di {{site.data.keyword.blockstoragefull}}. 

Il nuovo storage è disponibile in data center selezionati ed è supportato da uno storage flash a livelli IOPS più elevati con la crittografia a livello di disco per i dati inattivi. Di tutto lo storage di cui viene eseguito il provisioning in data center selezionati, verrà automaticamente eseguito il provisioning con la nuova versione di {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_full}}.

**Nota:** il punto di montaggio NFS per i nuovi volumi è cambiato. Vedi **Nuovo punto di montaggio per i volumi {{site.data.keyword.filestorage_short}} crittografati** per i dettagli.

I nuovi {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} sono attualmente disponibili nelle seguenti regioni/nei seguenti data center e a breve verrà aggiunta la disponibilità di ulteriori data center.
<table style="width:100%;">
	<caption>Disponibilità del data center</caption>
	<tbody>
		<tr>
			<td><strong>USA 2</strong></td>
			<td><strong>UE</strong></td>
			<td><strong>Australia</strong></td>
			<td><strong>Canada</strong></td>
			<td><strong>America Latina</strong></td>
			<td><strong>Asia Pacifico</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				   SJC04<br />
					WDC04<br />
					WDC06<br />
					WDC07<br />
					DAL09<br />
					DAL10<br />
					DAL12<br />
					DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
					MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
						<td>
				<p>TOK02<br />
				HKG02<br />
			        SEO01<br />
				SNG01<br /><br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


La nuova archiviazione ha le seguenti funzioni e capacità:

- [Crittografia gestita dal provider per i dati inattivi](block-file-storage-encryption-rest.html). Verrà eseguito automaticamente il provisioning di tutti i {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} come crittografati senza costi aggiuntivi.
- Opzione livello 10 IOPS per GB. Un nuovo livello è stato aggiunto a {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} di tipo Endurance per i carichi di lavoro più esigenti.
- Archiviazione con supporto all-flash. {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con Endurance o Performance a 2 IOPS per GB o superiore con archiviazione con supporto all-flash.
- Il supporto di istantanea e replica con {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con Endurance o Performance.
- L'opzione di fatturazione oraria aggiunta per l'archiviazione di cui è pianificato l'utilizzo per meno di un intero mese. 
- Fino a 48.000 IOPS per {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}} di cui viene eseguito il provisioning con Performance.
- I tassi di IOPS sono regolabili per migliorare le prestazioni nel caso di cambiamenti del carico stagionali. Leggi ulteriori informazioni su questa funzione [qui](adjustable-iops.html).
- Crea un nuovo clone dei tuoi dati con la funzione di duplicazione del volume di [{{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html).
- L'archiviazione è espandibile in incrementi di GB fino a 12 TB senza alcuna interruzione, senza dover creare un duplicato o eseguire manualmente la migrazione dei dati a un volume più grande. Leggi ulteriori informazioni su questa funzione [qui](expandable_block_storage.html).

## Nuovo punto di montaggio per i volumi di archiviazione crittografati

Tutti i volumi di archiviazione crittografati di cui viene eseguito il provisioning in questi data center hanno un punto di montaggio diverso rispetto ai volumi non crittografati. Per assicurarti che stai usando il punto di montaggio corretto per entrambi i volumi di archiviazione crittografati e non crittografati, puoi visualizzare le informazioni sul punto di montaggio nella pagina Volume Details nell'IU, nonché accedere al punto di montaggio corretto tramite una chiamata API:  `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Ritorna qui a controllare quando viene eseguito l'upgrade di ulteriori data center e per vedere le nuove funzioni e funzionalità che vengono aggiunte per {{site.data.keyword.blockstorageshort}}.
