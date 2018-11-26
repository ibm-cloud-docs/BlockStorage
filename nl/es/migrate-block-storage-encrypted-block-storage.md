---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Actualización de {{site.data.keyword.blockstorageshort}} existente a {{site.data.keyword.blockstorageshort}} mejorado

{{site.data.keyword.blockstoragefull}} mejorado ya está disponible en determinados centros de datos. Para ver la lista de los centros de datos actualizados y las características disponibles, como tasas de IOPS ajustables y volúmenes ampliables, pulse [aquí](new-ibm-block-and-file-storage-location-and-features.html). Para obtener más información sobre el almacenamiento cifrado gestionado por el proveedor, consulte [Cifrado de datos en reposo de {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html).

El método de migración recomendado es conectarse a ambos LUN simultáneamente y transferir datos directamente desde un LUN a otro. Los detalles dependen de su sistema operativo y de si se espera que los datos cambien durante la operación de copia.

Se supone que ya tiene su LUN no cifrado conectado al host. Si no es así, siga las directrices correspondientes a su sistema operativo que mejor se ajusten a esta tarea:

- [Conexión a los LUN iSCSI de MPIO en Linux](accessing_block_storage_linux.html)
- [Conexión a los LUN de iSCSI de MPIO en CloudLinux](configure-iscsi-cloudlinux.html)
- [Conexión a los LUN de iSCSI de MPIO en Microsoft Windows](accessing-block-storage-windows.html)

## Creación de nuevas {{site.data.keyword.blockstorageshort}}

Cuando realice un pedido con API, especifique el paquete "Almacenamiento como un servicio" para asegurarse de recibir las características actualizadas con el nuevo almacenamiento.
{:important}

Las siguientes instrucciones son para solicitar un LUN mejorado a través del {{site.data.keyword.slportal}}. El nuevo LUN debe tener el mismo tamaño o mayor que el volumen original para facilitar la migración.

### Solicitud de un LUN de Resistencia

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura > Almacenamiento > {{site.data.keyword.blockstorageshort}}**.
2. En la esquina superior derecha, pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.
3. Seleccione **Resistencia** en la lista **Seleccionar tipo de almacenamiento**.
4. Seleccione la **Ubicación** (centro de datos) del despliegue.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el volumen anterior.
5. Seleccione la opción de facturación. Puede elegir entre facturación por hora o mensual.
6. Seleccione el nivel IOPS.
7. Pulse **Seleccionar tamaño de almacenamiento** y seleccione el tamaño de almacenamiento en la lista.
8. Pulse **Especificar tamaño de espacio para instantáneas** y seleccione el tamaño de instantánea en la lista. Este espacio se añade al espacio utilizable.
   Para obtener más información sobre las consideraciones y recomendaciones del espacio de instantánea, consulte [Solicitud de instantáneas](ordering-snapshots.html).
   {:tip}
9. Elija el **Tipo de sistema operativo** en la lista.
10. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido.
11. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse **Realizar pedido**.

### Solicitud de un LUN de Rendimiento

1. En el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura > Almacenamiento > {{site.data.keyword.blockstorageshort}}**.
2. En la parte derecha, pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.
3. Seleccione **Rendimiento** en la lista **Seleccionar tipo de almacenamiento**.
4. Pulse **Ubicación** y seleccione el centro de datos.
   - Asegúrese de que el nuevo almacenamiento se añada en la misma ubicación que el host o los hosts pedidos anteriormente.
5. Seleccione la opción de facturación. Puede elegir entre facturación por hora o mensual.
6. Seleccione el **Tamaño de almacenamiento** apropiado.
7. Especifique las IOPS en el campo **Especificar IOPS**.
8. Pulse **Continuar**. Se muestran los cargos mensuales y prorrateados, es una última oportunidad para revisar los detalles del pedido. Pulse **Anterior** si desea cambiar el pedido.
9. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro** y pulse **Realizar pedido**.

El almacenamiento se suministra en menos de un minuto y está visible en la página de {{site.data.keyword.blockstorageshort}} del {{site.data.keyword.slportal}}.



## Conexión del nuevo {{site.data.keyword.blockstorageshort}} al host

Los hosts "autorizados" son hosts a los que se les ha otorgado acceso a un volumen. Sin la autorización del host, no puede acceder ni utilizar el almacenamiento de su sistema. Cuando se autoriza a un host a que acceda a un volumen, se genera el nombre de usuario, la contraseña y el nombre calificado iSCSI (IQN), que se necesitan para montar la conexión iSCSI de E/S de multivía de acceso (MPIO).

1. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**, y pulse el nombre de LUN.
2. Desplácese hasta la sección **Hosts autorizados**.
3. En la parte derecha, pulse **Autorizar host**. Seleccione los hosts que pueden acceder al volumen.


## Instantáneas y réplica

¿Tiene instantáneas y réplicas establecidas para su LUN original? Si es así, debe configurar la réplica, el espacio de instantáneas y crear planificaciones de instantáneas para el nuevo LUN con los mismos valores que el volumen original.

Si su centro de datos de destino de réplica no se ha actualizado aún, no puede establecer la réplica para el nuevo volumen hasta que se actualice el centro de datos.


## Migración de los datos

1. Conéctese a los LUN de {{site.data.keyword.blockstorageshort}} originales y nuevos.
   Si necesita ayuda para conectar los dos LUN a su host, abra un caso de soporte.
   {:tip}

2. Piense en el tipo de datos que tiene en el LUN de {{site.data.keyword.blockstorageshort}} original y decida la mejor forma de copiarlos en el nuevo LUN.
  - Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no hay ninguna preocupación importante.
  - Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.blockstorageshort}}, asegúrese de que los datos no se modifiquen durante la copia para evitar que resulten dañados. Si tiene problemas con el ancho de banda, realice la migración fuera de las horas punta. Si necesita ayuda con estas consideraciones, abra una incidencia de soporte.

3. Copie los datos.
   - **Microsoft Windows**: Para copiar los datos de su LUN de {{site.data.keyword.blockstorageshort}} original en el nuevo LUN, formatee el nuevo almacenamiento y copie los archivos encima utilizando Windows Explorer.
   - **Linux**: Puede utilizar `rsync` para copiar los datos. Aquí tiene un ejemplo:
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   Es una buena idea utilizar el mandato anterior con el distintivo `--dry-run` una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, puede suprimir el último archivo de destino que se ha copiado para asegurarse de que se copie en la nueva ubicación desde el principio.<br/>
   Cuando este mandato finaliza sin el distintivo `--dry-run`, los datos se copian en el nuevo LUN de {{site.data.keyword.blockstorageshort}}. Ejecute el mandato de nuevo para asegurarse de que no hace falta nada. También puede revisar manualmente ambas ubicaciones por si se ha omitido algo.<br/>
   Una vez completada la migración, puede pasar el entorno de producción al nuevo LUN. A continuación, puede desconectar y suprimir el LUN original de la configuración. La supresión también elimina cualquier instantánea o réplica en el sitio de destino que estuviera asociada al LUN original.
