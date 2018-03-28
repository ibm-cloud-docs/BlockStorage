---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Nuevas ubicaciones y características de {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}}

{{site.data.keyword.BluSoftlayer_full}} presenta una nueva versión de {{site.data.keyword.blockstoragefull}}. 

El nuevo almacenamiento está disponible en centros de datos seleccionados, y está respaldado por el almacenamiento flash a niveles de IOPS superiores con cifrado de disco para datos en reposo.  Todo el almacenamiento suministrado en los centros de datos seleccionados se suministrará automáticamente con la nueva versión de {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_full}}.

**Nota:** ha cambiado el punto de montaje de NFS para nuevos volúmenes. Consulte **Nuevo punto de montaje para volúmenes de {{site.data.keyword.filestorage_short}} cifrados** a continuación para obtener detalles.

Actualmente {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}} están disponibles actualmente en los siguientes centros de datos/regiones, pronto se incrementará la disponibilidad de centros de datos.
<table style="width:100%;">
	<caption>Disponibilidad de centros de datos</caption>
	<tbody>
		<tr>
			<td><strong>EE.UU. 2</strong></td>
			<td><strong>UE</strong></td>
			<td><strong>Australia</strong></td>
			<td><strong>Canadá</strong></td>
			<td><strong>Latinoamérica</strong></td>
			<td><strong>Asia-Pacífico</strong></td>
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


El nuevo almacenamiento tiene las siguientes características y funciones:

- [Cifrado gestionado por el proveedor para datos en reposo](block-file-storage-encryption-rest.html). Todos los {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}} se suministrarán automáticamente como cifrados sin ningún cargo adicional.
- Opción de nivel de 10 IOPS por GB. Se ha añadido un nuevo nivel al {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}} de tipo Resistencia para dar soporte a las cargas de trabajo más exigentes.
- Almacenamiento respaldado por all flash. {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}} suministrados con Resistencia o Rendimiento a 2 IOPS por GB o superior con respaldo del almacenamiento all-flash.
- Soporte a instantáneas y réplica con {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}} suministrado con Resistencia o Rendimiento.
- Se ha añadido la opción de facturación por horas para el almacenamiento que se prevé utilizar menos de un mes completo. 
- Hasta 48.000 IOPS para {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}} suministrado con Rendimiento.
- Las tasas de IOPS son ajustables para mejorar el rendimiento en caso de cambios de carga estacionales. [Aquí](adjustable-iops.html) puede leer más información sobre esta característica.
- Cree un nuevo clon de sus datos con la característica de [duplicación de volúmenes de {{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html).
- El almacenamiento es ampliable en incrementos de GB hasta 12 TB sobre la marcha, sin necesidad de crear un duplicado o migrar datos manualmente a un volumen más grande. [Aquí](expandable_block_storage.html) puede leer más información sobre esta característica.

## Nuevo punto de montaje para volúmenes de almacenamiento cifrados

Todos los volúmenes de almacenamiento cifrados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de almacenamiento cifrados y no cifrados, puede consultar la información de punto de montaje en la página Detalles del volumen en la interfaz de usuario, así como acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Consulte aquí para comprobar si se han actualizado centros de datos adicionales y se han añadido nuevas características y funcionalidades para {{site.data.keyword.blockstorageshort}}.
