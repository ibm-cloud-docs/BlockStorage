---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Réplica de datos

La réplica utiliza una de sus planificaciones de instantáneas para copiar automáticamente las instantáneas a un volumen de destino de un centro de datos remoto. Las copias se pueden recuperar en el sitio remoto si se produce un suceso catastrófico o los datos resultan dañados.

Con las réplicas puede recuperarse rápidamente de anomalías del sitio y otros desastres. En caso de emergencia, puede realizar una migración tras error al volumen de destino y acceder a sus datos desde un determinado punto en el tiempo en la copia DR. Para obtener más información, consulte [Duplicación de volúmenes de réplica para la recuperación tras desastre](disaster-recovery.html).

La réplica conserva sus datos sincronizados en dos ubicaciones diferentes. Si solo desea clonar su volumen y utilizarlo independientemente del volumen original, consulte [Creación de un volumen de bloque duplicado](how-to-create-duplicate-volume.html).
{:tip}

Antes de poder replicar, debe crear una planificación de instantáneas. Cuando realiza la migración tras error, está "cambiando el conmutador" de su volumen de almacenamiento del centro de datos primario al volumen de destino del centro de datos remoto. Por ejemplo, su centro de datos primario es Londres y el centro de datos secundario es Ámsterdam. Si se produjera un suceso de error, debería realizar la migración a Ámsterdam, conectando al ahora volumen primario desde una instancia de cálculo en Ámsterdam. Cuando su volumen de Londres se haya reparado, se realizará una instantánea del volumen de Ámsterdam para volver a Londres y al volumen primario de nuevo desde una instancia de cálculo de Londres.


## Determinación del centro de datos remoto para mi volumen de almacenamiento replicado

Los centros de datos de {{site.data.keyword.BluSoftlayer_full}} están emparejados en combinaciones de centros primarios y remotos.
Consulte la Tabla 1 para ver la lista completa de disponibilidad de centros de datos y destinos de réplica.

<table>
  <caption style="text-align: left;"><p>Tabla 1: esta tabla muestra la lista completa de centros de datos con funciones mejoradas en cada región. Cada región está en una columna separada. Algunas ciudades, como Dallas, San José, Washington DC, Ámsterdam, Frankfurt, Londres y Sídney, tienen varios centros de datos.</p>
  <p>&#42; Los centros de datos de la región EE.UU. 1 NO tienen almacenamiento mejorado. Los hosts de los centros de datos con funciones mejoradas de almacenamiento <strong>no pueden</strong> iniciar la réplica con destinos de réplica en los centros de datos de EE.UU. 1.</p>
  </caption>
  <thead>
    <tr>
      <th>EE.UU. 1 &#42;</th>
      <th>EE.UU. 2</th>
      <th>Latinoamérica</th>
      <th>Canadá</th>
      <th>Europa</th>
      <th>Asia Pacífico</th>
      <th>Australia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>DAL01<br />
          DAL05<br />
	  DAL06<br />
	  HOU02<br />
	  SJC01<br />
	  WDC01<br />
	  <br /><br /><br /><br /><br /><br />
      </td>
      <td>SJC03<br />
	  SJC04<br />
	  WDC04<br />
	  WDC06<br />
	  WDC07<br />
	  DAL09<br />
	  DAL10<br />
	  DAL12<br />
	  DAL13<br />
	  <br /><br /><br />
      </td>
      <td>MEX01<br />
	  SAO01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>TOR01<br />
          MON01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
      <td>AMS01<br />
	  AMS03<br />
	  FRA02<br />
	  FRA04<br />
	  FRA05<br />
	  LON02<br />
	  LON04<br />
	  LON05<br />
	  LON06<br />
	  OSL01<br />
	  PAR01<br />
	  MIL01<br />
      </td>
      <td>HKG02<br />
          TOK02<br />
	  TOK04<br />
	  TOK05<br />
	  SNG01<br />
	  SEO01<br />
          CHE01<br />
	  <br /><br /><br /><br /><br />
      </td>
      <td>SYD01<br />
          SYD04<br />
	  MEL01<br />
	  <br /><br /><br /><br /><br /><br /><br /><br /><br />
      </td>
    </tr>
  </tbody>
