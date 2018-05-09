---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-25"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Trabajar con réplicas

La réplica utiliza una de sus planificaciones de instantáneas para copiar automáticamente las instantáneas a un volumen de destino de un centro de datos remoto. Las copias se pueden recuperar en el sitio remoto en caso de corrupción de datos o de suceso catastrófico.

Las réplicas le permiten

- Recuperarse rápidamente de fallos del sitio y otros desastres realizando la migración al volumen de destino,
- Realizando la migración tras error a un punto específico en el tiempo en la copia de recuperación tras desastre.

Antes de poder replicar, debe crear una planificación de instantáneas. Cuando realiza la migración tras error, está “cambiando el interruptor” de su volumen de almacenamiento del centro de datos primario al volumen de destino del centro de datos remoto. Por ejemplo, su centro de datos primario es Londres y el centro de datos secundario es Amsterdam. Si se produce un suceso de error, realizaría la migración a Amsterdam – conectando al ahora volumen primario desde una instancia de cálculo en Amsterdam. Cuando su volumen de Londres se haya reparado, se realiza una instantánea del volumen de Amsterdam para volver a Londres y al volumen primario de nuevo desde una instancia de cálculo de Londres.


**Nota:** A menos que se indique lo contrario, los pasos son los mismos para {{site.data.keyword.blockstoragefull}} y File Storage.

## ¿Cómo determino el centro de datos remoto para mi volumen de almacenamiento replicado?

Los centros de datos de {{site.data.keyword.BluSoftlayer_full}} de todo el mundo se han emparejado en combinaciones de centros primarios y remotos.
Consulte la Tabla 1 para ver una lista completa de disponibilidad de centros de datos y destinos de réplica.
Tenga en cuenta que algunas ciudades, como Dallas, San José, Washington, D.C. y Amsterdam tienen varios centros de datos.


<table cellpadding="1" cellspacing="1">
	<caption>Tabla 1</caption>
	<tbody>
		<tr>
			<td><strong>EE.UU. 1</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>EE.UU. 2</strong></td>
			<td><strong>Latinoamérica/Sudamérica</strong></td>
			<td><strong>Canadá</strong></td>
			<td><strong>Europa</strong></td>
			<td><strong>Asia-Pacífico</strong></td>
			<td><strong>Australia</strong></td>
		</tr>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br /><br />
			</td>
			<td>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br /><br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				FRA04<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%"><p><sup><img src="/images/numberone.png" alt="1" /></sup>Los centros de datos de esta región NO disponen de almacenamiento cifrado.<br /><strong>Nota</strong>: Los centros de datos con almacenamiento cifrado <strong>no pueden</strong> iniciar la réplica con centros de datos no cifrados como destinos de réplica.</p>
			</td>
		</tr>
	</tbody>
</table>


## ¿Cómo creo una réplica inicial?

Las réplicas trabajan sobre una planificación de instantáneas. Primero debe tener un espacio de instantáneas y una planificación de instantáneas configuradas para el volumen de origen antes de poder replicar. Recibirá solicitudes informándole sobre las necesidades de espacio que deben adquirirse o una configuración de planificación si intenta configurar la réplica y falta alguno de estos componentes. Las réplicas se gestionan en Almacenamiento, {{site.data.keyword.blockstorageshort}} o File Storage en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Pulse en el volumen de almacenamiento.
2. Pulse el separador **Réplica** y, a continuación, el enlace **Adquirir una réplica**.
Seleccione la planificación de instantáneas existente que quiera que sigan sus réplicas. La lista contendrá todas las planificaciones de instantáneas activas. <br />
  **Nota:** Solo puede seleccionar una planificación, incluso si tiene una combinación de por hora, a diario y mensual.  Todas las instantáneas capturadas desde el ciclo de réplica anterior se replicarán independientemente de la planificación que las originó.<br />
  **Nota:** Si no tiene configuradas las instantáneas, se le solicitará que lo haga para poder solicitar una réplica. Consulte [Trabajar con instantáneas](snapshots.html) para obtener más detalles.
3. Pulse la flecha desplegable **Ubicación** y seleccione el centro de datos que será su sitio de recuperación tras desastre.
4. Pulse **Continuar**.
5. Especifique un **Código promocional** si tiene uno y pulse **Recalcular**. Los otros campos del recuadro de diálogo tienen valores predeterminados.
6. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**.


## ¿Cómo edito una réplica existente?

Puede editar la planificación de la réplica y cambiar el espacio de réplica desde el separador **Primario** o **Réplica** de **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## ¿Cómo edito una planificación de réplica?

Realmente está cambiando una planificación de instantáneas porque la planificación de réplica se basa en una planificación de instantáneas existente. Para cambiar la planificación de réplica, por ejemplo de por hora a semanalmente, debe cancelar la planificación de réplica y configurar una nueva.

Los cambios en la planificación pueden realizarse en el separador Primario o Réplica.

