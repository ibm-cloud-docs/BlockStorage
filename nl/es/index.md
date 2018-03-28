---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Acerca de {{site.data.keyword.blockstorageshort}}

{{site.data.keyword.blockstoragefull}} es almacenamiento iSCSI persistente y de alto rendimiento, que se suministra y gestiona independiente de las instancias de cálculo. Los LUN de {{site.data.keyword.blockstorageshort}} basados en iSCSI se conectan a dispositivos autorizados a través de conexiones de E/S de multivía de acceso (MPIO) redundantes.

{{site.data.keyword.blockstorageshort}} aporta los mejores niveles de durabilidad y disponibilidad con un conjunto de características único, utilizando estándares del sector y las mejores prácticas, y se ha diseñado para proteger la integridad de los datos y mantener la disponibilidad en sucesos de mantenimiento y fallos imprevistos, así como proporcionar una línea base de rendimiento coherente.

## Características principales

Aproveche las siguientes características de {{site.data.keyword.blockstorageshort}}:

- **Línea base de rendimiento coherente**
   - Se proporciona mediante la asignación de IOPS a nivel de protocolo a volúmenes individuales.
- **Muy duradero y resistente**
   - Protege la integridad de los datos y mantiene la disponibilidad en sucesos de mantenimiento y fallos imprevistos sin la necesidad de crear y gestionar una matriz redundante a nivel de sistema operativo de matrices de discos independientes (RAID)
- **Cifrado de datos en reposo** ([disponible en centros de datos seleccionados)](new-ibm-block-and-file-storage-location-and-features.html)
   - Cifrado gestionado por el proveedor de datos en reposo sin ningún coste adicional
- **Almacenamiento All Flash respaldado** ([disponible en centros de datos seleccionados)](new-ibm-block-and-file-storage-location-and-features.html)
   - Almacenamiento all flash para volúmenes suministrados con Resistencia o Rendimiento a 2 IOPS/GB o superior
- **Instantáneas** (si se suministran con Resistencia o Rendimiento en [centros de datos seleccionados)](new-ibm-block-and-file-storage-location-and-features.html).
   - Captura instantáneas de datos en un momento específico sin interrumpir
- **Réplica** (si se suministra con Resistencia o Rendimiento en [centros de datos seleccionados)](/new-ibm-block-and-file-storage-location-and-features.html).
   - Copia automáticamente instantáneas a un centro de datos de {{site.data.keyword.BluSoftlayer_full}} asociado
- **Conectividad de alta disponibilidad**
   - Utiliza conexiones de red redundantes para maximizar la disponibilidad - {{site.data.keyword.blockstorageshort}} basado en iSCSI utiliza E/S de multivía de acceso (MPIO)
- **Acceso simultáneo**
   - Permite a varios hosts acceder a volúmenes de bloques simultáneamente (hasta 8) para configuraciones en clúster.
- **Bases de datos en clúster**
   - Da soporte a casos de uso avanzados, como bases de datos en clúster
     
## Facturación por horas o mensual

Puede elegir entre facturación por horas o mensual para un LUN de bloque. El tipo de facturación seleccionado para un LUN será aplicable a su espacio de instantáneas y réplicas. Por ejemplo, si suministra un LUN con facturación por horas, todas las tasas de instantáneas o réplicas se facturarán por horas. Si suministra un LUN con facturación mensual, todas las tasas de instantáneas o réplicas se facturarán mensualmente. 

Con la **facturación por horas**, el cálculo del número de horas que el LUN de bloque ha existido en la cuenta se realiza en el momento en que se suprime el LUN o al final del ciclo de facturación, lo que se produzca primero.  La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 

Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de archivos se suprime antes de finalizar el ciclo de facturación.  La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más). 

### Rendimiento:
<table>
 <tbody>
  <tr>
   <th>Precio mensual</th>
   <td>0,10 USD/GB + 0,07 USD/IOPS</td>
  </tr>
  <tr>
   <th>Precio por hora</th>
   <td>0,0001 USD/GB + 0,0002 USD/IOPS</td>
  </tr>
  </tbody>
</table>
 
### Resistencia:
<table>
 <tbody>
  <tr>
   <th>Nivel de IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Precio mensual</th>
   <td>0,10 USD/GB</td>
   <td>0,20 USD/GB</td>
   <td>0,35 USD/GB</td>
   <td>0,58 USD/GB</td>
  </tr>
  <tr>
   <th>Precio por hora</th>
   <td>0,0002 USD/GB</td>
   <td>0,0003 USD/GB</td>
   <td>0,0005 USD/GB</td>
   <td>0,0009 USD/GB</td>
  </tr>
  </tbody>
</table>



## Suministro

Los LUN de {{site.data.keyword.blockstorageshort}} se pueden suministrar de 20 GB a 12 TB con dos opciones para el suministro: <br/>
- Suministro de niveles de **Resistencia** que presentan niveles de rendimiento predefinidos y características como instantáneas y réplica.
- Crear un entorno de **Rendimiento** de alta potencia con operaciones de entrada/salida asignadas por segundo (IOPS). 

### Niveles de resistencia

La resistencia está disponible en tres niveles de rendimiento de IOPS para dar soporte a las distintas necesidades de las aplicaciones. <br />

- **0,25 IOPS por GB** está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en un momento determinado. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.
- **2 IOPS por GB** está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.
- **4 IOPS por GB** está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en un momento determinado. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.
- **10 IOPS por GB** está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL, y el procesamiento de datos para Analytics.  Este nivel está disponible para almacenamiento suministrado de hasta 4 TB de tamaño en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).