</table>

## Creación de la réplica inicial

Las réplicas se basan en una planificación de réplica. Primero debe tener un espacio de instantáneas y una planificación de instantáneas para el volumen de origen antes de poder replicar. Si intenta configurar la réplica y uno o el otro no está en su lugar, se le solicitará que compre más espacio o que establezca una planificación. Las réplicas se gestionan bajo **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Pulse el volumen de almacenamiento.
2. Pulse **Réplica** y pulse **Adquirir una réplica**.
3. Seleccione la planificación de instantáneas existente que quiera que siga su réplica. La lista contiene todas las planificaciones de instantáneas activas. <br />
   Solo puede seleccionar una planificación, incluso si tiene una combinación de por hora, a diario y mensual. Todas las instantáneas capturadas desde el ciclo de réplica anterior se replicarán, independientemente de la planificación que las originó.<br />Si no tiene configuradas las instantáneas, se le solicitará que lo haga para poder solicitar una réplica. Consulte [Trabajar con instantáneas](snapshots.html) para obtener más detalles.
   {:important}
3. Pulse **Ubicación** y seleccione el centro de datos que es su sitio de recuperación tras desastre.
4. Pulse **Continuar**.
5. Especifique un **Código promocional** si tiene uno y pulse **Recalcular**. Los otros campos de la ventana se completan de forma predeterminada.
6. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**.


## Edición de una réplica existente

Puede editar la planificación de réplica y cambiar el espacio de réplica desde el separador **Primario** o **Réplica** de **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## Edición de la planificación de réplica

La planificación de réplica se basa en una planificación de instantáneas existente. Para cambiar la planificación de réplica, por ejemplo de por hora a semanalmente, debe cancelar la planificación de réplica y configurar una nueva.

Los cambios en la planificación pueden realizarse en el separador Primario o Réplica.

1. Pulse **Acciones** en el separador **Primario** o **Réplica**.
2. Seleccione **Editar planificación de instantáneas**.
3. Busque en el marco **Instantánea** bajo **Planificar** para determinar qué planificación está utilizando para la réplica. Cambie la planificación que desea. Por ejemplo, si la planificación de réplica es **Diaria**, puede cambiar la hora del día en que se realiza la réplica.
4. Pulse **Guardar**.


## Cambio del espacio de réplica

El espacio de instantáneas primario y el espacio de réplica deben ser el mismo. Si cambia el espacio en el separador **Primario** o **Réplica**, añade automáticamente espacio a los centros de datos de origen y de destino. El aumento del espacio de instantáneas desencadena también una actualización de réplica inmediata.

1. Pulse **Acciones** en el separador **Primario** o **Réplica**.
2. Seleccione **Añadir más espacio de instantáneas**.
3. Seleccione el tamaño de almacenamiento de la lista y pulse **Continuar**.
4. Especifique un **Código promocional** si tiene uno y pulse **Recalcular**. Los otros campos del recuadro de diálogo contienen información de forma predeterminada.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**.


## Visualización de los volúmenes de réplica en la lista de volúmenes

Puede visualizar los volúmenes de réplica en la página de {{site.data.keyword.blockstorageshort}}, en **Almacenamiento > {{site.data.keyword.blockstorageshort}}**. El **Nombre de LUN** muestra el nombre del volumen primario seguido de REP. El **Tipo** de réplica puede ser Resistente o Rendimiento. La **Dirección de destino** es N/D porque el volumen de réplica no está montado en el centro de datos y el **Estado** es Inactivo.


## Visualización de los detalles de un volumen replicado en el centro de datos de réplicas

Puede visualizar los detalles de un volumen de réplica en el separador **Réplica**, en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**. Otra opción es seleccionar el volumen de réplica desde la página de **{{site.data.keyword.blockstorageshort}}** y pulsar el separador **Réplica**.


## Especificación de las autorizaciones de host antes de que el servidor falle en el centro de datos secundario