1. Pulse el menú desplegable **Acciones** desde el separador **Primario** o **Réplica**.
2. Seleccione **Editar planificación de instantáneas**.
3. Consulte en el marco Instantánea, en Planificación, para determinar qué planificación está utilizando para la réplica. Realice los cambios en la planificación que se utiliza para la réplica; por ejemplo, si la planificación de réplica es **A diario**, puede cambiar la hora del día en que tendrá lugar la réplica.
4. Pulse **Guardar**.


## ¿Cómo cambio el espacio de réplica?

El espacio de instantáneas primario y el espacio de réplica deben ser el mismo. Si cambia en espacio en el separador **Primario** o **Réplica**, se añadirá automáticamente espacio a los centros de datos de origen y de destino. Tenga en cuenta que aumentar el espacio de instantáneas desencadenará una actualización de réplica inmediata.

Pulse el menú desplegable **Acciones** desde el separador **Primario** o **Réplica**.
Seleccione **Añadir más espacio de instantáneas**.
Seleccione el tamaño de almacenamiento de la lista y pulse **Continuar**.
Especifique un **Código promocional** si tiene uno y pulse **Recalcular**. Los otros campos del recuadro de diálogo tienen valores predeterminados.
Marque el recuadro de selección He leído el Acuerdo de Servicio Maestro… y pulse Realizar pedido.


## ¿Cómo visualizo mis volúmenes de réplica en la lista de volúmenes?

Puede visualizar los volúmenes de réplica desde la página de {{site.data.keyword.blockstorageshort}}, en **Almacenamiento > {{site.data.keyword.blockstorageshort}}**. El Nombre de LUN tendrá el nombre del volumen primario seguido por REP. El Tipo de Resistencia (Rendimiento) – Réplica, la dirección de destino no está disponible porque el volumen de réplica no está montado en el centro de datos de réplicas, y el estado es inactivo.



## ¿Cómo visualizo los detalles de un volumen replicado en el centro de datos de réplicas?

Puede visualizar los detalles de un volumen de réplica en el separador **Réplica**, en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**. Otra opción es seleccionar el volumen de réplica desde la página de **{{site.data.keyword.blockstorageshort}}** y pulsar el separador **Réplica**.



## ¿Cómo especifico autorizaciones de host antes de realizar la migración tras error a un centro de datos secundario?

Los hosts y volúmenes autorizados deben estar en el mismo centro de datos. No puede tener un volumen de réplica en Londres y el host en Amsterdam; ambos deben estar en Londres o en Amsterdam.

1. Pulse sobre el volumen de origen o destino en la página de **{{site.data.keyword.blockstorageshort}}**.
2. Pulse el separador **Réplica**.
3. Desplácese hasta el marco **Autorizar hosts** y pulse el enlace **Autorizar hosts** en la parte derecha de la pantalla.
4. Marque el host que se va a autorizar para las réplicas. Para seleccionar varios hosts, utilice la tecla CTRL y pulse los hosts aplicables.
5. Pulse **Enviar**. Si no tiene ningún host, el recuadro de diálogo le ofrecerá la opción de comprar recursos de cálculo en el mismo centro de datos o puede pulsar **Cerrar**.


## ¿Cómo aumento el espacio de instantáneas en el centro de datos de réplicas cuando aumento el espacio del centro de datos primario?

Los tamaños de volumen deben ser los mismos para sus volúmenes de almacenamiento primario y de réplica, uno no puede ser más grande que el otro. Cuando aumenta el espacio de instantáneas para su volumen primario, el espacio de réplica se aumenta automáticamente. Tenga en cuenta que aumentar el espacio de instantáneas desencadenará una actualización de réplica inmediata. El aumento en ambos volúmenes se mostrará como elementos de línea en su factura, y se prorratearán en caso necesario.

Pulse [aquí](snapshots.html) para obtener información sobre cómo aumentar el espacio de instantáneas.



## ¿Cómo inicio una migración tras error desde un volumen a su réplica?

Si se produce un fallo, la acción **Migración tras error** le permite iniciar la migración tras error a su volumen de destino. El volumen de destino pasa a estado activo, se activa la última instantánea replicada correctamente y el volumen se activa para el montaje. Los datos escritos en el volumen de origen desde el ciclo de réplica anterior se destruirán. Tenga en cuenta que una vez iniciada la migración tras error, la **relación de réplica se invierte**. El volumen de destino es ahora el volumen de origen, y el volumen de origen anterior pasa a ser el destino, como indica el **Nombre de LUN** seguido de **REP**.

Las migraciones tras error se inician en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Antes de continuar con este proceso, se recomienda desconectar el volumen. De lo contrario, se producirá una pérdida de datos o su corrupción.**

