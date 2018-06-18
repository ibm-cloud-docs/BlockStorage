---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gestión de {{site.data.keyword.blockstorageshort}}

Puede gestionar sus volúmenes de {{site.data.keyword.blockstoragefull}} mediante el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. Este artículo proporciona instrucciones para las tareas más comunes.

## Consultar los detalles de un LUN de {{site.data.keyword.blockstorageshort}} suministrado

Puede ver un resumen de la información clave para el LUN de almacenamiento seleccionado, incluyendo las prestaciones adicionales de instantánea y réplica que se han añadido al almacenamiento.

1. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**.
2. Pulse el Nombre de LUN adecuado en la lista.

## Autorizar que los hosts accedan a {{site.data.keyword.blockstorageshort}}

Los hosts "autorizados" son los hosts a los que se les ha otorgado derechos de acceso a un LUN determinado. Sin la autorización del host, no podrá acceder ni utilizar el almacenamiento de su sistema. Autorizar a un host a que acceda a LUN genera el nombre de usuario, la contraseña y el nombre calificado iSCSI (IQN), que se necesitan para montar la conexión iSCSI de E/S de multivía de acceso (MPIO).

**Nota**: solo puede autorizar y conectar hosts que residan en el mismo centro de datos que su almacenamiento. Si tiene varias cuentas, puede autorizar a un host de una cuenta a que acceda a su almacenamiento en otra.

1. Pulse **Almacenamiento** -> **{{site.data.keyword.blockstorageshort}}**, y pulse el Nombre de LUN.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. En la parte derecha, pulse **Autorizar host**. Seleccione los hosts que pueden acceder a ese LUN determinado.

 

## Visualizar la lista de hosts autorizados a un LUN de {{site.data.keyword.blockstorageshort}}

Efectúe los pasos siguientes para visualizar la lista de hosts autorizados a un LUN.

1. Pulse **Almacenamiento** -> **{{site.data.keyword.blockstorageshort}}**, y pulse el Nombre de LUN.
2. Desplácese hasta la sección **Hosts autorizados** en la parte inferior de la página.

Aquí verá la lista de hosts que actualmente tienen autorización para acceder al LUN y, específicamente para iSCSI, la información de autenticación necesaria para realizar una conexión: nombre de usuario, contraseña y host de IQN. La dirección de destino está en la parte superior de la página **Detalle de almacenamiento**. Para NFS, la dirección de destino se describe como un nombre de DNS y, para iSCSI, es la dirección IP de Descubrir portal de destino.

 

## Visualizar el {{site.data.keyword.blockstorageshort}} al cual un host está autorizado

Puede ver los LUN a los cuales un host tiene acceso, incluyendo la información necesaria para realizar una conexión: nombre de LUN, tipo de almacenamiento, dirección de destino, capacidad y ubicación:

1. Pulse **Dispositivos** -> **Lista de dispositivos** en el [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} y pulse sobre el dispositivo adecuado.
2. Seleccione el separador **Almacenamiento**.

Se le presentará una lista de los LUN de almacenamiento a los cuales este host tiene acceso. La lista está agrupada por tipo de almacenamiento (bloque, archivo, otros). Puede autorizar más almacenamiento o puede eliminar el acceso pulsando **Acciones**.

 

## Montar y desmontar {{site.data.keyword.blockstorageshort}}

Consulte los siguientes artículos con detalles:

- [{{site.data.keyword.blockstorageshort}} en Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} en Microsoft Windows](accessing-block-storage-windows.html)

 

## Revocar el acceso de un host a {{site.data.keyword.blockstorageshort}}

Si quiere detener el acceso desde un host a un LUN de almacenamiento determinado, puede revocar el acceso. Al revocar el acceso, la conexión de host se descartará del LUN. El sistema operativo y las aplicaciones ya no podrán comunicarse con el LUN.

**Nota**: Para impedir problemas del lado del host, desmonte el LUN de almacenamiento desde el sistema operativo antes de revocar el acceso para evitar perder unidades o corrupción de datos.

Puede revocar el acceso desde la **Lista de dispositivos** o desde la **Vista de almacenamiento**.

### Revocar el acceso desde la lista de dispositivos:

1. Pulse **Dispositivos**, **Lista de dispositivos** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} y efectúe una doble pulsación sobre el dispositivo adecuado.
2. Seleccione el separador **Almacenamiento**.
3. Se le presentará una lista de los LUN de almacenamiento a los cuales este host tiene acceso. La lista está agrupada por tipo de almacenamiento (bloque, archivo, otros). Junto al nombre de LUN, seleccione **Acción"" y pulse **Revocar acceso**.
4. Confirme que desea revocar el acceso al LUN porque la acción no puede deshacerse. Pulse **Yes** para revocar el acceso al LUN o **No** para cancelar la acción.

**Nota**: Si desea desconectar varios LUN desde un host específico, debe repetir la acción Revocar acceso para cada LUN.


### Revocar el acceso desde la vista de almacenamiento:

1. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** y seleccione el LUN desde el cual desea revocar el acceso.
2. Desplácese hasta la sección **Hosts autorizados**.
3. Pulse **Acciones** junto al host cuyo acceso se va a revocar y seleccione **Revocar acceso**.
4. Confirme que desea revocar el acceso al LUN porque la acción no puede deshacerse. Pulse **Yes** para revocar el acceso al LUN o **No** para cancelar la acción.

**Nota**: si desea desconectar varios hosts de un LUN específico, debe repetir la acción Revocar acceso para cada host.

 

## Cancelar un LUN de almacenamiento

Si ya no necesita un LUN específico, puede cancelarlo. Para cancelar un LUN de almacenamiento, primero es necesario revocar el acceso desde cualquier host.

1. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**.
2. Pulse **Acciones** para el LUN que se va a cancelar y seleccione **Cancelar {{site.data.keyword.blockstorageshort}}**.
3. Confirme si desea cancelar el LUN inmediatamente o en la fecha de aniversario en la que se suministró el LUN. 
**Nota**: Si selecciona la opción de cancelar el LUN en su fecha de aniversario, puede anular la solicitud de cancelación antes de su fecha de aniversario.
4. Pulse **Continuar** o **Cerrar**. 
5. Marque el recuadro de selección **Acuse de recibo** y pulse **Confirmar**.

 

