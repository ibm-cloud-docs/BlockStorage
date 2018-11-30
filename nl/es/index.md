---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Iniciación a {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} es almacenamiento iSCSI persistente y de alto rendimiento, que se suministra y gestiona independientemente de las instancias de cálculo. Los LUN de {{site.data.keyword.blockstorageshort}} basados en iSCSI se conectan a dispositivos autorizados a través de conexiones de E/S de multivía de acceso (MPIO) redundantes.

{{site.data.keyword.blockstorageshort}} aporta niveles óptimos de durabilidad y disponibilidad con un excelente conjunto de características. Se basa en las prácticas recomendadas y en los estándares de la industria. {{site.data.keyword.blockstorageshort}} se ha diseñado para proteger la integridad de los datos y mantener la disponibilidad en casos de mantenimiento y fallos imprevistos, así como proporcionar una línea base de rendimiento coherente.

## Características principales

Aproveche las siguientes características de {{site.data.keyword.blockstorageshort}}:

- **Línea base de rendimiento coherente**
   - Se proporciona mediante la asignación de IOPS a nivel de protocolo a volúmenes individuales.
- **Muy duradero y resistente**
   - Protege la integridad de los datos y mantiene la disponibilidad en sucesos de mantenimiento y fallos imprevistos sin la necesidad de crear y gestionar una matriz redundante a nivel de sistema operativo de matrices de discos independientes (RAID).
- **Cifrado de datos en reposo** ([disponible en centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html))
   - Cifrado gestionado por el proveedor de datos en reposo sin ningún coste adicional.
- **Almacenamiento All Flash respaldado** ([disponible en centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html))
   - Almacenamiento all flash para volúmenes suministrados con Resistencia o Rendimiento a 2 IOPS/GB o niveles superiores.
- **Instantáneas** ([disponible en centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html))
   - Captura instantáneas de datos en un momento específico sin interrupción.
- **Réplica** ([disponible en centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html))
   - Copia automáticamente instantáneas a un centro de datos de {{site.data.keyword.BluSoftlayer_full}} asociado.
- **Conectividad de alta disponibilidad**
   - Utiliza conexiones de red redundantes para maximizar la disponibilidad
   - {{site.data.keyword.blockstorageshort}} basado en iSCSI utiliza E/S de multivía de acceso (MPIO).
- **Acceso simultáneo**
   - Permite a varios hosts acceder a volúmenes de bloques simultáneamente (hasta ocho) para configuraciones en clúster.
- **Bases de datos en clúster**
   - Da soporte a casos de uso avanzados, como bases de datos en clúster.

## Facturación

Puede elegir entre facturación por horas o mensual para un LUN de bloque. El tipo de facturación seleccionado para un LUN se aplica a su espacio de instantáneas y réplicas. Por ejemplo, si suministra un LUN con facturación por horas, todas las tasas de instantáneas o réplicas se facturan por horas. Si suministra un LUN con facturación mensual, todas las tasas de instantáneas o réplicas se facturan mensualmente.

Con la **facturación por horas**, el número de horas que el LUN de bloque ha existido en la cuenta se calcula en el momento en que se suprime el LUN o al final del ciclo de facturación, lo que se produzca primero. La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas está disponible para el almacenamiento suministrado solo en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).

Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. Si se suprime un LUN antes de finalizar el ciclo de facturación, no se reembolsará. La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).

**Rendimiento**
<table>
  <caption>La Tabla 1 muestra los precios de Almacenamiento de rendimiento con facturación mensual y por horas.</caption>
  <tr>
   <th>Precio mensual</th>
   <td>0,10 USD/GB + 0,07 USD/IOPS</td>
  </tr>
  <tr>
   <th>Precio por hora</th>
   <td>0,0001 USD/GB + 0,0002 USD/IOPS</td>
  </tr>
</table>

