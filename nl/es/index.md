---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-22"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Acerca de {{site.data.keyword.blockstorageshort}}
{: #About}

{{site.data.keyword.cloud}} {{site.data.keyword.blockstorageshort}} es almacenamiento iSCSI persistente y de alto rendimiento, que se suministra y gestiona independientemente de las instancias de cálculo. Los LUN de {{site.data.keyword.blockstorageshort}} basados en iSCSI se conectan a dispositivos autorizados a través de conexiones de E/S de multivía de acceso (MPIO) redundantes.

{{site.data.keyword.blockstorageshort}} aporta niveles óptimos de durabilidad y disponibilidad con un excelente conjunto de características. Se basa en las prácticas recomendadas y en los estándares de la industria. {{site.data.keyword.blockstorageshort}} se ha diseñado para proteger la integridad de los datos y mantener la disponibilidad en casos de mantenimiento y fallos imprevistos, así como proporcionar una línea base de rendimiento coherente.

## Características principales
{: #corefeatures}

Aproveche las siguientes características de {{site.data.keyword.blockstorageshort}}:

- **Línea base de rendimiento coherente**
   - Se proporciona mediante la asignación de IOPS a nivel de protocolo a volúmenes individuales.
- **Muy duradero y resistente**
   - Protege la integridad de los datos y mantiene la disponibilidad en sucesos de mantenimiento y fallos imprevistos sin la necesidad de crear y gestionar una matriz redundante a nivel de sistema operativo de matrices de discos independientes (RAID).
- **Cifrado de datos en reposo** ([disponible en centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Cifrado gestionado por el proveedor de datos en reposo sin ningún coste adicional.
- **Almacenamiento All Flash respaldado** ([disponible en centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Almacenamiento all flash para volúmenes suministrados con Resistencia o Rendimiento a 2 IOPS/GB o niveles superiores.
- **Instantáneas** ([disponible en centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Captura instantáneas de datos en un momento específico sin interrupción.
- **Réplica** ([disponible en centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Copia automáticamente instantáneas a un centro de datos de {{site.data.keyword.cloud}} asociado.
- **Conectividad de alta disponibilidad**
   - Utiliza conexiones de red redundantes para maximizar la disponibilidad
   - {{site.data.keyword.blockstorageshort}} basado en iSCSI utiliza E/S de multivía de acceso (MPIO).
- **Acceso simultáneo**
   - Permite a varios hosts acceder a volúmenes de bloques simultáneamente (hasta ocho) para configuraciones en clúster.
- **Bases de datos en clúster**
   - Da soporte a casos de uso avanzados, como bases de datos en clúster.


## Suministro
{: #provisioning}

Los LUN de {{site.data.keyword.blockstorageshort}} se pueden suministrar de 20 GB a 12 TB con dos opciones: <br/>
- Suministro de niveles de **Resistencia** que presentan niveles de rendimiento predefinidos y otras características como instantáneas y réplica.
- Crear un entorno de **Rendimiento** de alta potencia con operaciones de entrada/salida asignadas por segundo (IOPS).


### Suministro con niveles de Resistencia
{: #provendurance}

{{site.data.keyword.blockstorageshort}} de Resistencia está disponible en cuatro niveles de rendimiento de IOPS para dar soporte a las distintas necesidades de las aplicaciones. <br />

- **0,25 IOPS por GB** está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en cualquier momento. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.

- **2 IOPS por GB** está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.

- **4 IOPS por GB** está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en cualquier momento. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.

- **10 IOPS por GB** está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL y el proceso de datos para Analytics. Este nivel está disponible para almacenamiento suministrado de hasta 4 TB solo en [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

Hay disponibles hasta 48.000 IOPS con el volumen de Resistencia de 12 TB.

Elegir el nivel de Resistencia adecuado para su carga de trabajo es clave. Es igualmente importante utilizar valores adecuados para el tamaño de bloque, la velocidad de conexión de Ethernet y el número de hosts necesarios para alcanzar el máximo rendimiento. Si alguno de estos componentes no se alinea con los demás, puede afectar negativamente sobre el rendimiento final.


### Suministro con Rendimiento
{: #provperformance}

El Rendimiento es una clase de {{site.data.keyword.blockstorageshort}} diseñada para dar soporte a aplicaciones de alto nivel de entrada/salida con requisitos de rendimiento definidos, que no encajan en un nivel de Resistencia. El rendimiento previsible se consigue mediante la asignación de IOPS a nivel de protocolo a volúmenes individuales. Se pueden suministrar varias tasas de IOPS (de 100 a 48.000) con tamaños de almacenamiento que oscilan entre los 20 GB y los 12 TB.

Se requiere una conexión de interfaz para pequeños sistemas (iSCSI) de E/S de multivía de acceso (MPIO) para acceder y montar el rendimiento para {{site.data.keyword.blockstorageshort}}. {{site.data.keyword.blockstorageshort}} se suele utilizar cuando un único servidor accede al volumen. Se pueden montar varios volúmenes en un host y juntarse para conseguir volúmenes más grandes y recuentos de IOPS más altos. Se pueden realizar pedidos de volúmenes de rendimiento de acuerdo con los tamaños y las tasas de IOPS indicados en la Tabla 3 para sistemas operativos Linux, XEN y Windows.

| Tamaño (GB) | IOPS mín. | IOPS máx.
|-----|-----|-----|
| 20 | 100 | 1.000 |
| 40 | 100 | 2.000  |
| 80 | 100 | 4.000 |
| 100 | 100 | 6.000 |
| 250 | 100 | 6.000 |
| 500 | 100  | 6.000 o 10.000 |
| 1.000 | 100 | 6.000 o 20.000 ![Nota a pie de página](/images/numberone.png) |
| 2.000 | 200 | 6.000 o 40.000 ![Nota a pie de página](/images/numberone.png) |
| 3.000-7.000 | 300 | 6.000 o 48.000 ![Nota a pie de página](/images/numberone.png) |
| 8.000-9.000 | 500 | 6.000 o 48.000 ![Nota a pie de página](/images/numberone.png) |
| 10.000-12.000 | 1.000 | 6.000 o 48.000 ![Nota a pie de página](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparación de tabla" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![Nota a pie de página](/images/numberone.png) *El límite de IOPS superior a 6.000 está disponible en centros de datos seleccionados.*

Los volúmenes de rendimiento están diseñados para funcionar constantemente cerca del nivel de IOPS suministrado. La coherencia facilita el dimensionamiento y la escalabilidad de los entornos de aplicaciones con un determinado nivel de rendimiento. Además, es posible optimizar un entorno creando un volumen con la proporción ideal de precio-rendimiento.

## Facturación
{: #billing}

Puede elegir entre facturación por horas o mensual para un LUN de bloque. El tipo de facturación seleccionado para un LUN se aplica a su espacio de instantáneas y réplicas. Por ejemplo, si suministra un LUN con facturación por horas, todas las tasas de instantáneas o réplicas se facturan por horas. Si suministra un LUN con facturación mensual, todas las tasas de instantáneas o réplicas se facturan mensualmente.

 * Con la **facturación por horas**, el número de horas que el LUN de bloque ha existido en la cuenta se calcula en el momento en que se suprime el LUN o al final del ciclo de facturación, lo que se produzca primero. La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas está disponible para el almacenamiento suministrado solo en [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

 * Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. Si se suprime un LUN antes de finalizar el ciclo de facturación, no se reembolsará. La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).

### Resistencia
{: #pricing-comparison-endurance}

| Opciones de precios para niveles de IOPS predefinidos | 0,25 IOPS | 2 IOPS/GB | 4 IOPS/GB | 10 IOPS/GB |
|-----|-----|-----|-----|-----|
| Precio mensual | 0,06 USD/GB | 0,15 USD/GB | 0,20 USD/GB | 0,58 USD/GB |
| Precio por hora | 0,0001 USD/GB | 0,0002 USD/GB | 0,0003 USD/GB | 0,0009 USD/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparación de tabla" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Rendimiento
{: #pricing-comparison-performance}

| Opciones de precios para IOPS personalizados | Cálculo de precio |
|-----|-----|
| Precio mensual | 0,10 USD/GB + 0,07 USD/IOPS |
| Precio por hora | 0,0001 USD/GB + 0,0002 USD/IOPS |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparación de tabla" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
