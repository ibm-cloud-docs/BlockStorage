---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gestión de instantáneas

## Creación de una planificación de instantáneas

Usted decide con qué frecuencia y cuándo se debe crear una referencia de un punto en el tiempo de su volumen de almacenamiento con planificaciones de instantáneas. Puede tener un máximo de 50 instantáneas del volumen de almacenamiento. Las planificaciones se gestionan a través del separador **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Para poder configurar la planificación inicial, debe adquirir el espacio de instantáneas si no lo ha comprado durante el suministro inicial del volumen de almacenamiento.

### Adición de una planificación de instantáneas

Las planificaciones de instantáneas se pueden configurar para intervalos por horas, diarios y semanales, cada uno con un ciclo de retención distinto. Hay un máximo de 50 instantáneas planificadas, que pueden ser una combinación de planificaciones por horas, diarias y semanales, e instantáneas manuales por volumen de almacenamiento.

1. Pulse el volumen de almacenamiento, pulse **Acciones** y luego pulse **Planificar instantánea**.
2. En la ventana Nueva planificación de instantáneas, puede elegir entre tres frecuencias de instantáneas distintas. Utilice cualquier combinación de las tres para crear una planificación de instantáneas completa.
   - Por hora
      - Especifique el minuto de cada hora en el que se debe tomar una instantánea. El valor predeterminado es el minuto actual.
      - Especifique el número de instantáneas por hora que se conservarán antes de descartar las más antiguas.
   - Diario
      - Especifique la hora y el minuto en que se va a tomar una instantánea. El valor predeterminado es la hora y el minuto actuales.
      - Seleccione el número de instantáneas por hora que se deben conservar antes de descartar las más antiguas.
   - Semanal
      - Especifique el día de la semana, la hora y el minuto en que se va a tomar una instantánea. El valor predeterminado es el día, la hora y el minuto actuales.
      - Seleccione el número de instantáneas semanales que se deben conservar antes de descartar las más antiguas.
3. Pulse **Guardar** y cree otra planificación con una frecuencia distinta. Si el número total de instantáneas planificada es superior a 50, recibe un mensaje de aviso y no podrá guardar.

Las instantáneas de la lista se muestran tal como se han tomado en la sección **Instantáneas** de la página **Detalles**.

## Toma de una instantánea manual

Las instantáneas manuales se pueden realizar en distintos puntos durante una actualización o mantenimiento de la aplicación. También puede tomar instantáneas en varios servidores que se desactivaron temporalmente a nivel de aplicación.

Hay un máximo de 50 instantáneas manuales por volumen de almacenamiento.

1. Pulse el volumen de almacenamiento.
2. Pulse **Acciones**.
3. Pulse **Realizar instantánea manual**.
Se realiza la instantánea y se muestra en la sección **Instantáneas** de la página **Detalles**. Su planificación es Manual.

## Listado de todas las instantáneas con información de espacio utilizado y de funciones de gestión

Se puede visualizar una lista de las instantáneas retenidas y el espacio utilizado en la página **Detalles** (Almacenamiento, {{site.data.keyword.blockstorageshort}}). Las funciones de gestión (editar planificaciones y añadir más espacio) se realizan en la página Detalles utilizando el menú **Acciones** o los enlaces de las distintas secciones de la página.

## Visualización de la lista de instantáneas retenidas

Las instantáneas retenidas se basan en el número que haya especificado en el campo **Mantener la última** al configurar las planificaciones. Puede visualizar las instantáneas que se han realizado en la sección **Instantáneas**. Las instantáneas se listan por planificación.

## Visualización de la cantidad de espacio de instantáneas que se utiliza

El gráfico circular en la parte superior de la página **Detalles** muestra la cantidad de espacio que se utiliza y la cantidad de espacio que queda. Recibirá notificaciones cuando empiece a alcanzar los umbrales de espacio: 75 %, 90 % y 95 %.

## Cambio de la cantidad de espacio de instantáneas para un volumen

Es posible que necesite añadir espacio de instantáneas a un volumen que anteriormente no tuviera o que requiera espacio de instantáneas adicional. Puede añadir entre 5 GB y 4.000 GB, en función de sus necesidades. 

**Nota**: El espacio de instantáneas solo se puede aumentar. No se puede reducir. Puede seleccionar una cantidad de espacio inferior hasta que determine cuánto espacio necesita realmente. Recuerde que las instantáneas automatizadas y manuales comparten el espacio.

El espacio de instantáneas se cambia seleccionando **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.

1. Pulse los volúmenes de almacenamiento, pulse **Acciones** y luego pulse **Añadir más espacio de instantáneas**.
2. Seleccione entre un rango de tamaños que se le ofrece. Los tamaños normalmente oscilan entre 0 y el tamaño de su volumen.
3. Pulse **Continuar**.
4. Especifique cualquier código promocional que tenga y pulse **Recalcular**. Los campos Cargos para este pedido y Revisión de pedido contienen información de forma predeterminada.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas adicional se suministra en pocos minutos.

