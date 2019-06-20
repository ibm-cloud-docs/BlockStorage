---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestión de {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

Puede gestionar sus volúmenes de {{site.data.keyword.blockstoragefull}} mediante la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}. En el **menú**, seleccione **Infraestructura clásica** para interactuar con los servicios clásicos.

## Visualización de los detalles de LUN de {{site.data.keyword.blockstorageshort}}

Puede ver un resumen de la información clave para la LUN de almacenamiento seleccionada, incluidas las prestaciones adicionales de instantánea y réplica que se añadieron al almacenamiento.

1. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**.
2. Pulse el Nombre de LUN adecuado en la lista.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli block volume-detail --help
Uso: slcli block volume-detail [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --help  Mostrar este mensaje y salir.
```

## Autorización de hosts para acceder a {{site.data.keyword.blockstorageshort}}

Los hosts "autorizados" son hosts a los que se les ha otorgado acceso a una LUN particular. Sin la autorización del host, no puede acceder ni utilizar el almacenamiento de su sistema. Autorizar a un host a que acceda a LUN genera el nombre de usuario, la contraseña y el nombre calificado iSCSI (IQN), que se necesitan para montar la conexión iSCSI de E/S de multivía de acceso (MPIO).

Puede autorizar y conectar hosts que estén ubicados en el mismo centro de datos que su almacenamiento. Puede tener varias cuentas, pero no puede autorizar a un host de una cuenta a que acceda a su almacenamiento en otra cuenta.
{:important}

1. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**, y pulse el nombre de LUN.
2. Desplácese a la sección **Hosts autorizados** de la página.
3. En la parte derecha, pulse **Autorizar host**. Seleccione los hosts que pueden acceder a esa LUN determinada.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli block access-authorize --help
Uso: slcli block access-authorize [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXT    El ID del servidor de hardware que se va a autorizar.
  -v, --virtual-id TEXT     El ID de un servidor virtual que se va a autorizar.
  -i, --ip-address-id TEXT  El ID de una dirección IP que se va a autorizar.
  -p, --ip-address TEXT     Una dirección IP que se va a autorizar.
  --help                    Mostrar este mensaje y salir.
```

## Visualización de la lista de hosts que están autorizados para acceder a una LUN de {{site.data.keyword.blockstorageshort}}

1. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**, y pulse el nombre de LUN.
2. Desplácese hasta la sección **Hosts autorizados**.

Allí puede ver la lista de hosts, que actualmente tienen autorización para acceder a la LUN. También puede ver la información de autenticación necesarias para realizar una conexión: nombre de usuario, contraseña y host de IQN. La dirección de destino aparece listada en la página **Detalle de almacenamiento**. Para NFS, la dirección de destino se describe como un nombre de DNS y, para iSCSI, es la dirección IP de Descubrir portal de destino.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli block access-list --help
Uso: slcli block access-list [OPCIONES] ID_VOLUMEN

Opciones:
  --sortby TEXTO  Columna por la que se debe ordenar
  --columns TEXTO Columnas que se deben visualizar. Opciones: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Mostrar este mensaje y salir.
```

## Visualización del {{site.data.keyword.blockstorageshort}} al cual un host está autorizado

Puede ver las LUN a las cuales un host tiene acceso, incluida la información necesaria para realizar una conexión: nombre de LUN, tipo de almacenamiento, dirección de destino, capacidad y ubicación:

1. Pulse **Dispositivos** -> **Lista de dispositivos** en el y pulse sobre el dispositivo adecuado.
2. Seleccione el separador **Almacenamiento**.

Se le presentará una lista de las LUN de almacenamiento a las cuales este host tiene acceso. La lista está agrupada por tipo de almacenamiento (bloque, archivo, otros). Puede autorizar más almacenamiento o puede eliminar el acceso pulsando **Acciones**.

No se puede autorizar a un host para que acceda a LUN de distintos tipos de SO a la vez. Solo se puede autorizar a un host a que acceda a LUN de un solo tipo de SO. Si intenta autorizar el acceso a varias LUN con distintos tipos de SO, la operación genera un error.
{:note}

## Montaje y desmontaje de {{site.data.keyword.blockstorageshort}}

En función del sistema operativo del host, siga las instrucciones adecuadas.

- [Conexión a LUN en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conexión a LUN en CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conexión a LUN en Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configuración de almacenamiento en bloque para la copia de seguridad con cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configuración de almacenamiento en bloque para la copia de seguridad con Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## Revocación del acceso de un host a {{site.data.keyword.blockstorageshort}}

Si quiere detener el acceso desde un host a una LUN de almacenamiento determinada, puede revocar el acceso. Al revocar el acceso, la conexión de host se descarta de la LUN. El sistema operativo y las aplicaciones de ese host ya no se pueden comunicar con la LUN.

Para evitar problemas del lado del host, desmonte la LUN de almacenamiento desde el sistema operativo antes de revocar el acceso para evitar perder unidades o corrupción de datos.
{:important}

Puede revocar el acceso desde la **Lista de dispositivos** o desde la **Vista de almacenamiento**.

### Revocación del acceso de la lista de dispositivos

1. Pulse **Dispositivos**, **Lista de dispositivos** en la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external} y realice una doble pulsación sobre el dispositivo adecuado.
2. Seleccione el separador **Almacenamiento**.
3. Se le presentará una lista de las LUN de almacenamiento a las cuales este host tiene acceso. La lista está agrupada por tipo de almacenamiento (bloque, archivo, otros). Junto al nombre de LUN, seleccione **Acción** y pulse **Revocar acceso****.
4. Confirme que desea revocar el acceso a la LUN porque la acción no puede deshacerse. Pulse **Sí** para revocar el acceso a la LUN o **No** para cancelar la acción.

Si desea desconectar varias LUN desde un host específico, debe repetir la acción Revocar acceso para cada LUN.
{:tip}


### Revocación del acceso de la vista de almacenamiento

1. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** y seleccione la LUN desde la cual desea revocar el acceso.
2. Desplácese hasta la sección **Hosts autorizados**.
3. Pulse **Acciones** junto al host cuyo acceso se va a revocar y seleccione **Revocar acceso**.
4. Confirme que desea revocar el acceso a la LUN porque la acción no puede deshacerse. Pulse **Sí** para revocar el acceso a la LUN o **No** para cancelar la acción.

Si desea desconectar varios hosts de una LUN específica, debe repetir la acción Revocar acceso para cada host.
{:tip}

### Revocación del acceso mediante SLCLI.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli block access-revoke --help
Uso: slcli block access-revoke [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXT    El ID del servidor de hardware cuya autorización se va a revocar.
  -v, --virtual-id TEXT     El ID de un servidor virtual cuya autorización se va a revocar.
  -i, --ip-address-id TEXT  El ID de una dirección IP cuya autorización se va a revocar.
  -p, --ip-address TEXT     Una dirección IP cuya autorización se va a revocar.
  --help                    Mostrar este mensaje y salir.
```

## Cancelación de una LUN de almacenamiento

Si ya no necesita una LUN específica, puede cancelarla en cualquier momento.

Para cancelar una LUN de almacenamiento, es necesario revocar el acceso de los hosts en primer lugar.
{:important}

1. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**.
2. Pulse **Acciones** para la LUN que se va a cancelar y seleccione **Cancelar {{site.data.keyword.blockstorageshort}}**.
3. Confirme si desea cancelar la LUN inmediatamente o en la fecha de aniversario en la que se suministró la LUN.

   Si selecciona la opción de cancelar la LUN en su fecha de aniversario, puede anular la solicitud de cancelación antes de su fecha de aniversario.
   {:tip}
4. Pulse **Continuar** o **Cerrar**.
5. Marque el recuadro de selección **Acuse de recibo** y pulse **Confirmar**.

De manera alternativa, puede utilizar el mandato siguiente en SLCLI.
```
# slcli block volume-cancel --help
Uso: slcli block volume-cancel [OPCIONES] ID_VOLUMEN

Opciones:
  --reason TEXTO Una razón para la cancelación (opcional)
  --immediate    Cancelar el volumen de almacenamiento de bloques inmediatamente en lugar de
                 hacerlo en el aniversario de facturación
  -h, --help     Mostrar este mensaje y salir.
```

Puede esperar que la LUN siga resultando visible en la lista de almacenamiento durante al menos 24 horas (cancelación inmediata) o hasta la fecha de aniversario. Algunas características dejarán de estar disponibles, pero el volumen sigue resultando visible hasta que se reclame. Sin embargo, la facturación se detiene inmediatamente después de que pulse Suprimir/Cancelar.

La activación de réplicas puede bloquear la reclamación del volumen de almacenamiento. Asegúrese de que el volumen ya no esté montado, de que las autorizaciones de host se hayan revocado y de que la réplica se haya cancelado antes de intentar cancelar el volumen original.
