---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Solicitud de {{site.data.keyword.blockstorageshort}}

Existen dos tipos distintos de almacenamiento que puede suministrar en función de sus necesidades y preferencias. Las dos opciones de volúmenes de {{site.data.keyword.blockstorageshort}} son: 

- **Resistencia**: suministro de niveles de resistencia que presentan niveles de rendimiento predefinidos y características como instantáneas y réplica. 
- **Rendimiento**: crear un entorno de rendimiento de alta potencia donde puede asignar las operaciones de entrada/salida deseadas por segundo (IOPS).

## Cómo solicitar Resistencia para {{site.data.keyword.blockstorageshort}}

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura > Almacenamiento > {{site.data.keyword.blockstorageshort}}**.
2. En la esquina superior derecha, pulse el enlace **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.
3. Seleccione **Resistencia** en la lista desplegable Seleccionar tipo de almacenamiento.
4. Pulse la lista desplegable y seleccione la **Ubicación** del despliegue (centro de datos).
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host pedido anteriormente.
   - Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un * en el desplegable), tendrá la opción de elegir entre facturación mensual o por horas. 
     1. Con la **facturación por horas**, el cálculo del número de horas que el LUN de bloque ha existido en la cuenta se realiza en el momento en que se suprime el LUN o al final del ciclo de facturación, lo que se produzca primero.  La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de bloque de archivos se suprime antes de finalizar el ciclo de facturación.  La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).
     **NOTA**: el tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que **no** están actualizados con prestaciones mejoradas.
5. Marque el botón de selección situado junto al nivel de IOPS necesario para la aplicación.
    - *0,25 IOPS por GB* está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en un momento determinado. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.
    - *2 IOPS por GB* está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.
    - *4 IOPS por GB* está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en un momento determinado. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.
    - *10 IOPS por GB* está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL, y el procesamiento de datos para Analytics.  Este nivel está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html) para el almacenamiento suministrado de hasta 4 TB de tamaño.
6. Pulse la lista desplegable **Seleccionar tamaño de almacenamiento** y seleccione el tamaño del almacenamiento.
7. Pulse la lista desplegable **Especificar tamaño de espacio de instantáneas** y seleccione el tamaño de instantánea (además del espacio utilizable). Para consultar consideraciones y recomendaciones sobre el espacio de instantáneas, consulte [Realizar pedido de instantáneas](ordering-snapshots.html).
8. Elija el **Tipo de sistema operativo** en la lista desplegable.
9. Pulse la lista desplegable y seleccione la Ubicación del despliegue (centro de datos).
10. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido.
11. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse el botón **Realizar pedido**.
12. La nueva asignación de almacenamiento debería estar disponible en unos minutos.

**Nota**: de forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con su representante de ventas. [Aquí](managing-storage-limits.html) puede leer más información sobre cómo aumentar los límites.

Para información sobre el límite en autorizaciones simultáneas, consulte nuestras [Preguntas más frecuentes](BlockStorageFAQ.html)
 
## Cómo solicitar Rendimiento para Block Storage

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura > Almacenamiento > {{site.data.keyword.blockstorageshort}}**.
2. Pulse **Realizar el pedido de {{site.data.keyword.blockstorageshort}}** en la esquina superior derecha de la pantalla.
3. Seleccione **Rendimiento** en la lista desplegable **Seleccionar tipo de almacenamiento**.
4. Pulse la lista desplegable **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host pedido anteriormente.
   - Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un * en el desplegable), tendrá la opción de elegir entre facturación mensual o por horas. 
     1. Con la **facturación por horas**, el cálculo del número de horas que el LUN de bloque ha existido en la cuenta se realiza en el momento en que se suprime el LUN o al final del ciclo de facturación, lo que se produzca primero.  La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de bloque de archivos se suprime antes de finalizar el ciclo de facturación.  La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).
     **NOTA**: el tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que **no** están actualizados con prestaciones mejoradas.
5. Marque el botón de selección situado junto al **Tamaño de almacenamiento** adecuado.
6. Especifique las IOPS en el campo **Especificar IOPS**.
7. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
8. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse el botón **Realizar pedido**.
9. La nueva asignación de almacenamiento debería estar disponible en unos minutos.

**Nota**: de forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con su representante de ventas. [Aquí](managing-storage-limits.html) puede leer más información sobre cómo aumentar los límites.

Para información sobre el límite en autorizaciones simultáneas, consulte nuestras [Preguntas más frecuentes](BlockStorageFAQ.html)

## Cómo identificar {{site.data.keyword.blockstorageshort}} en la factura

Todos los LUN aparecerán en la factura como un elemento de línea; Resistencia aparecerá como "Servicio de Almacenamiento de Resistencia" y Rendimiento aparecerá como "servicio de Almacenamiento de Rendimiento". La tarifa variará en función de su nivel de almacenamiento. Entonces, puede ampliar el Rendimiento o la Resistencia para ver que es {{site.data.keyword.blockstorageshort}}.
