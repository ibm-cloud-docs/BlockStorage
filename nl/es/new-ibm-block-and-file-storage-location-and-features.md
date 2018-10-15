---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-17"

---
{:new_window: target="_blank"}

# Nuevas ubicaciones y características de {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.BluSoftlayer_full}} presenta una nueva versión de {{site.data.keyword.blockstoragefull}}.

El nuevo almacenamiento está disponible en centros de datos seleccionados, y está respaldado por el almacenamiento flash a niveles de IOPS superiores con cifrado de disco para datos en reposo. Todo el almacenamiento suministrado en los centros de datos actualizados se creará automáticamente con la nueva versión.

**Nota:** El punto de montaje de NFS para los volúmenes nuevos difiere del punto de montaje de volúmenes no cifrados. Consulte la sección **Nuevo punto de montaje para volúmenes de {{site.data.keyword.filestorage_short}} cifrados** para obtener detalles.

El nuevo {{site.data.keyword.blockstorageshort}} está disponible en las siguientes regiones/centros de datos.
<table role="presentation">
  <tr>
    <td><strong>EE.UU. 2</strong></td>
    <td><strong>UE</strong></td>
    <td><strong>Australia</strong></td>
    <td><strong>Canadá</strong></td>
    <td><strong>Latinoamérica</strong></td>
    <td><strong>Asia-Pacífico</strong></td>
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
	<br /><br /><br /><br /><br /><br /><br /><br /><br />
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

*La Tabla 1 muestra nuestra disponibilidad de centro de datos. Cada región tiene su propia columna. Algunas ciudades, como Dallas, San José, Washington DC, Ámsterdam, Frankfurt, Londres y Sídney, tienen varios centros de datos.*

El nuevo almacenamiento tiene las siguientes características y funciones:

- **[Cifrado gestionado por el proveedor para datos en reposo](block-file-storage-encryption-rest.html)**.
  Todos los {{site.data.keyword.blockstorageshort}} se suministran automáticamente como cifrados sin ningún cargo adicional.
- **Opción de nivel de 10 IOPS por GB**.
  Se ha añadido un nuevo nivel con el tipo de resistencia al {{site.data.keyword.blockstorageshort}} para dar soporte a las cargas de trabajo más exigentes.
- **Almacenamiento respaldado por all flash.**
  Todo el {{site.data.keyword.blockstorageshort}} suministrado con el tipo Resistencia o Rendimiento a 2 IOPS por GB o superior está respaldado por almacenamiento all flash.
- Soporte de **Instantánea y réplica** con {{site.data.keyword.blockstorageshort}}
- Se ha añadido la opción de **Facturación por hora** para el almacenamiento cuya previsión de uso no alcanza un mes completo.
- **Hasta 48.000 IOPS** para {{site.data.keyword.blockstorageshort}} suministrado con la opción de Rendimiento.
- **Las tasas de IOPS se pueden ajustar** para mejorar el rendimiento durante cambios estacionales de la carga de trabajo. [Aquí](adjustable-iops.html) puede leer más información sobre esta característica.
- Cree un clon de sus datos con la característica de **[duplicación de volúmenes de {{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html)**.
- El **almacenamiento se puede ampliar** en incrementos de GB hasta 12 TB, sin necesidad de crear un duplicado ni de mover datos manualmente a un volumen de mayor tamaño. [Aquí](expandable_block_storage.html) puede leer más información sobre esta característica.

## Nuevo punto de montaje para volúmenes de almacenamiento cifrados

Todos los volúmenes de almacenamiento mejorados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para sus volúmenes de almacenamiento, puede consultar la información sobre el punto de montaje en la página **Detalles del volumen** de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Aquí puede consultar si se han actualizado más centros de datos y si se han añadido nuevas funciones y características a {{site.data.keyword.blockstorageshort}}.
