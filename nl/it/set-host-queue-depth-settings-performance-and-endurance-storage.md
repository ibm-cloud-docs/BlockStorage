---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Raccomandazione per le impostazioni della profondità di coda dell'host

{{site.data.keyword.BluSoftlayer_full}} consiglia una profondità massima di coda I/O (input/output) di applicazioni e host per ognuno dei livelli Performance. L'impostazione host non influenza la latenza di disco e controller, solo la latenza osservata dall'host e dall'applicazione.

Una profondità di coda superiore ai numeri consigliati può aumentare la latenza I/O dell'host mentre una profondità della coda al di sotto del numero consigliato può ridurre le prestazioni I/O dell'host. Poiché ciascuna applicazione è differente, sono necessarie una regolazione e un'osservazione per raggiungere le prestazioni massime dello storage.

La profondità di coda dell'host viene di norma regolata nel driver Host Bus Adaptor o nell'hypervisor e, a volte, nell'applicazione. I valori predefiniti standard quali 32 o 64 possono causare una latenza eccessiva di host o applicazioni.

Se un host o un hypervisor stanno utilizzando più livelli Performance, usa la profondità di coda per il livello più veloce e osserva la latenza sul livello Performance più lento. Se la latenza sul livello più basso è inaccettabile, regola la profondità di coda fino ad osservare un equilibrio tra latenza e prestazioni per tutti i livelli.

<table align="center">
	<tbody>
		<tr>
			<td><strong>Livello prestazioni</strong></td>
			<td style="text-align: center; vertical-align: middle;">0,25 IOPS per GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS per GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS per GB</td>
		</tr>
		<tr>
			<td><strong>Profondità massima della coda host</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
