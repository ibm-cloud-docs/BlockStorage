---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestión de instantáneas

## ¿Cómo creo una planificación de instantáneas?

Las planificaciones de instantáneas le permiten decidir con qué frecuencia y cuándo desea crear una referencia de un punto en el tiempo de su volumen de almacenamiento. Puede tener un máximo de 50 instantáneas del volumen de almacenamiento. Las planificaciones se gestionan a través del separador **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Antes de poder configurar la planificación inicial, debe adquirir el espacio de instantáneas si no lo ha comprado durante el suministro inicial del volumen de almacenamiento.

### ¿Cómo añado una planificación de instantáneas?

Las planificaciones de instantáneas se pueden configurar para intervalos por horas, diarios y semanales, cada uno con un ciclo de retención distinto. Hay un máximo de 50 instantáneas planificadas, que pueden ser una combinación de planificaciones por horas, diarias y semanales, e instantáneas manuales por volumen de almacenamiento.

1. Pulse en el volumen de almacenamiento, seleccione el recuadro desplegable **Acciones** y pulse **Planificar instantánea**.
2. En el diálogo Nueva planificación de instantáneas, puede elegir entre tres frecuencias de instantáneas distintas. Utilice cualquier combinación de las tres para crear una planificación de instantáneas completa.
   - Por hora
      - Especifique el minuto de cada hora a la que deberá realizarse una instantánea; el valor predeterminado es el minuto actual.
      - Especifique el número de instantáneas por hora que se conservarán antes de descartar las más antiguas.
   - Diario
      - Especifique la hora y el minuto a la que deberá realizarse una instantánea; el valor predeterminado es la hora y el minuto actual.
      - Especifique el número de instantáneas diarias que se conservarán antes de descartar las más antiguas.
   - Semanal
      - Especifique el día de la semana, la hora y el minuto a la que deberá realizarse una instantánea; el valor predeterminado es el día, hora y minuto actual.
      - Especifique el número de instantáneas semanales que se conservarán antes de descartar las más antiguas.
3. Pulse **Guardar** y cree otra planificación con una frecuencia diferente. Tenga en cuenta que recibirá un mensaje de aviso y no podrá guardar si el número total de instantáneas planificadas es superior a 50.

Se mostrará una lista de las instantáneas a medida que se realizan en la sección Instantáneas de la página de detalles.

## ¿Cómo realizo una instantánea manual?

Las instantáneas manuales se pueden realizar en distintos puntos durante una actualización o mantenimiento de la aplicación. También se permite realizar instantáneas en varias máquinas que se hayan desactivado temporalmente a nivel de aplicación.

Hay un máximo de 50 instantáneas manual por volumen de almacenamiento.

1. Pulse en el volumen de almacenamiento.
2. Pulse el recuadro desplegable Acciones.
3. Pulse **Realizar instantánea manual**.
La instantánea se realizará y se mostrará en la sección Instantáneas de la página de detalles. Su planificación será Manual.

## ¿Cómo visualizo una lista de instantáneas con las funciones de gestión y espacio consumido?

Se puede visualizar una lista de las instantáneas retenidas y el espacio consumido en la página **Detalles** (Almacenamiento, {{site.data.keyword.blockstorageshort}}). Las funciones de gestión (editar planificaciones y añadir más espacio) se realizan en la página Detalles utilizando el menú desplegable **Acciones** o enlaces en las distintas secciones de la página.

## ¿Cómo visualizo una lista de instantáneas retenidas?

Las instantáneas retenidas se basan en el número que haya especificado en Mantener el último campo al configurar las planificaciones. Puede visualizar las instantáneas que se han realizado en la sección Instantánea. Las instantáneas se listan por planificación.

## ¿Cómo visualizo cuánto espacio de instantáneas se ha utilizado?

El gráfico circular en la parte superior de la página Detalles muestra cuánto espacio se ha utilizado y cuánto espacio queda. Recibirá notificaciones cuando empiece a alcanzar los umbrales de espacio – 75%, 90%, y 95%.

## ¿Cómo cambio la cantidad de espacio de instantáneas para mi volumen?

Es posible que necesite añadir espacio de instantáneas a un volumen que anteriormente no tuviera o que requiera espacio de instantáneas adicional. Puede añadir entre 5 GB y 4.000 GB, en función de sus necesidades. 

**Nota**: El espacio de instantáneas sólo se puede aumentar, no reducir. Le conviene seleccionar una cantidad de espacio inferior hasta que determine cuánto espacio necesita realmente. Recuerde, las instantáneas automatizadas y manuales comparten el mismo espacio.

El espacio de instantáneas se cambia a través de **Almacenamiento, {{site.data.keyword.blockstorageshort}}**.

1. Pulse en los volúmenes de almacenamiento, seleccione el recuadro desplegable **Acciones** y pulse **Añadir más espacio de instantáneas**.
2. Seleccione entre un rango de tamaños que se le ofrece. Los tamaños normalmente oscilan entre 0 y el tamaño de su volumen.
3. Pulse **Continuar** para suministrar el espacio adicional.
4. Especifique cualquier código promocional que tenga y pulse **Recalcular**. Las opciones Cargos para este pedido y Revisión del pedido tendrán valores predeterminados.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas adicional se suministrará en unos minutos.