1. Pulse en el LUN activo (“origen”).
2. Pulse el separador **Réplica** y pulse el enlace **Acciones** en la esquina superior derecha.
3. Seleccione Migración tras error.
   Recibirá un mensaje en la parte superior de la página indicando que la migración está en curso. Además, aparecerá un icono junto al volumen en el **{{site.data.keyword.blockstorageshort}}** indicando que se está produciendo la transacción activa. Al pasar el ratón sobre el icono se abre un diálogo que indica la transacción. El icono desaparecerá una vez completada la transacción. Durante el proceso de migración tras error, las acciones relacionadas con la configuración son de solo lectura, no podrá editar ninguna planificación de instantáneas, cambiar el espacio de instantáneas, etc. El suceso se registra en el historial de réplicas.
   Otro mensaje le informará cuando el volumen de destino esté activado. El Nombre de LUN del volumen de origen original irá seguido de REP y su estado será inactivo.
4. Pulse el enlace **Ver todo el almacenamiento** (**{{site.data.keyword.blockstorageshort}}**) en la esquina superior derecha.
5. Pulse sobre el LUN activo (anteriormente volumen de destino). Este volumen ahora tendrá un estado **Activo**.
6. Monte y conecte el volumen de almacenamiento al host. Pulse [aquí](provisioning-block_storage.html) para obtener instrucciones.


## ¿Cómo inicio un restablecimiento de un volumen a su réplica?

Una vez reparado el volumen de origen original, la acción **Restablecimiento** le permite iniciar un restablecimiento controlado a su volumen de origen original. En un restablecimiento controlado,

- El volumen de origen activo se pone fuera de línea;
- Se realiza una instantánea;
- Se completa el ciclo de réplica;
- La instantánea de datos recién realizada se activa;
- Y el volumen de origen se activa para el montaje.

Tenga en cuenta que una vez iniciado el restablecimiento, la **relación de réplica se vuelve a invertir**. El volumen de origen se restaura como el volumen de origen y el volumen de destino pasa a ser, una vez más, el volumen de destino, como indica el **Nombre de LUN** seguido de **REP**.

Los restablecimientos se inician en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** desde el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Pulse en el LUN de Resistencia activo (“destino”).
2. Pulse el separador **Réplica** y pulse el enlace **Acciones** en la esquina superior derecha.
3. Seleccione **Restablecimiento**.
   Recibirá un mensaje en la parte superior de la página indicando que la migración está en curso. Además, aparecerá un icono junto al volumen en el **{{site.data.keyword.blockstorageshort}}** indicando que se está produciendo la transacción activa. Al pasar el ratón sobre el icono se abre un diálogo que indica la transacción. El icono desaparecerá una vez completada la transacción. Durante el proceso de restablecimiento, las acciones relacionadas con la configuración son de solo lectura, no podrá editar ninguna planificación de instantáneas, cambiar el espacio de instantáneas, etc. El suceso se registra en el historial de réplicas.
   Otro mensaje le informará cuando el volumen de origen esté activado. El volumen de destino ahora tendrá un estado Inactivo.
4. Pulse el enlace **Ver todo el almacenamiento** (**{{site.data.keyword.blockstorageshort}}**) en la esquina superior derecha.
5. Pulse en el LUN de Resistencia activo (origen). Este volumen ahora tendrá un estado **Activo**.
6. Monte y conecte el volumen de almacenamiento al host. Pulse [aquí](provisioning-block_storage.html) para obtener instrucciones.


## ¿Cómo consulto mi historial de réplicas?

El historial de réplicas se visualiza en el **Registro de auditoría** a través del separador **Cuenta**, en **Gestionar**. Tanto el volumen primario como el de réplica mostrarán el mismo historial de réplica, que incluye:

- Tipo de réplica (migración tras error o restablecimiento)
- Cuando se ha iniciado,
- Instantánea utilizada para la réplica
- Tamaño de la réplica
- Cuando se ha completado


## ¿Cómo cancelo una réplica existente?

La cancelación puede realizarse inmediatamente o en la fecha de aniversario, lo que finaliza la facturación. La réplica se puede cancelar desde el separador **Primario** o **Réplica**.

1. Pulse en el volumen desde la página de **{{site.data.keyword.blockstorageshort}}**.
2. Pulse el menú desplegable **Acciones** desde el separador **Primario** o **Réplica**.
3. Seleccione **Cancelar réplica**.
4. Seleccione cuándo cancelar, **Inmediatamente** o **Fecha de aniversario** y pulse **Continuar**.
5. Marque el recuadro de selección **Reconozco que a causa de la cancelación, es posible que se pierdan datos** y pulse **Cancelar réplica**.


## ¿Cómo cancelo la réplica cuando se cancela el volumen primario?

Cuando se cancela un volumen primario, la planificación de réplica y el volumen del centro de datos de réplica se suprimen. Las réplicas se cancelan desde la página de {{site.data.keyword.blockstorageshort}}.

 1. Marque el volumen en la página de **{{site.data.keyword.blockstorageshort}}**.
 2. Pulse el menú desplegable **Acciones** y seleccione **Cancelar {{site.data.keyword.blockstorageshort}}**.
 3. Seleccione cuándo cancelar el volumen, **Inmediatamente** o **Fecha de aniversario** y pulse **Continuar**.
 4. Marque el recuadro de selección **Reconozco que a causa de la cancelación, es posible que se pierdan datos** y pulse **Cancelar**.
