---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-18"

keywords: Block Storage, migrate to new Block Storage, how to encrypt existing Block Storage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Actualización de {{site.data.keyword.blockstorageshort}} existente a {{site.data.keyword.blockstorageshort}} mejorado
{: #migratestorage}

Ahora {{site.data.keyword.blockstoragefull}} mejorado está disponible en la mayoría de los [centros de datos](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

El método de migración recomendado es conectarse a ambos LUN simultáneamente y transferir datos directamente desde un LUN a otro. Los detalles dependen de su sistema operativo y de si se espera que los datos cambien durante la operación de copia.

El supuesto es que ya tiene su LUN no cifrado conectado al host. Si no es así, siga las directrices correspondientes a su sistema operativo que mejor se ajusten a esta tarea:

- [Conexión a LUN en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conexión a LUN en CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conexión a LUN en Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Todos los volúmenes de {{site.data.keyword.blockstorageshort}} mejorados suministrados en estos centros de datos tienen un punto de montaje distinto que los volúmenes no cifrados. Para asegurarse de que utiliza el punto de montaje correcto para los volúmenes de almacenamiento, puede consultar la información sobre el punto de montaje en la página **Detalles del volumen** en la consola. También puede acceder al punto de montaje correcto mediante una llamada de API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

## Creación de un {{site.data.keyword.blockstorageshort}}

Cuando realice un pedido con API, especifique el paquete "Almacenamiento como un servicio" para asegurarse de recibir las características actualizadas con el nuevo almacenamiento.
{:important}

Puede solicitar una LUN mejorada desde la consola de IBM Cloud. El nuevo LUN debe tener el mismo tamaño o mayor que el volumen original para facilitar la migración.

- [Solicitud de {{site.data.keyword.blockstorageshort}} con los niveles de IOPS predefinidos (Resistencia)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsoleEndurance)
- [Solicitud de {{site.data.keyword.blockstorageshort}} con IOPS personalizados (Rendimiento)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#orderingthroughConsolePerformance)

Su nuevo almacenamiento está preparado para que se monte en pocos minutos. Puede verlo en la lista de recursos y en la lista de {{site.data.keyword.blockstorageshort}}.

## Conexión del nuevo {{site.data.keyword.blockstorageshort}} al host

Los hosts "autorizados" son hosts a los que se les ha otorgado acceso a un volumen. Sin la autorización del host, no puede acceder ni utilizar el almacenamiento de su sistema. Cuando se autoriza a un host a que acceda a un volumen, se genera el nombre de usuario, la contraseña y el nombre calificado iSCSI (IQN), que se necesitan para montar la conexión iSCSI de E/S de multivía de acceso (MPIO).

1. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**, y pulse el nombre de LUN.
2. Desplácese hasta la sección **Hosts autorizados**.
3. En la parte derecha, pulse **Autorizar host**. Seleccione los hosts que pueden acceder al volumen.


## Instantáneas y réplica

¿Tiene instantáneas y réplicas establecidas para su LUN original? Si es así, debe configurar la réplica, el espacio de instantáneas y crear planificaciones de instantáneas para el nuevo LUN con los mismos valores que el volumen original.

Si su centro de datos de destino de réplica no se ha actualizado aún, no puede establecer la réplica para el nuevo volumen hasta que se actualice el centro de datos.


## Migración de los datos

1. Conéctese a las LUN de {{site.data.keyword.blockstorageshort}} originales y nuevas.

   Si necesita ayuda para conectar los dos LUN a su host, abra un caso de soporte.
   {:tip}

2. Piense en el tipo de datos que tiene en la LUN de {{site.data.keyword.blockstorageshort}} original y decida la mejor forma de copiarlos en el nuevo LUN.
  - Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no debe preocuparse demasiado.
  - Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.blockstorageshort}}, asegúrese de que los datos no se modifiquen durante la copia para evitar que resulten dañados.
  - Si tiene problemas con el ancho de banda, realice la migración fuera de las horas punta.
  - Si necesita ayuda con estas consideraciones, abra un caso de soporte.

3. Copie los datos.
   - Para **Microsoft Windows**, formatee el nuevo almacenamiento y copie los datos de la LUN de {{site.data.keyword.blockstorageshort}} original en la nueva LUN mediante Windows Explorer.
   - Para **Linux**, puede utilizar `rsync` para copiar los datos.
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   Es una buena idea utilizar el mandato anterior con el distintivo `--dry-run` una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, puede suprimir el último archivo de destino que se ha copiado para asegurarse de que se copie en la nueva ubicación desde el principio.<br/>
   Cuando este mandato finaliza sin el distintivo `--dry-run`, los datos se copian en el nuevo LUN de {{site.data.keyword.blockstorageshort}}. Ejecute el mandato de nuevo para asegurarse de que no hace falta nada. También puede revisar manualmente ambas ubicaciones por si se ha omitido algo.<br/>
   Una vez completada la migración, puede pasar el entorno de producción al nuevo LUN. A continuación, puede desconectar y suprimir la LUN original de la configuración. La supresión también elimina cualquier instantánea o réplica en el sitio de destino que estuviera asociada al LUN original.