Los hosts y volúmenes autorizados deben estar en el mismo centro de datos. No puede tener un volumen de réplica en Londres ni el host en Ámsterdam. Ambos deben estar en Londres o en Ámsterdam.

1. Pulse el volumen de origen o de destino en la página de **{{site.data.keyword.blockstorageshort}}**.
2. Pulse **Réplica**.
3. Desplácese hacia abajo hasta el marco **Autorizar hosts** y pulse **Autorizar hosts** en la parte derecha.
4. Marque el host que se va a autorizar para las réplicas. Para seleccionar varios hosts, utilice la tecla CTRL y pulse los hosts aplicables.
5. Pulse **Enviar**. Si no tiene ningún host, se le solicitará comprar recursos de cálculo en el mismo centro de datos.


## Aumento del espacio de instantáneas en el centro de datos de réplica cuando el espacio de instantáneas se incrementa en el centro de datos primario

Los tamaños de volumen deben ser los mismos para sus volúmenes de almacenamiento primario y de réplica. No puede haber uno mayor que otro. Cuando aumenta el espacio de instantáneas para su volumen primario, el espacio de réplica se aumenta automáticamente. El aumento del espacio de instantáneas desencadena una actualización de réplica inmediata. El aumento en ambos volúmenes se muestra como elementos de línea en su factura, y se prorratea en caso necesario.

Para obtener más información sobre cómo aumentar el espacio de instantáneas, consulte [Solicitud de instantáneas](ordering-snapshots.html).
{:tip}


## Inicio de una migración tras error desde un volumen a su réplica

Si se produce un suceso de error, puede iniciar una **migración tras error** al volumen de destino. El volumen de destino se activa. Se activa la última instantánea replicada correctamente y el volumen pasa a estar disponible para su montaje. Los datos escritos en el volumen de origen desde el ciclo de réplica anterior se perderán. Cuando se inicia una migración tras error, la relación de réplica se invierte. El volumen de destino pasa a ser el volumen de origen, y el volumen de origen anterior pasa a ser el destino, como indica el **Nombre de LUN** seguido de **REP**.

