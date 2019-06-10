---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Solicitud de {{site.data.keyword.blockstorageshort}} mediante la consola
{: #orderingthroughConsole}

Puede suministrar {{site.data.keyword.blockstorageshort}} y realizar ajustes para satisfacer sus necesidades de capacidad y de IOPS. Saque el mayor partido de su almacenamiento con dos opciones para especificar el rendimiento.

- Puede elegir entre los niveles de IOPS de Resistencia que incluyen los niveles de rendimiento predefinidos para que se ajusten las cargas de trabajo que no han definido bien los requisitos de rendimiento.
- Puede ajustar el almacenamiento para que cumpla los requisitos de rendimiento específicos especificando el número total de IOPS con Rendimiento.

## Solicitud de {{site.data.keyword.blockstorageshort}} con los niveles de IOPS predefinidos (Resistencia)

1. Inicie la sesión en el [catálogo de IBM Cloud](https://{DomainName}/catalog){: external} y pulse **Almacenamiento**. A continuación, seleccione **{{site.data.keyword.blockstorageshort}}** y pulse **Crear**.

   También puede iniciar la sesión en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**. En la parte superior derecha, pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.

2. Seleccione la **Ubicación** (centro de datos) del despliegue.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host o los hosts de cálculo que tiene.
3. Facturación. Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un asterisco), podrá elegir entre facturación mensual o por horas.
     1. Con la facturación **por hora**, el número de horas que el LUN de bloque existía en la cuenta se calcula en el momento en que se suprime el LUN o al final del ciclo de facturación. Lo que se produzca primero. La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de bloque de archivos se suprime antes de finalizar el ciclo de facturación. La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).

        El tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que **no** están actualizados con prestaciones mejoradas.
        {:important}
