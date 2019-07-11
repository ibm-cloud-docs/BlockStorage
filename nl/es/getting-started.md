---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:ui-linked}

# Guía de aprendizaje de iniciación
{: #getting-started}

{{site.data.keyword.blockstoragefull}} es almacenamiento iSCSI persistente y de alto rendimiento, que se suministra y gestiona independientemente de las instancias de cálculo. Las LUN de {{site.data.keyword.blockstorageshort}} basadas en iSCSI se conectan a dispositivos autorizados a través de conexiones de E/S de multivía de acceso (MPIO) redundantes.

{{site.data.keyword.blockstorageshort}} aporta niveles óptimos de durabilidad y disponibilidad con un excelente conjunto de características. Se basa en las prácticas recomendadas y en los estándares de la industria. {{site.data.keyword.blockstorageshort}} se ha diseñado para proteger la integridad de los datos y mantener la disponibilidad en casos de mantenimiento y fallos imprevistos, así como proporcionar una línea base de rendimiento coherente.
{:shortdesc}

## Antes de empezar
{: #prereqs}

Las LUN de {{site.data.keyword.blockstorageshort}} se pueden suministrar de 20 GB a 12 TB con dos opciones: <br/>
- Suministro de niveles de **Resistencia** que presentan niveles de rendimiento predefinidos y otras características como instantáneas y réplica.
- Crear un entorno de **Rendimiento** de alta potencia con operaciones de entrada/salida asignadas por segundo (IOPS).

Para obtener más información sobre la oferta de {{site.data.keyword.blockstorageshort}}, consulte [Más información sobre {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

## Consideraciones sobre el suministro

### Tamaño de bloque

Los IOPS, para Resistencia y Rendimiento, se basan en un tamaño de bloque de 16 KB con una carga de trabajo del 50/50 de lectura/escritura y 50/50 aleatoria/secuencial. Un bloque de 16 KB es el equivalente a una escritura en el volumen.
{:important}

El tamaño de bloque que utiliza la aplicación afecta directamente al rendimiento del almacenamiento. Si el tamaño de bloque utilizado por la aplicación es inferior a 16 KB, se alcanza el límite de IOPS antes que el límite de rendimiento. Por el contrario, si el tamaño de bloque utilizado por la aplicación es superior a 16 KB, se alcanza el límite de rendimiento antes que el límite de IOPS.

| Tamaño de bloque (KB) | IOPS | Rendimiento (MB/s) |
|-----|-----|-----|
| 4 | 1.000 | 16 |
| 8 | 1.000 | 16 |
| 16 | 1.000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="En la Tabla 1 se muestran ejemplos de cómo el tamaño de bloque e IOPS afectan al rendimiento.<br/>Tamaño promedio de E/S x IOPS = Rendimiento en MB/s." caption-side="top"}

### Hosts autorizados

Otro factor a tener en cuenta es el número de hosts que están utilizando el volumen. Si solo hay un host que accede al volumen, puede ser difícil alcanzar el máximo de IOPS, sobre todo en recuentos de IOPS extremos (10.000 s). Si su carga de trabajo requiere un alto rendimiento, será mejor configurar al menos dos servidores para acceder al volumen a fin de evitar un cuello de botella por un servidor.

### Conexión de red

La velocidad de la conexión de Ethernet debe ser más rápida que el rendimiento máximo previsto de su volumen. Generalmente, no espere que se sature la conexión de Ethernet más allá del 70 % del ancho de banda disponible. Por ejemplo, si tiene 6.000 IOPS y está utilizando un tamaño de bloque de 16 KB, el volumen puede manejar aproximadamente un rendimiento de 94 MBps. Si tiene una conexión de Ethernet de 1 Gbps para la LUN, se genera un cuello de botella cuando los servidores intenten utilizar el máximo rendimiento disponible. Esto se debe a que el 70 % del límite teórico de una conexión de Ethernet de 1 Gbps (125 MB por segundo) solo permitiría 88 MB por segundo.

Para alcanzar el número máximo de IOPS, es necesario disponer de los recursos de red adecuados. Otros aspectos a tener en cuenta son el uso de la red privada fuera del almacenamiento y los ajustes del lado del host y específicos de la aplicación (pila IP o [profundidades de colas](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings) y otros valores).

El tráfico de almacenamiento debería estar aislado de otros tipos de tráfico y no dirigirse a través de cortafuegos ni direccionadores. Para obtener más información, consulte [Preguntas más frecuentes](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic).

El tráfico de almacenamiento se incluye en el uso total de la red de los servidores virtuales públicos. Para obtener más información acerca de los límites que puede imponer el servicio, consulte la [Documentación de servidor virtual](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers).
{:tip}

## Envío de su pedido
{: #submitorder}

Cuando esté listo para enviar el pedido, puede realizarlo mediante la [consola](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole), la [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI) o la [CLI de IBMCLOUD](/docs/cli/reference/ibmcloud?topic=cloud-cli-sl-block-storage#sl_block_volume_order).

Para obtener información sobre cómo solicitar {{site.data.keyword.blockstorageshort}} a través de la API, consulte [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Para poder acceder a todas las nuevas características, solicite `el paquete 759 de almacenamiento como servicio`.
{:tip}

## Conexión del nuevo almacenamiento
{: #mountingstorage}

Cuando se haya completado la solicitud de suministro, autorice a los hosts a acceder al nuevo almacenamiento y configure la conexión. En función del sistema operativo del host, siga el enlace adecuado.
- [Conexión a LUN en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conexión a LUN en CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conexión a LUN en Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuración de almacenamiento en bloque para la copia de seguridad con cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuración de almacenamiento en bloque para la copia de seguridad con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Gestión del nuevo almacenamiento

Mediante el portal o SLCLI, puede gestionar diversos aspectos de su {{site.data.keyword.blockstorageshort}}, como autorizaciones de hosts y cancelaciones. Para obtener más información, consulte [Gestión de {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
