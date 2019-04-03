---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, performance tuning, host performance improvement,

subcollection: BlockStorage

---
{:new_window: target="_blank"}

# Ajuste de los valores de profundidad de cola de host
{: #hostqueuesettings}

{{site.data.keyword.BluSoftlayer_full}} sugiere una profundidad de cola máxima de entrada/salida (E/S) de la aplicación y el host para cada nivel de rendimiento.

<table align="center">
  <caption>Profundidad de cola recomendada para cada nivel de IOPS</caption>
        <thead>
	    <tr>
		<th>Nivel de rendimiento</th>
		<th>Profundidad máxima de cola de host</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0,25 IOPS por GB</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2 IOPS por GB</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4 IOPS por GB</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

El valor del host no afecta a la latencia del controlador ni del disco. Solo afecta la latencia observada por el host y la aplicación.

Una profundidad de cola que supere las cifras listadas puede incrementar la latencia de E/S del host, mientras que una profundidad de cola menor que las cifras listadas puede reducir el rendimiento de E/S del host. Como cada aplicación es diferente, será necesario observar y realizar ajustes para alcanzar el máximo rendimiento de almacenamiento.

La profundidad de cola del host normalmente se ajusta en el controlador del adaptador de bus del host o el hipervisor y, a veces, en la aplicación. Los valores predeterminados estándar, como 32 o 64, pueden provocar una latencia de aplicación o host excesiva.

Si un host o un hipervisor está utilizando varios niveles de rendimiento, utilice la profundidad de cola para el nivel más rápido y observe la latencia en el nivel de rendimiento más lento.

Si la latencia en el nivel más lento no es aceptable, ajuste la profundidad de cola hasta conseguir el equilibrio entre latencia y rendimiento para todos los niveles.