4. Especifique el tamaño de almacenamiento en el campo **Nuevo tamaño de almacenamiento**.
5. Seleccione **Resistencia (IOPS en niveles)** en la sección **Opciones de IOPS de almacenamiento**.
6. Seleccione el nivel de IOPS que necesita la aplicación.
    - **0,25 IOPS por GB** está diseñado para cargas de trabajo con intensidad baja de E/S. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje elevado de datos inactivos en un momento. Aplicaciones de ejemplo incluyen el almacenamiento de buzones o el uso compartido de archivos a nivel departamental.
    - **2 IOPS por GB** está diseñado para usos de finalidad más general. Entre las aplicaciones de ejemplo, se incluye el alojamiento de bases de datos pequeñas que respaldan aplicaciones web o imágenes de disco de máquinas virtuales para un hipervisor.
    - **4 IOPS por GB** está diseñado para cargas de trabajo de mayor intensidad. Estas cargas de trabajo se suelen caracterizar por tener un porcentaje alto de datos activos en un momento. Entre las aplicaciones de ejemplo, se incluyen las bases de datos transaccionales y otras bases de datos que dependen del rendimiento.
    - **10 IOPS por GB** está diseñado para las cargas de trabajo más exigentes, como las creadas por bases de datos NoSQL y el proceso de datos para Analytics. Este nivel está disponible para almacenamiento suministrado de hasta 4 TB en [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
7. Pulse **Especificar tamaño de espacio para instantáneas** y seleccione el tamaño de instantánea en la lista. Este espacio se añade al espacio utilizable. Para consultar consideraciones y recomendaciones sobre el espacio de instantáneas, consulte [Solicitud de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Elija el **Tipo de sistema operativo** en la lista.<br/>

   Esta selección depende del sistema operativo en el que se ejecuta el host y no se puede modificar posteriormente. Por ejemplo, si su servidor es Ubuntu o RHEL, seleccione Linux. Si su host es un servidor Windows 2012 o Windows 2016, seleccione la opción Windows 2008+ en la lista. Para obtener más información sobre las distintas opciones de Windows, consulte [Preguntas más frecuentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
   {:tip}
9. En la parte derecha, revise el resumen del pedido y aplique su código de promoción si lo tiene.

   Los descuentos se aplican cuando se procesa el pedido.
   {:note}
10. Después de revisar los términos y condiciones, marque el recuadro de selección **He leído y acepto los acuerdos de servicio de terceros**.
11. Pulse **Crear**. La nueva asignación de almacenamiento está disponible en pocos minutos.

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con el representante de ventas. [Aquí](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits) puede leer más información sobre cómo aumentar los límites.<br/><br/>Para obtener información sobre el límite en autorizaciones simultáneas, consulte las [Preguntas más frecuentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}

## Solicitud de {{site.data.keyword.blockstorageshort}} con IOPS personalizados (Rendimiento)

1. Inicie la sesión en el [catálogo de IBM Cloud](https://{DomainName}/catalog){: external} y pulse **Almacenamiento**. A continuación, seleccione {{site.data.keyword.blockstorageshort}} y pulse **Crear**.

   También puede iniciar la sesión en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**. En la parte superior derecha, pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.
2. Pulse **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host o los hosts de cálculo que tiene.
3. Facturación. Si ha seleccionado un centro de datos con prestaciones mejoradas (marcados con un asterisco), podrá elegir entre facturación mensual o por horas.
     1. Con la facturación **por hora**, el número de horas que el LUN de bloque existía en la cuenta se calcula en el momento en que se suprime el LUN o al final del ciclo de facturación. Lo que se produzca primero. La facturación por horas es una buena opción para el almacenamiento que se utiliza unos pocos días o menos de un mes completo. La facturación por horas solo está disponible para el almacenamiento suministrado en estos [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. Con la **facturación mensual**, el cálculo del precio se prorratea desde la fecha de creación hasta la finalización del ciclo de facturación y se factura al momento. No se reembolsará si un LUN de bloque de archivos se suprime antes de finalizar el ciclo de facturación. La facturación mensual es una buena opción para el almacenamiento utilizado en cargas de trabajo de producción que utilizan datos que tienen que almacenarse, y por tanto acceder a ellos, durante largo periodos de tiempo (un mes o más).

        El tipo de facturación mensual se utiliza de forma predeterminada para el almacenamiento suministrado en centros de datos que **no** están actualizados con prestaciones mejoradas.
        {:note}
4. Especifique el tamaño de almacenamiento en el campo **Nuevo tamaño de almacenamiento**.
5. Seleccione **Rendimiento (IOPS asignados)** en la sección **Opciones de IOPS de almacenamiento**.
6. Especifique las IOPS en el campo **Rendimiento (IOPS asignado)**.
7. Pulse **Especificar tamaño de espacio para instantáneas** y seleccione el tamaño de instantánea en la lista. Este espacio se añade al espacio utilizable. Para consultar consideraciones y recomendaciones sobre el espacio de instantáneas, consulte [Solicitud de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Elija el **Tipo de sistema operativo** en la lista.<br/>

   Esta selección depende del sistema operativo en el que se ejecuta el host y no se puede modificar posteriormente. Por ejemplo, si su servidor es Ubuntu o RHEL, seleccione Linux. Si su host es un servidor Windows 2012 o Windows 2016, seleccione la opción Windows 2008+ en la lista. Para obtener más información sobre las distintas opciones de Windows, consulte [Preguntas más frecuentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
   {:tip}
9. En la parte derecha, revise el resumen del pedido y aplique su código de promoción si lo tiene.

   Los descuentos se aplican cuando se procesa el pedido.
   {:note}
10. Después de revisar los términos y condiciones, marque el recuadro de selección **He leído y acepto los acuerdos de servicio de terceros**.
11. Pulse **Crear**. La nueva asignación de almacenamiento está disponible en pocos minutos.

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con el representante de ventas. [Aquí](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits) puede leer más información sobre cómo aumentar los límites.<br/><br/>Para obtener información sobre el límite en autorizaciones simultáneas, consulte las [Preguntas más frecuentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}

## Conexión del nuevo almacenamiento
{: #mountingnewLUN}

Cuando se haya completado la solicitud de suministro, autorice a los hosts a acceder al nuevo almacenamiento y configure la conexión. En función del sistema operativo del host, siga el enlace adecuado.
- [Conexión a LUN en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conexión a LUN en CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conexión a LUN en Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Montaje de un LUN en almacenamiento compartido XenServer](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Configuración de almacenamiento en bloque para la copia de seguridad con cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuración de almacenamiento en bloque para la copia de seguridad con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Consideraciones sobre la recuperación tras desastre

Para evitar la pérdida de datos y garantizar la continuidad del negocio, considere la posibilidad de replicar los servidores y el almacenamiento en otro centro de datos. La réplica mantiene los datos en sincronización en dos ubicaciones distintas en función de la planificación de la instantánea. Para obtener más información, consulte [Réplica de datos](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).

Si desea clonar su volumen y utilizarlo independientemente del volumen original, consulte [Creación de un volumen de bloque duplicado](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Identificación de {{site.data.keyword.blockstorageshort}} en la factura

Todos los LUN aparecen en la factura como un elemento de línea. La Resistencia aparece como “Servicio de almacenamiento de resistencia” y Rendimiento aparece como "Servicio de almacenamiento de rendimiento". La tarifa varía en función de su nivel de almacenamiento. Puede ampliar la Resistencia o el Rendimiento para ver que es {{site.data.keyword.blockstorageshort}}.
