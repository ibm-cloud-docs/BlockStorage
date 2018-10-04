---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-26"

---
{:new_window: target="_blank"}

# Solicitud de {{site.data.keyword.blockstorageshort}}

Puede suministrar Almacenamiento y mayor precisión de {{site.data.keyword.blockstorageshort}} para satisfacer las necesidades de su capacidad e IOPS. Saque el mayor partido de su almacenamiento con dos opciones para especificar el rendimiento.

- Puede elegir entre los niveles de IOPS de Resistencia que incluyen los niveles de rendimiento predefinidos para que se ajusten las cargas de trabajo que no han definido bien los requisitos de rendimiento. 
- Puede ajustar el almacenamiento para que cumpla los requisitos de rendimiento muy específicos especificando el número total de IOPS con Rendimiento.

## Pedido de {{site.data.keyword.blockstorageshort}} con los niveles de IOPS predefinidos (Resistencia)

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura > Almacenamiento > {{site.data.keyword.blockstorageshort}}**.
2. En la parte superior derecha, pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.
3. Seleccione la **Ubicación** (centro de datos) del despliegue.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host o los hosts de cálculo que tiene.
4. Facturación. Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un asterisco), podrá elegir entre facturación mensual o por horas. 
     1. Con la facturación **por hora**, el número de horas que el LUN de bloque existía en la cuenta se calcula en el momento en que se suprime el LUN o al final del ciclo de facturación. Lo que se produzca primero. La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de bloque de archivos se suprime antes de finalizar el ciclo de facturación. La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).
        >**NOTA**: El tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que **no** están actualizados con prestaciones mejoradas.
5. Especifique el tamaño de almacenamiento en el campo **Nuevo tamaño de almacenamiento**.
6. Seleccione **Resistencia (IOPS en niveles)** en la sección **Opciones de IOPS de almacenamiento**.
7. Seleccione el nivel de IOPS que necesita la aplicación.
    - **0,25 IOPS por GB** está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en un momento. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.
    - **2 IOPS por GB** está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.
    - **4 IOPS por GB** está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en un momento. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.
    - **10 IOPS por GB** está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL y el proceso de datos para Analytics. Este nivel está disponible para almacenamiento suministrado de hasta 4 TB en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).
8. Pulse **Especificar tamaño de espacio para instantáneas** y seleccione el tamaño de instantánea en la lista. Este espacio se añade al espacio utilizable. Para consultar consideraciones y recomendaciones sobre el espacio de instantáneas, consulte [Realizar pedidos de instantáneas](ordering-snapshots.html).
9. Elija el **Tipo de sistema operativo** en la lista.
10. Marque los recuadros de selección de **Términos y condiciones** y pulse **Realizar pedido**.
11. La nueva asignación de almacenamiento está disponible en pocos minutos.

>**Nota**: De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con el representante de ventas. [Aquí](managing-storage-limits.html) puede leer más información sobre cómo aumentar los límites.<br/><br/>Para obtener información sobre el límite en autorizaciones simultáneas, consulte las [Preguntas más frecuentes](BlockStorageFAQ.html).
 
## Pedido de {{site.data.keyword.blockstorageshort}} con IOPS personalizados (Rendimiento)

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura > Almacenamiento > {{site.data.keyword.blockstorageshort}}**.
2. En la parte superior derecha, pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.
3. Pulse **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host o los hosts de cálculo que tiene.
4. Facturación. Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un asterisco), podrá elegir entre facturación mensual o por horas.
     1. Con la facturación **por hora**, el número de horas que el LUN de bloque existía en la cuenta se calcula en el momento en que se suprime el LUN o al final del ciclo de facturación. Lo que se produzca primero. La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de bloque de archivos se suprime antes de finalizar el ciclo de facturación. La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).
        >**NOTA**: El tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que **no** están actualizados con prestaciones mejoradas.
5. Especifique el tamaño de almacenamiento en el campo **Nuevo tamaño de almacenamiento**.
6. Seleccione **Rendimiento (IOPS asignados)** en la sección **Opciones de IOPS de almacenamiento**.
7. Especifique las IOPS en el campo **Rendimiento (IOPS asignado)**.
8. Pulse **Especificar tamaño de espacio para instantáneas** y seleccione el tamaño de instantánea en la lista. Este espacio se añade al espacio utilizable. Para consultar consideraciones y recomendaciones sobre el espacio de instantáneas, consulte [Realizar pedidos de instantáneas](ordering-snapshots.html).
9. Elija el **Tipo de sistema operativo** en la lista.
10. La nueva asignación de almacenamiento está disponible en pocos minutos.

>**Nota**: De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con el representante de ventas. [Aquí](managing-storage-limits.html) puede leer más información sobre cómo aumentar los límites.<br/><br/>Para obtener información sobre el límite en autorizaciones simultáneas, consulte las [Preguntas más frecuentes](BlockStorageFAQ.html).

## Conexión del nuevo almacenamiento

Cuando se haya completado la solicitud de suministro, autorice a los hosts a acceder al nuevo almacenamiento y configure la conexión. En función del sistema operativo del host, siga el enlace adecuado.
- [Conexión a los LUN iSCSI de MPIO en Linux](accessing_block_storage_linux.html)
- [Conexión a los LUN de iSCSI de MPIO en CloudLinux](configure-iscsi-cloudlinux.html)
- [Conexión a los LUN de iSCSI de MPIO en Microsoft Windows](accessing-block-storage-windows.html)
- [Configuración de Block Storage para la copia de seguridad con cPanel](configure-backup-cpanel.html)
- [Configuración de Block Storage para la copia de seguridad con Plesk](configure-backup-plesk.html)

## Identificación de {{site.data.keyword.blockstorageshort}} en la factura

Todos los LUN aparecen en la factura como un elemento de línea. La Resistencia aparece como “Servicio de almacenamiento de resistencia” y Rendimiento aparece como "Servicio de almacenamiento de rendimiento". La tarifa varía en función de su nivel de almacenamiento. Puede ampliar la Resistencia o el Rendimiento para ver que es {{site.data.keyword.blockstorageshort}}.