**Resistencia**
<table>
  <caption>La Tabla 2 muestra los precios de Almacenamiento resistente para cada nivel con opciones de facturación mensual y por horas.</caption>
  <tr>
   <th>Nivel de IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Precio mensual</th>
   <td>0,06 USD/GB</td>
   <td>0,15 USD/GB</td>
   <td>0,20 USD/GB</td>
   <td>0,58 USD/GB</td>
  </tr>
  <tr>
   <th>Precio por hora</th>
   <td>0,0001 USD/GB</td>
   <td>0,0002 USD/GB</td>
   <td>0,0003 USD/GB</td>
   <td>0,0009 USD/GB</td>
  </tr>
</table>



## Suministro

Los LUN de {{site.data.keyword.blockstorageshort}} se pueden suministrar de 20 GB a 12 TB con dos opciones: <br/>
- Suministro de niveles de **Resistencia** que presentan niveles de rendimiento predefinidos y otras características como instantáneas y réplica.
- Crear un entorno de **Rendimiento** de alta potencia con operaciones de entrada/salida asignadas por segundo (IOPS).

### Suministro con niveles de Resistencia

{{site.data.keyword.blockstorageshort}} de Resistencia está disponible en cuatro niveles de rendimiento de IOPS para dar soporte a las distintas necesidades de las aplicaciones. <br />

- **0,25 IOPS por GB** está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en cualquier momento. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.

- **2 IOPS por GB** está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.

- **4 IOPS por GB** está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en cualquier momento. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.

- **10 IOPS por GB** está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL y el proceso de datos para Analytics. Este nivel está disponible para almacenamiento suministrado de hasta 4 TB solo en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).

Hay disponibles hasta 48.000 IOPS con el volumen de Resistencia de 12 TB.

Elegir el nivel de Resistencia adecuado para su carga de trabajo es clave. Es igualmente importante utilizar valores adecuados para el tamaño de bloque, la velocidad de conexión de Ethernet y el número de hosts necesarios para alcanzar el máximo rendimiento. Si alguno de estos componentes no se alinea con los demás, puede afectar negativamente sobre el rendimiento final.


### Suministro con Rendimiento

El Rendimiento es una clase de {{site.data.keyword.blockstorageshort}} diseñada para dar soporte a aplicaciones de alto nivel de entrada/salida con requisitos de rendimiento definidos, que no encajan en un nivel de Resistencia. El rendimiento previsible se consigue mediante la asignación de IOPS a nivel de protocolo a volúmenes individuales. Se pueden suministrar varias tasas de IOPS (de 100 a 48.000) con tamaños de almacenamiento que oscilan entre los 20 GB y los 12 TB.

Se requiere una conexión de interfaz para pequeños sistemas (iSCSI) de E/S de multivía de acceso (MPIO) para acceder y montar el rendimiento para {{site.data.keyword.blockstorageshort}}. {{site.data.keyword.blockstorageshort}} se suele utilizar cuando un único servidor accede al volumen. Se pueden montar varios volúmenes en un host y juntarse para conseguir volúmenes más grandes y recuentos de IOPS más altos. Se pueden realizar pedidos de volúmenes de rendimiento de acuerdo con los tamaños y las tasas de IOPS indicados en la Tabla 3 para sistemas operativos Linux, XEN y Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>La Tabla 3 muestra el tamaño y las combinaciones de IOPS para el almacenamiento de Rendimiento.<br/><sup><img src="/images/numberone.png" alt="Nota a pie de página" /></sup> El límite de IOPS superior a 6.000 está disponible en centros de datos seleccionados.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>Tamaño (GB)</th>
            <th>IOPS mín.</th>
            <th>IOPS máx.</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 o 10.000<sup><img src="/images/numberone.png" alt="nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 o 20.000<sup><img src="/images/numberone.png" alt="Nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 o 40.000<sup><img src="/images/numberone.png" alt="Nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="Nota a pie de página" /></sup></td>
          </tr>
</table>


