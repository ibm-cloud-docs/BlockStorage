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

# Nuevas ubicaciones y características
{: #news}

{{site.data.keyword.cloud}} presenta una nueva versión de {{site.data.keyword.blockstoragefull}}.

El nuevo almacenamiento está disponible en centros de datos seleccionados, y está respaldado por el almacenamiento flash a niveles de IOPS superiores con cifrado de disco para datos en reposo. Todo el almacenamiento suministrado en los centros de datos actualizados se creará automáticamente con la nueva versión.

El punto de montaje de NFS para los volúmenes nuevos difiere del punto de montaje de volúmenes no cifrados. Para obtener más información, consulte la sección [Nuevo punto de montaje para volúmenes de {{site.data.keyword.blockstorageshort}} cifrados](#mountpoints).
{:important}

## Nuevas ubicaciones
{: #new-locations}

El nuevo {{site.data.keyword.blockstorageshort}} está disponible en las siguientes regiones y centros de datos.

|EE.UU. 2|Latinoamérica|Canadá|UE|Asia Pacífico|Australia|
|-----|-----|-----|-----|-----|------|
| DAL09<br >DAL10<br />DAL12<br />DAL13<br />SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07 | MEX01<br />SAO01 | MON01<br />TOR01  | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />MIL01<br />OSLO1<br />PAR01 | CHE01<br />HKG02<br />SEO01<br />SNG01<br />TOK02<br />TOK04<br />TOK05 | MEL01<br />SYD01<br />SYD04<br />SYD05 |
{: caption="La Tabla 1 muestra la disponibilidad de los centros de datos. Cada región tiene su propia columna. Algunas ciudades, como Dallas, San José, Washington DC, Ámsterdam, Frankfurt, Londres y Sídney, tienen varios centros de datos." caption-side="top"}


## Nuevas características y prestaciones
{: #features}

- **[Cifrado gestionado por el proveedor para datos en reposo](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption)**.
  Todos los {{site.data.keyword.blockstorageshort}} se suministran automáticamente como cifrados sin ningún cargo adicional.
- **Opción de nivel de 10 IOPS por GB**.
  Se ha añadido un nuevo nivel con el tipo de resistencia al {{site.data.keyword.blockstorageshort}} para dar soporte a las cargas de trabajo más exigentes.
- **Almacenamiento respaldado por all flash.**
  Todo el {{site.data.keyword.blockstorageshort}} suministrado con el tipo Resistencia o Rendimiento a 2 IOPS por GB o superior está respaldado por almacenamiento all flash.
- Soporte de **Instantánea y réplica** con {{site.data.keyword.blockstorageshort}}
- Se ha añadido la opción de **Facturación por hora** para el almacenamiento cuya previsión de uso no alcanza un mes completo.
- **Hasta 48.000 IOPS** para {{site.data.keyword.blockstorageshort}} suministrado con la opción de Rendimiento.
- **Las tasas de IOPS se pueden ajustar** para mejorar el rendimiento durante cambios estacionales de la carga de trabajo. [Aquí](/docs/infrastructure/BlockStorage?topic=BlockStorage-adjustingIOPS) puede leer más información sobre esta característica.
- Cree un clon de sus datos con la característica de **[Duplicación de volúmenes de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume)**.
- El **almacenamiento se puede ampliar** en incrementos de GB hasta 12 TB, sin necesidad de crear un duplicado ni de mover datos manualmente a un volumen de mayor tamaño. [Aquí](/docs/infrastructure/BlockStorage?topic=BlockStorage-expandingcapacity) puede leer más información sobre esta característica.

## Nuevo punto de montaje para volúmenes de almacenamiento cifrados
{: #mountpoints}

Todos los volúmenes de almacenamiento mejorados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Compruebe la información sobre el punto de montaje en la página **Detalles del volumen** de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} para asegurarse de que utiliza el punto de montaje correcto. También puede obtener la información de punto de montaje correcta a través de una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Para poder acceder a todas las nuevas características, seleccione `Storage-as-a-Service Package 759` cuando realice el pedido a través de la API. Para obtener más información sobre cómo solicitar {{site.data.keyword.blockstorageshort}} a través de la API, consulte [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
{:important}

Aquí puede consultar si se han actualizado más centros de datos y si se han añadido nuevas funciones y características a {{site.data.keyword.blockstorageshort}}.
{:tip}
