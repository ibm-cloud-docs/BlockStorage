---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, performance tuning, host performance improvement,

subcollection: BlockStorage

---
{:external: target="_blank" .external}

# Modifica delle impostazioni della profondità di coda dell'host
{: #hostqueuesettings}

{{site.data.keyword.BluSoftlayer_full}} consiglia una profondità massima di coda I/O (input/output) di applicazioni e host per ciascun livello Performance.

<table align="center">
  <caption>Profondità della coda consigliata per ciascun livello IOPS</caption>
        <thead>
	    <tr>
		<th>Livello Performance</th>
		<th>Profondità massima della coda host</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0,25 IOPS per GB</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2 IOPS per GB</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4 IOPS per GB</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

L'impostazione host non influenza la latenza di disco e controller. Influisce solo sulla latenza osservata dall'host e dall'applicazione.

Una profondità di coda che supera i numeri elencati può aumentare la latenza I/O dell'host, mentre una profondità della coda al di sotto del numero elencato può ridurre le prestazioni I/O dell'host. Poiché ciascuna applicazione è differente, sono necessarie una regolazione e un'osservazione per raggiungere le prestazioni massime dell'archiviazione.

La profondità di coda dell'host viene di norma regolata nel driver dell'adattatore bus host o nell'hypervisor e, a volte, nell'applicazione. I valori predefiniti standard quali 32 o 64 possono causare una latenza eccessiva di host o applicazioni.

Se un host o un hypervisor stanno utilizzando più livelli Performance, usa la profondità di coda per il livello più veloce e osserva la latenza sul livello Performance più lento.

Se la latenza sul livello più basso è inaccettabile, regola la profondità di coda fino a raggiungere un equilibrio tra latenza e prestazioni per tutti i livelli.
