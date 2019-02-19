---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Instantáneas
{: #snapshots}

Las instantáneas son una característica de {{site.data.keyword.blockstoragefull}}. Una instantánea representa el contenido de un volumen en un momento específico. Con las instantáneas, puede proteger los datos sin afectar al rendimiento y con un consumo mínimo de espacio. Las instantáneas son consideradas su primera línea de defensa para la protección de datos. Si un usuario modifica o suprime accidentalmente datos cruciales de un volumen, los datos se pueden restaurar de forma fácil y rápida desde una copia de instantánea.

{{site.data.keyword.blockstorageshort}} le ofrece dos formas de realizar instantáneas.

* La primera, mediante una planificación de instantáneas configurable que crea y suprime copias de instantáneas automáticamente para cada volumen de almacenamiento. También puede crear planificaciones de instantáneas adicionales, suprimir copias manualmente y gestionar las planificaciones basándose en sus requisitos.
* La segunda forma es realizar una instantánea manual.

Una copia de instantánea es una imagen de solo lectura de un LUN de {{site.data.keyword.blockstorageshort}}, que captura el estado del volumen en un momento específico. Las copias de instantánea son eficientes, por el tiempo necesario en crearlas y por el espacio de almacenamiento. Se tarda unos pocos segundos en crear una copia de instantánea de {{site.data.keyword.blockstorageshort}}. Normalmente se tarda menos de 1 segundo, independientemente del tamaño del volumen o del nivel de actividad en el almacenamiento. Una vez creada una copia de instantánea, los cambios en los objetos de datos se reflejan en actualizaciones de la versión actual de los objetos, como si las copias de instantáneas no existieran. Mientras tanto, la copia de los datos permanece estable.

Una copia de instantánea no implica una disminución en el rendimiento. Los usuarios pueden almacenar fácilmente hasta 50 instantáneas planificadas y 50 instantáneas manuales por volumen de {{site.data.keyword.blockstorageshort}}, todas ellas accesibles como solo lectura y versiones en línea de los datos.

Con las instantáneas, puede:

- Crear puntos de recuperación de un punto en el tiempo sin interrupciones.
- Revertir los volúmenes a puntos en el tiempo anteriores.

Debe adquirir cierta cantidad de espacio de instantáneas para su volumen para poder realizar instantáneas. El espacio de instantáneas se puede añadir durante el pedido inicial o posteriormente, a través de la página **Detalles del volumen**. Las instantáneas planificadas y manuales comparten el espacio de instantáneas, por lo tanto, asegúrese de solicitar suficiente espacio de instantáneas. Para más información, consulte [Solicitud de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).

## Prácticas recomendadas en torno a las instantáneas

El diseño de instantáneas depende del entorno del cliente. Las siguientes consideraciones sobre diseño le ayudarán a planificar e implementar copias de instantáneas:
- Pueden crearse hasta 50 instantáneas a través de una planificación y hasta 50 manualmente en cada volumen o LUN.
- No cree más instantáneas de las necesarias. Asegúrese de que la frecuencia de instantáneas planificadas cumpla sus necesidades de objetivo de tiempo de recuperación (RTO) y objetivo de punto de recuperación (RPO), así como los requisitos empresariales de las aplicaciones planificando las instantáneas por hora, a diario o semanalmente.
- La opción de supresión automática de instantáneas puede utilizarse para controlar el crecimiento del consumo de almacenamiento. <br/>

  El umbral de supresión automática está fijado en 95 por ciento.
  {:note}

Las instantáneas no son sustituciones de la réplica de recuperación tras desastre externa real ni de las copias de seguridad de larga retención.
{:important}

## Seguridad

Todas las instantáneas y réplicas de {{site.data.keyword.blockstorageshort}} cifrados también se cifran de forma predeterminada. Esta característica no se puede desactivar por volumen. Para obtener más información sobre el cifrado en reposo gestionado por el proveedor, consulte [Protección de los datos](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

## Cómo afectan las instantáneas al espacio de disco

Las copias de instantáneas minimizan el consumo de disco conservando bloques individuales en lugar de archivos completos. Las copias de instantáneas solo utilizan espacio adicional cuando se cambian o suprimen archivos en el sistema de archivos activo.

En el sistema de archivos activo, los bloques modificados se vuelven a escribir en diferentes ubicaciones del disco o se eliminan como bloques de archivos activos por completo. Cuando los archivos se modifican o se suprimen, los bloques de archivos originales se conservan como parte de una o más copias de instantáneas. Como resultado, aún se reserva el espacio de disco utilizado por los bloques originales para reflejar el estado del sistema de archivos activo antes del cambio. Este espacio se reserva además del espacio de disco utilizado por los bloques en el sistema de archivos activo modificado.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Uso de espacio de disco antes y después de una copia de instantánea</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Antes de una copia de instantánea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Después de una copia de instantánea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Cambios después de una copia de instantánea"></td>
     </tr><tr>
 <td style="border: 0.0px;">Antes de crear una copia de instantánea, solo el sistema de archivos activo utiliza el espacio de disco.</td>
        <td style="border: 0.0px;">Después de crear una copia de instantánea, el sistema de archivos activo y la copia de instantánea apuntan a los mismos bloques de discos. La copia de instantánea no utiliza espacio de disco adicional.</td>
        <td style="border: 0.0px;">Tras suprimir <i>myfile.txt</i> del sistema de archivos activo, la copia de instantánea aún incluye el archivo y hace referencia a sus bloques de discos. Por este motivo, el hecho de suprimir los datos del sistema de archivos activo no siempre libera espacio de disco.</td>
      </tr>
</table>

Para obtener más información sobre el uso de espacio por parte de las instantáneas, consulte [Gestión de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots).