Las migraciones tras error se inician en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en el [[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Antes de continuar con estos pasos, desconecte el volumen. De lo contrario, dará lugar a la pérdida de datos o a que estos puedan resultar dañados.**

1. Pulse el LUN activo (“origen”).
2. Pulse **Réplica** y pulse el enlace **Acciones** en la esquina superior derecha.
3. Seleccione **Migración tras error**.
   >Recibirá un mensaje en la parte superior de la página que indicará que la migración tras error está en curso. También aparecerá un icono junto al volumen en **{{site.data.keyword.blockstorageshort}}** que indicará que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre una ventana que muestra la transacción. El icono desaparecerá una vez completada la transacción. Durante el proceso de migración tras error, las acciones relacionadas con la configuración son de solo lectura. No puede editar ninguna planificación de instantáneas ni cambiar el espacio de instantáneas. El suceso se registra en el historial de réplicas.<br/> Cuando el volumen de destino está activo, obtiene otro mensaje. El Nombre de LUN de su volumen de origen original se actualiza para finalizar en "REP" y su Estado pasa a ser Inactivo.
4. Pulse **Ver todos ({{site.data.keyword.blockstorageshort}})**.
5. Pulse el LUN activo (anteriormente volumen de destino).
6. Monte y conecte el volumen de almacenamiento al host. Pulse [aquí](provisioning-block_storage.html) para obtener instrucciones.


## Inicio de un restablecimiento desde un volumen a su réplica

Cuando el volumen de origen original se ha reparado, puede iniciar un restablecimiento controlado a su volumen de origen original. En un restablecimiento controlado,

- El volumen de origen activo se pone fuera de línea,
- Se toma una instantánea,
- Se completa el ciclo de réplica,
- Se activa la instantánea de datos recién tomada,
- Y el volumen de origen pasa a estar activo para su montaje.

Cuando se inicia un restablecimiento, la relación de réplica se invierte de nuevo. El volumen de origen se restaura como el volumen de origen y el volumen de destino vuelve a ser el volumen de destino, tal como indica el **Nombre de LUN** seguido de **REP**.

Los restablecimientos se inician en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Pulse el LUN activo ("destino").
2. En la parte superior derecha, pulse **Réplica** y pulse **Acciones**.
3. Seleccione **Restablecimiento**. >Recibirá un mensaje en la parte superior de la página que indicará que la migración tras error está en curso. También aparecerá un icono junto al volumen en **{{site.data.keyword.blockstorageshort}}** que indicará que hay una transacción activa en curso. Al pasar el ratón sobre el icono se abre una ventana que muestra la transacción. El icono desaparecerá una vez completada la transacción. Durante el proceso de retrotracción, las acciones relacionadas con la configuración son de solo lectura. No puede editar ninguna planificación de instantáneas ni cambiar el espacio de instantáneas. El suceso se registra en el historial de réplicas.
4. En la parte superior derecha, pulse el enlace **Ver todo {{site.data.keyword.blockstorageshort}}**.
5. Pulse el LUN activo ("origen").
6. Monte y conecte el volumen de almacenamiento al host. Pulse [aquí](provisioning-block_storage.html) para obtener instrucciones.


## Visualización del historial de réplica

El historial de réplica se puede visualizar en **Registro de auditoría** en el separador **Cuenta**, en **Gestionar**. Tanto el volumen primario como el de réplica muestran el mismo historial de réplicas. El historial incluye:

- Tipo de réplica (migración tras error o restablecimiento)
- Cuándo se inició
- Instantánea utilizada para la réplica
- Tamaño de la réplica
- Cuando se ha completado


## Creación de un duplicado de una réplica

Puede crear un duplicado de un {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}} existente. El volumen duplicado hereda la capacidad y las opciones de rendimiento del LUN/volumen original de forma predeterminada y tiene una copia de los datos hasta el momento de la instantánea.

Los duplicados pueden crearse a partir de volúmenes primarios y de réplica. El nuevo duplicado se crea en el mismo centro de datos que el volumen original. Si crea un duplicado a partir de un volumen de réplica, el nuevo volumen se crea en el mismo centro de datos que el volumen de réplica.

Se puede acceder a los volúmenes duplicados mediante un host para lectura/escritura siempre y cuando el almacenamiento esté suministrado. Sin embargo, no se permiten instantáneas ni réplicas hasta que se completa la copia de datos del original en el duplicado.

Para obtener más información, consulte [Creación de un volumen de bloque duplicado](how-to-create-duplicate-volume.html).


## Cancelación de una réplica existente

Puede cancelar una réplica de inmediato o en la fecha de aniversario, lo que hace que finalice la facturación. La réplica se puede cancelar desde el separador **Primario** o **Réplica**.

1. Pulse el volumen en la página de **{{site.data.keyword.blockstorageshort}}**.
2. Pulse **Acciones** en el separador **Primario** o **Réplica**.
3. Seleccione **Cancelar réplica**.
4. Seleccione cuándo desea cancelarla. Elija **Inmediatamente** o **Fecha de aniversario**, y pulse **Continuar**.
5. Pulse **Reconozco que a causa de la cancelación, es posible que se pierdan datos** y pulse **Cancelar réplica**.


## Cancelación de la réplica cuando el volumen primario se cancela

Cuando se cancela un volumen primario, la planificación de réplica y el volumen del centro de datos de réplica se suprimen. Las réplicas se cancelan desde la página de {{site.data.keyword.blockstorageshort}}.

 1. Marque el volumen en la página de **{{site.data.keyword.blockstorageshort}}**.
 2. Pulse **Acciones** y seleccione **Cancelar {{site.data.keyword.blockstorageshort}}**.
 3. Seleccione cuándo desea cancelarla. Elija **Inmediatamente** o **Fecha de aniversario**, y pulse **Continuar**.
 4. Pulse **Reconozco que a causa de la cancelación, es posible que se pierdan datos** y pulse **Cancelar**.
