---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Migración de {{site.data.keyword.blockstorageshort}} a {{site.data.keyword.blockstorageshort}} cifrado

Ahora {{site.data.keyword.blockstoragefull}} para Resistencia o Rendimiento está disponible en centros de datos seleccionados. A continuación, encontrará información sobre cómo migrar su {{site.data.keyword.blockstorageshort}} de no cifrado a cifrado. Para obtener más información sobre el almacenamiento cifrado gestionado por el proveedor, lea el [artículo Cifrado en reposo de {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html). Para consultar una lista de centros de datos actualizados y características disponibles, pulse [aquí](new-ibm-block-and-file-storage-location-and-features.html).

La vía de acceso de migración preferida es conectarse a ambos LUN simultáneamente y transferir datos directamente desde un LUN de archivos a otro. Los detalles dependerán de su sistema operativo y de si se espera que los datos cambien durante la operación de copia.

Se describen los casos más comunes para su comodidad. Se supone que ya tiene el LUN de archivos no cifrados conectado al host. De lo contrario, siga las direcciones a continuación que mejor se ajusten a su sistema operativo en ejecución para completar esta tarea.

- [Acceso a {{site.data.keyword.blockstorageshort}} en Linux](accessing_block_storage_linux.html)

- [Acceso a {{site.data.keyword.blockstorageshort}} en Windows](accessing-block-storage-windows.html)

 
## Crear un LUN cifrado

Efectúe los pasos siguientes para crear un LUN del mismo tamaño o más grande que esté cifrado para facilitar el proceso de migración. 
Realizar un pedido de un LUN de almacenamiento de Resistencia cifrado

1. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** desde la página de inicio de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} O pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** en el catálogo de {{site.data.keyword.BluSoftlayer_full}}.

2. Pulse sobre el enlace **Realizar el pedido de {{site.data.keyword.blockstorageshort}}** en la página de {{site.data.keyword.blockstorageshort}}.

3. Seleccione **Resistencia**.

4. Seleccione el centro de datos donde esté ubicado el LUN original. <br/> **Nota:** El cifrado solo está disponible en centros de datos seleccionados.

5. Seleccione el nivel de IOPS deseado.

6. Seleccione la cantidad de espacio de almacenamiento deseada en GB. Para TB, 1 TB equivale a 1.000 GB, y 12 TB equivale a 12.000 GB.

7. Especifique la cantidad deseada de almacenamiento en GB para las instantáneas.

8. Seleccione el sistema operativo VMware OS de la lista desplegable.

9. Envíe el pedido.

## Realizar un pedido de un LUN de almacenamiento de Rendimiento cifrado

1. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** desde la página de inicio de [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} O pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** en el catálogo de {{site.data.keyword.BluSoftlayer_full}}.

2. Pulse **Realizar pedido de {{site.data.keyword.blockstorageshort}}**.

3. Seleccione **Rendimiento**.

4. Seleccione el centro de datos donde esté ubicado el LUN original. Tenga en cuenta que el cifrado solo está disponible en centros de datos con un asterisco (`*`).

5. Seleccione la cantidad de espacio de almacenamiento deseada en GB. Para TB, 1 TB equivale a 1.000 GB, y 12 TB equivale a 12.000 GB.

6. Especifique la cantidad de IOPS deseada en intervalos de 100.

7. Seleccione el sistema operativo VMware OS de la lista desplegable.

8. Envíe el pedido.

El almacenamiento se suministrará en menos de un minuto y estará visible en la página de {{site.data.keyword.blockstorageshort}} del {{site.data.keyword.slportal}}.

 
## Conectar un nuevo volumen al host

Los hosts “autorizados” son los hosts a los que se les ha otorgado derechos de acceso a un volumen. Sin la autorización del host, no podrá acceder ni utilizar el almacenamiento de su sistema. Autorizar a un host el acceso al volumen genera el nombre de usuario, contraseña y nombre calificado iSCSI (IQN), que se necesita para montar la conexión iSCSI de E/S de multivía de acceso (MPIO).

1. Pulse **Almacenamiento**  > **{{site.data.keyword.blockstorageshort}}** y pulse el Nombre de LUN.

2. Desplácese a la sección **Hosts autorizados** de la página.

3. Pulse el enlace **Autorizar host** de la parte derecha de la página. Seleccione los hosts que pueden acceder al volumen.

 
## Instantáneas y réplica

¿Tiene instantáneas y réplicas establecidas para su LUN original? Si es así, deberá configurar la réplica, el espacio de instantáneas y crear planificaciones de instantáneas para el nuevo LUN cifrado con los mismos valores que el volumen original. 

Tenga en cuenta que si su centro de datos de destino de réplica no se ha actualizado para el cifrado, no podrá establecer la réplica para el nuevo LUN hasta que actualice el centro de datos.

 
## Migrar los datos

Debe estar conectado tanto al LUN de {{site.data.keyword.blockstorageshort}} original y como al LUN cifrado. 
- Si no, asegúrese de haber seguido correctamente los pasos anteriores y las referencias en otras publicaciones. Abra una incidencia de soporte para obtener ayuda en la conexión de dos LUN.

### Consideraciones sobre los datos

En este punto, considere qué tipo de datos tiene en el LUN de {{site.data.keyword.blockstorageshort}} original y cómo copiarlos mejor al LUN cifrado. Si tiene copias de seguridad, contenido estático y cosas que no se espera que cambien durante la copia, no son consideraciones importantes.

Si está ejecutando una base de datos o una máquina virtual en su {{site.data.keyword.blockstorageshort}}, asegúrese de que los datos del LUN original no se modifiquen durante la copia, para que no se corrompan. Si tiene problemas con el ancho de banda, debería realizar la migración fuera de las horas punta. Si necesita ayuda con estas consideraciones, no dude en abrir una incidencia de soporte.
 
### Microsoft
Windows

Para copiar los datos desde su LUN de {{site.data.keyword.blockstorageshort}} original al LUN cifrado, formatee el nuevo almacenamiento y copie los archivos encima utilizando Windows Explorer.

 
### Linux

Debería considerar utilizar rsync para copiar sobre los datos. A continuación se muestra un mandato de ejemplo:

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

Se recomienda utilizar el mandato anterior con el distintivo --dry-run una vez para asegurarse de que las vías de acceso se alinean correctamente. Si este proceso se interrumpe, suprima mejor el último archivo de destino que se ha copiado para asegurarse de que se ha copiado desde el principio a la nueva ubicación.

Una vez completado este mandato sin el distintivo --dry-run, los datos deberían copiarse al LUN de {{site.data.keyword.blockstorageshort}} cifrado. Debería ejecutar el mandato de nuevo para asegurarse de que no falta nada. También podría revisar manualmente ambas ubicaciones por si se ha omitido algo.

Una vez completada la migración, podrá pasar la producción al LUN cifrado y desconectar y suprimir el LUN original de la configuración. Tenga en cuenta que la supresión también eliminará cualquier instantánea o réplica en el sitio de destino que estuviera asociada al LUN original.