Hay disponibles hasta 48.000 IOPS con el volumen de Resistencia de 12 TB.
 

Aunque elegir el nivel adecuado de {{site.data.keyword.blockstorageshort}} de Resistencia para su carga de trabajo es clave, es igualmente importante utilizar el tamaño de bloque, la velocidad de conexión de Ethernet y el número de hosts necesarios para alcanzar el máximo rendimiento. Si alguno de estos componentes no se alinea con los demás, puede afectar negativamente sobre el rendimiento final.

 
### Rendimiento

El Rendimiento es una clase de {{site.data.keyword.blockstorageshort}} diseñada para dar soporte a aplicaciones de alto nivel de entrada/salida con requisitos de rendimiento bien definidos, que no encajan en un nivel de Resistencia. El rendimiento previsible se consigue mediante la asignación de IOPS a nivel de protocolo a volúmenes individuales. Los IOPS que van de 100 a 48.000 se pueden suministrar con tamaños de almacenamiento que oscilan entre los 20 GB y los 12 TB. 

Se requiere una conexión de interfaz para pequeños sistemas (iSCSI) de E/S de multivía de acceso (MPIO) para acceder y montar el rendimiento para {{site.data.keyword.blockstorageshort}}. {{site.data.keyword.blockstorageshort}} se suele utilizar cuando una única máquina va a acceder al volumen. Se pueden montar varios volúmenes en un host y juntarse para conseguir volúmenes más grandes y recuentos de IOPS más altos. Se pueden realizar pedidos de volúmenes de rendimiento de acuerdo con los tamaños e IOPS indicados en la Tabla 1 para sistemas operativos Linux, XEN, VMware y Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
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
            <td>6.000 o 20.000<sup><img src="/images/numberone.png" alt="nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>2.000-3.000</td>
            <td>200</td>
            <td>6.000 o 40.000<sup><img src="/images/numberone.png" alt="nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>4.000-7.000</td>
            <td>300</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>8.000-9.000</td>
            <td>500</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="nota a pie de página" /></sup></td>
          </tr>
          <tr>
            <td>10.000-12.000</td>
            <td>1.000</td>
            <td>6.000 o 48.000<sup><img src="/images/numberone.png" alt="nota a pie de página" /></sup></td>
          </tr>
        </tbody>
</table>

<sup>![nota a pie de página](/images/numberone.png)</sup> El límite de IOPS superior a 6.000 está [disponible en centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).


Los volúmenes de rendimiento están diseñados para ejecutarse constantemente cerca del nivel de IOPS suministrado. La consistencia facilita el dimensionamiento y la escalabilidad de los entornos de aplicaciones con un determinado nivel de rendimiento. Además, dado el rango de tamaños de volúmenes y recuentos de IOPS, es posible optimizar un entorno creando un volumen con la proporción ideal de precio-rendimiento.

### Consejos para suministrar IOPS para {{site.data.keyword.blockstorageshort}}

Los IOPS, para Resistencia y Rendimiento, se basan en un tamaño de bloque de 16 KB con una carga de trabajo aleatoria del 50% de lectura/escritura 50/50. Un bloque de 16 KB es el equivalente a una escritura en el volumen.

El tamaño de bloque utilizado por la aplicación afectará directamente al rendimiento.  Si el tamaño de bloque utilizado por la aplicación es inferior a 16 KB, el límite de IOPS se alcanzará antes que el límite de rendimiento.  Por el contrario, si el tamaño de bloque utilizado por la aplicación es superior a 16 KB, el límite de rendimiento se alcanzará antes que el límite de IOPS.

Cambiar el tamaño de bloque afectará al rendimiento de la siguiente manera:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Tamaño de bloque (KB)</th>
            <th>IOPS</th>
            <th>Rendimiento (MB/s)</th>
          </tr>
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
            <td>32 (típico para SQLServer)</td>
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

Elegir el {{site.data.keyword.blockstorageshort}} adecuado para su carga de trabajo es importante, pero es igualmente importante determinar cómo evitar cuellos de botella. La velocidad de la conexión de Ethernet debe ser más rápida que el rendimiento máximo previsto de su volumen. Como regla general, no debe esperar que se sature la conexión de Ethernet más allá del 70% del ancho de banda disponible. Por ejemplo, si tiene 6.000 IOPS y está utilizando un tamaño de bloque de 16 KB, el volumen es capaz de alcanzar aproximadamente 94 MB por segundo. Si tiene una conexión de Ethernet de 1 Gbps para el LUN, se generará un cuello de botella cuando los servidores intenten utilizar el máximo rendimiento disponible, porque el 70% del límite teórico de una conexión de Ethernet de 1 Gbps (125 MB por segundo) solo permitiría 88 MB por segundo.


Otro factor a tener en cuenta es el número de hosts que están utilizando el volumen. Si hay un solo host accediendo al volumen, puede ser difícil alcanzar el máximo de IOPS, sobre todo en recuentos de IOPS extremos (10.000s). Si su carga de trabajo requiere un alto rendimiento, será mejor configurar al menos dos o tres servidores para acceder al volumen a fin de evitar un cuello de botella por un servidor.


Para alcanzar el número máximo de IOPS, es necesario disponer de los recursos de red adecuados. Otros aspectos a tener en cuenta son el uso de la red privada fuera del almacenamiento y los ajustes del lado del host y específicos de la aplicación (pila IP, profundidades de colas, etc.).