## Recepción de notificaciones cuando se alcanza el límite de espacio de instantánea y se suprimen las instantáneas

Las notificaciones se envían a través de incidencias de soporte al usuario maestro en la cuenta cuando se alcanzan tres umbrales de espacio distintos: 75 %, 90 % y 95 %.

- **75 % de capacidad**: Se envía un aviso que indica que el uso de espacio de instantáneas ha superado el 75 %. Si presta atención al aviso y añade espacio manualmente o suprime las instantáneas retenidas innecesarias, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- **90 % de capacidad**: Se envía un segundo aviso que indica que el uso de espacio de instantáneas ha superado el 90 %. Al igual que sucede cuando alcanza el 75 % de capacidad, si realiza las acciones necesarias para disminuir el espacio utilizado, la acción se anota y la incidencia se cierra. Si no hace nada, deberá reconocer la incidencia manualmente y se cerrará.
- **95 % de capacidad**: se envía un aviso final. Si no se realiza ninguna acción para reducir el uso del espacio por debajo del umbral, se genera una notificación y se produce la supresión automática para que se puedan crear instantáneas futuras. Se suprimen instantáneas planificadas, empezando por las más antiguas, hasta que el uso cae por debajo del 95 %. Se siguen suprimiendo instantáneas cada vez que el uso supera el 95% hasta que cae por debajo del umbral. Si el espacio se aumenta manualmente o si se suprimen instantáneas, el aviso se restablece y se volverá a enviar si se vuelve a superar el umbral. Si no se lleva a cabo ninguna acción, esta notificación es el único aviso que recibe.

## Supresión de una planificación de instantáneas

Las planificaciones de instantáneas se pueden cancelar seleccionando **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.

1. Pulse la planificación que se va a suprimir en el marco **Planificaciones de instantáneas** de la página **Detalles**.
2. Marque el recuadro de selección junto a la planificación que se va a suprimir y pulse **Guardar**.<br />

>**Precaución**: Si está utilizando la característica de réplica, asegúrese de que la planificación que está suprimiendo no sea la planificación utilizada por la réplica. Pulse [aquí](replication.html) para obtener más información sobre cómo suprimir una planificación de réplica.

## Supresión de una instantánea

Las instantáneas que ya no se necesiten se pueden eliminar manualmente para liberar espacio para futuras instantáneas. La supresión se realiza mediante **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.

1. Pulse el volumen de almacenamiento y desplácese hasta la sección **Instantánea** para ver la lista de instantáneas existentes.
2. Pulse **Acciones** junto a una instantánea particular y pulse **Suprimir** para suprimir la instantánea. Esta supresión no afectará a ninguna instantánea anterior ni futura de la misma planificación, ya que no existe dependencia entre instantáneas.

Las instantáneas manuales que no se supriman del modo descrito anteriormente, se suprimirán automáticamente cuando alcance las limitaciones de espacio (las más antiguas primero).

## Restauración del volumen de almacenamiento a un punto en el tiempo específico utilizando una instantánea

Es posible que necesite recuperar el volumen de almacenamiento a un punto en el tiempo específico debido a un error de usuario o porque los datos hayan resultado dañados.

1. Desmonte y desconecte el volumen de almacenamiento del host.
   - Pulse [aquí](accessing_block_storage_linux.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Linux.
   - Pulse [aquí](accessing-block-storage-windows.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Microsoft Windows.
2. Pulse **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Desplácese y pulse el volumen que se va a restaurar. La sección **Instantáneas** de la página **Detalles** mostrará una lista de todas las instantáneas guardadas junto con su tamaño y fecha de creación.
4. Pulse **Acciones** junto a la instantánea que se va a utilizar y pulse **Restaurar**. <br/>
   >**Nota**: La restauración de los resultados da como resultado la pérdida de los datos que se han creado o modificado después de que la instantánea se haya realizado. Esta pérdida de datos se produce porque el volumen de almacenamiento vuelve al mismo estado en el que se encontraba en el momento de la instantánea. 
5. Pulse **Sí** para iniciar la restauración. Recibirá un mensaje en la parte superior de la página que indicará que el volumen se ha restaurado utilizando la instantánea seleccionada. También aparecerá un icono junto al volumen en {{site.data.keyword.blockstorageshort}} que indicará que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre una ventana que muestra la transacción. El icono desaparecerá una vez completada la transacción.
6. Monte y vuelva a conectar el volumen de almacenamiento al host.
   - Pulse [aquí](accessing_block_storage_linux.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Linux.
   - Pulse [aquí](accessing-block-storage-windows.html) para obtener las instrucciones de {{site.data.keyword.blockstorageshort}} en Microsoft Windows.
   
>**Nota**: La restauración de un volumen da lugar a la supresión de todas las instantáneas que se tomaron antes de la instantánea restaurada.