Los volúmenes de rendimiento están diseñados para funcionar constantemente cerca del nivel de IOPS suministrado. La coherencia facilita el dimensionamiento y la escalabilidad de los entornos de aplicaciones con un determinado nivel de rendimiento. Además, es posible optimizar un entorno creando un volumen con la proporción ideal de precio-rendimiento.

### Consideraciones sobre el suministro

**Tamaño de bloque**

Los IOPS, para Resistencia y Rendimiento, se basan en un tamaño de bloque de 16 KB con una carga de trabajo aleatoria del 50 % de lectura/escritura 50/50. Un bloque de 16 KB es el equivalente a una escritura en el volumen.
{:important}

El tamaño de bloque que utiliza la aplicación afecta directamente al rendimiento del almacenamiento. Si el tamaño de bloque utilizado por la aplicación es inferior a 16 KB, se alcanza el límite de IOPS antes que el límite de rendimiento. Por el contrario, si el tamaño de bloque utilizado por la aplicación es superior a 16 KB, se alcanza el límite de rendimiento antes que el límite de IOPS.

<table>
  <caption>En la Tabla 4 se muestran ejemplos de cómo el tamaño de bloque e IOPS afectan al rendimiento.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Tamaño de bloque (KB)</th>
            <th>IOPS</th>
            <th>Rendimiento (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (típico para Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (típico para Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (típico para SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

**Hosts autorizados**

Otro factor a tener en cuenta es el número de hosts que están utilizando el volumen. Si solo hay un host que accede al volumen, puede ser difícil alcanzar el máximo de IOPS, sobre todo en recuentos de IOPS extremos (10.000 s). Si su carga de trabajo requiere un alto rendimiento, será mejor configurar al menos dos servidores para acceder al volumen a fin de evitar un cuello de botella por un servidor.

**Conexión de red**

La velocidad de la conexión de Ethernet debe ser más rápida que el rendimiento máximo previsto de su volumen. Generalmente, no espere que se sature la conexión de Ethernet más allá del 70 % del ancho de banda disponible. Por ejemplo, si tiene 6.000 IOPS y está utilizando un tamaño de bloque de 16 KB, el volumen puede manejar aproximadamente un rendimiento de 94 MBps. Si tiene una conexión de Ethernet de 1 Gbps para el LUN, se genera un cuello de botella cuando los servidores intenten utilizar el máximo rendimiento disponible. Esto se debe a que el 70 % del límite teórico de una conexión de Ethernet de 1 Gbps (125 MB por segundo) solo permitiría 88 MB por segundo.

Para alcanzar el número máximo de IOPS, es necesario disponer de los recursos de red adecuados. Otros aspectos a tener en cuenta son el uso de la red privada fuera del almacenamiento y los ajustes del lado del host y específicos de la aplicación (pila IP o [profundidades de colas](set-host-queue-depth-settings-performance-and-endurance-storage.html) y otros valores).

El tráfico de almacenamiento se incluye en el uso total de la red de los servidores virtuales públicos. Consulte la [documentación de servidor virtual](https://{DomainName}/docs/vsi/vsi_public.html#public-virtual-servers) para conocer los límites que puede imponer el servicio.
{:tip}

## Envío de su pedido

Cuando esté listo para enviar el pedido, siga las instrucciones [aquí](provisioning-block_storage.html).

## Conexión del nuevo almacenamiento

Cuando se haya completado la solicitud de suministro, autorice a los hosts a acceder al nuevo almacenamiento y configure la conexión. En función del sistema operativo del host, siga el enlace adecuado.
- [Conexión a los LUN iSCSI de MPIO en Linux](accessing_block_storage_linux.html)
- [Conexión a los LUN de iSCSI de MPIO en CloudLinux](configure-iscsi-cloudlinux.html)
- [Conexión a los LUN de iSCSI de MPIO en Microsoft Windows](accessing-block-storage-windows.html)
- [Configuración de Block Storage para la copia de seguridad con cPanel](configure-backup-cpanel.html)
- [Configuración de Block Storage para la copia de seguridad con Plesk](configure-backup-plesk.html)
