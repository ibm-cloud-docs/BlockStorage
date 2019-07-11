---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, accessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Recuperación tras desastre: migración tras error con un volumen primario accesible
{: #dr-accessible}

Si se produce un error catastrófico o un desastre en el sitio primario y el almacenamiento primario todavía está disponible, los clientes pueden llevar a cabo las siguientes acciones para acceder rápidamente a sus datos en el sitio secundario.

Antes de iniciar la migración tras error, asegúrese de que todas las autorizaciones de host están en vigor.
{:important}

Los hosts y volúmenes autorizados deben estar en el mismo centro de datos. Por ejemplo, no puede tener un volumen de réplica en Londres y el host en Ámsterdam. Ambos deben estar en Londres o en Ámsterdam.
{:note}

1. Inicie una sesión en la [consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external} y pulse el icono de **menú** en la parte superior izquierda. Seleccione **Infraestructura clásica**.
2. Pulse el volumen de origen o de destino en la página de **{{site.data.keyword.blockstorageshort}}**.
3. Pulse **Réplica**.
4. Desplácese hacia abajo hasta el marco **Autorizar hosts** y pulse **Autorizar hosts** en la parte derecha.
5. Marque el host que se va a autorizar para las réplicas. Para seleccionar varios hosts, utilice la tecla Control y pulse los hosts aplicables.
6. Pulse **Enviar**. Si no tiene ningún host disponible, se le solicitará comprar recursos de cálculo en el mismo centro de datos.


## Inicio de una migración tras error desde un volumen a su réplica

Si es inminente que se produzca un suceso de error, puede iniciar una **migración tras error** al volumen de destino. El volumen de destino se activa. Se activa la última instantánea replicada correctamente y el volumen pasa a estar disponible para su montaje. Los datos escritos en el volumen de origen desde el ciclo de réplica anterior se perderán.

Cuando se inicia una migración tras error, la relación de réplica se invierte. El volumen de destino pasa a ser el volumen de origen, y el volumen de origen anterior pasa a ser el destino, como indica el **Nombre de LUN** seguido de **REP**.

Las migraciones tras error se inician en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en la [[consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

Antes de continuar con estos pasos, desconecte el volumen. De lo contrario, los datos podrían resultar dañados o los podría perder.
{:important}

1. Pulse la LUN activa (“origen”).
2. Pulse **Réplica** y luego **Acciones**.
3. Seleccione **Migración tras error**.

   Recibirá un mensaje en la página que indicará que la migración tras error está en curso. También aparecerá un icono junto al volumen en **{{site.data.keyword.blockstorageshort}}** que indicará que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre una ventana que muestra la transacción. El icono desaparecerá una vez completada la transacción. Durante el proceso de migración tras error, las acciones relacionadas con la configuración son de solo lectura. No puede editar ninguna planificación de instantáneas ni cambiar el espacio de instantáneas. El suceso se registra en el historial de réplicas.<br/> Cuando el volumen de destino está activo, obtiene otro mensaje. El Nombre de LUN de su volumen de origen original se actualiza para finalizar en "REP" y su Estado pasa a ser Inactivo.
   {:note}
4. Pulse **Ver todos los {{site.data.keyword.blockstorageshort}}**.
5. Pulse la LUN activa (antes era el volumen de destino).
6. Monte y conecte el volumen de almacenamiento al host. Para obtener más información, consulte [Conexión del almacenamiento](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#mountingnewLUN).


## Inicio de un restablecimiento desde un volumen a su réplica

Cuando el volumen de origen original se ha reparado, puede iniciar un restablecimiento controlado a su volumen de origen original. En un restablecimiento controlado:

- El volumen de origen activo se pone fuera de línea.
- Se toma una instantánea.
- Se completa el ciclo de réplica.
- Se activa la instantánea de datos recién tomada.
- El volumen de origen pasa a estar activo para su montaje.

Cuando se inicia un restablecimiento, la relación de réplica se invierte de nuevo. El volumen de origen se restaura como el volumen de origen y el volumen de destino vuelve a ser el volumen de destino, tal como indica el **Nombre de LUN** seguido de **REP**.

Los restablecimientos se inician en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

1. Pulse la LUN activa ("destino").
2. En la parte superior derecha, pulse **Réplica** y luego **Acciones**.
3. Seleccione **Restablecimiento**.

   Recibirá un mensaje en la página que indicará que la migración tras error está en curso. También aparecerá un icono junto al volumen en **{{site.data.keyword.blockstorageshort}}** que indicará que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre una ventana que muestra la transacción. El icono desaparecerá una vez completada la transacción. Durante el proceso de retrotracción, las acciones relacionadas con la configuración son de solo lectura. No puede editar ninguna planificación de instantáneas ni cambiar el espacio de instantáneas. El suceso se registra en el historial de réplicas.
   {:note}
4. En la parte superior derecha, pulse **Ver todo {{site.data.keyword.blockstorageshort}}**.
5. Pulse la LUN activa ("origen").
6. Monte y conecte el volumen de almacenamiento al host. Para obtener más información, consulte [Conexión del almacenamiento](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#mountingnewLUN).