## ¿Cómo recibo notificaciones cuando me acerque al límite de espacio de instantáneas y las instantáneas se supriman?

Las notificaciones se envían a través de incidencias de soporte desde el Soporte al Usuario maestro en su cuenta, cuando alcance tres umbrales de espacio distintos – 75%, 90%, y 95%.

- **75% de capacidad**: Se envía un aviso de que la utilización de espacio de instantáneas ha superado el 75%. Si presta atención al aviso y manualmente añade espacio o suprime instantáneas retenidas innecesarias, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- **90% de capacidad**: Se envía un segundo aviso cuando la utilización de espacio de instantáneas ha superado el 90%. Al igual que cuando alcanza el 75% de capacidad, si realiza las acciones necesarias para disminuir el espacio utilizado, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- **95% de capacidad**: Se envía un aviso final. Si no se realiza ninguna acción para reducir el espacio por debajo del umbral, se genera una notificación y se produce la supresión automática para que se puedan crear instantáneas futuras. Las instantáneas planificadas se suprimen, empezando por las más antiguas, hasta que el uso cae por debajo del 95%, y se seguirán suprimiendo cada vez que la utilización supere el 95% hasta quedar por debajo del umbral. Si el espacio se aumenta manualmente o las instantáneas se suprimen, el aviso se restablece y se volverá a enviar si se vuelve a superar el umbral. Si no se lleva a cabo ninguna acción, este será el único aviso que se reciba.

## ¿Cómo suprimo una planificación de instantáneas?

Las planificaciones de instantáneas se pueden cancelar a través de **Almacenamiento, {{site.data.keyword.blockstorageshort}}**.

1. Pulse en la planificación que se va a suprimir en el marco **Planificaciones de instantáneas** de la página **Detalles**.
2. Marque el recuadro de selección junto a la planificación que se va a suprimir y pulse **Guardar**.<br />
**Precaución**: Si está utilizando la característica de réplica, asegúrese de que la planificación que está suprimiendo no sea la planificación utilizada por la réplica. Pulse [aquí](replication.html) para obtener más información sobre cómo suprimir una planificación de réplica.

## ¿Cómo suprimo una instantánea?

Las instantáneas que ya no se necesiten se pueden eliminar manualmente para liberar espacio para futuras instantáneas. La supresión se realiza a través de **Almacenamiento, {{site.data.keyword.blockstorageshort}}**.

1. Pulse en el volumen de almacenamiento y desplácese a la sección Instantáneas para ver una lista de las instantáneas existentes.
2. Pulse la lista desplegable **Acciones** situada junto a una instantánea particular y pulse **Suprimir** para suprimir la instantánea. Esto no afectará a ninguna instantánea anterior ni futura de la misma planificación, ya que no existe dependencia entre instantáneas.

Las instantáneas manuales que no se supriman del modo descrito anteriormente, se suprimirán automáticamente (las más antiguas primero) cuando alcance las limitaciones de espacio.

## ¿Cómo restauro mi volumen de almacenamiento a un momento específico utilizando una instantánea?

Es posible que necesite recuperar el volumen de almacenamiento a un momento específico debido a un error de usuario o una corrupción de datos.

1. Desmonte y desconecte el volumen de almacenamiento del host.
   - Pulse [aquí](accessing_block_storage_linux.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Linux.
   - Pulse [aquí](accessing-block-storage-windows.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Microsoft Windows.
2. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Desplácese y pulse el volumen que se va a restaurar. La sección **Instantáneas** de la página **Detalles** mostrará una lista de todas las instantáneas guardadas junto con su tamaño y fecha de creación.
4. Pulse el botón **Acciones** de la instantánea que se va a utilizar y pulse **Restaurar**. <br/>
  **Nota**: la realización de una restauración provocará la pérdida de los datos que se hayan creado o modificado desde que la instantánea utilizada se creó. Una vez completada, el volumen de almacenamiento volverá al mismo estado que cuando se realizó la instantánea. Aparecerá una solicitud que le informará de ello.
5. Pulse **Sí** para iniciar la restauración. Recibirá un mensaje en la parte superior de la página indicando que el volumen se ha restaurado utilizando la instantánea seleccionada. Además, aparecerá un icono junto al volumen en el {{site.data.keyword.blockstorageshort}} indicando que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre un diálogo que indica la transacción. El icono desaparecerá una vez completada la transacción.
6. Monte y vuelva a conectar el volumen de almacenamiento al host.
   - Pulse [aquí](accessing_block_storage_linux.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Linux.
   - Pulse [aquí](accessing-block-storage-windows.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Microsoft Windows.
   
**Nota**: Restaurar un volumen provocará la supresión de todas las instantáneas anteriores a la instantánea restaurada.
