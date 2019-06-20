---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, secondary storage, replication, duplicate volume, synchronized volumes, primary volume, secondary volume, DR, disaster recovery

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Réplica de datos
{: #replication}

La réplica utiliza una de sus planificaciones de instantáneas para copiar automáticamente las instantáneas a un volumen de destino de un centro de datos remoto. Las copias se pueden recuperar en el sitio remoto si se produce un suceso catastrófico o los datos resultan dañados.

La réplica mantiene sus datos sincronizados entre dos ubicaciones distintas. Si desea clonar su volumen y utilizarlo independientemente del volumen original, consulte [Creación de un volumen de bloque duplicado](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).
{:tip}

Antes de poder replicar, debe crear una planificación de instantáneas.
{:important}


## Determinación del centro de datos remoto para mi volumen de almacenamiento replicado

Los centros de datos de {{site.data.keyword.cloud}} están emparejados en combinaciones de centros primarios y remotos.
Consulte la Tabla 1 para ver la lista completa de disponibilidad de centros de datos y destinos de réplica.

| EE.UU. 1 | EE.UU. 2 | Latinoamérica | Canadá  | Europa  | Asia Pacífico  | Australia  |
|-----|-----|-----|-----|-----|-----|-----|
| DAL01<br />DAL05<br />DAL06<br />HOU02<br />SJC01<br />WDC01 | SJC03<br />SJC04<br />WDC04<br />WDC06<br />WDC07<br />DAL09<br />DAL10<br />DAL12<br />DAL13 | MEX01<br />SAO01 | TOR01<br />MON01 | AMS01<br />AMS03<br />FRA02<br />FRA04<br />FRA05<br />LON02<br />LON04<br />LON05<br />LON06<br />OSL01<br />PAR01<br />MIL01 | HKG02<br />TOK02<br />TOK04<br />TOK05<br />SNG01<br />SEO01<br />CHE01 | SYD01<br />SYD04<br />SYD05<br />MEL01 |
{: caption="Tabla 1: esta tabla muestra la lista completa de centros de datos con funciones mejoradas en cada región. Cada región está en una columna separada. Algunas ciudades, como Dallas, San José, Washington DC, Ámsterdam, Frankfurt, Londres y Sídney, tienen varios centros de datos. Los centros de datos de la región EE.UU. 1 NO tienen almacenamiento mejorado. Los hosts de los centros de datos con funciones mejoradas de almacenamiento no pueden iniciar la réplica con destinos de réplica en los centros de datos de EE.UU. 1." caption-side="top"}

## Creación de la réplica inicial

Las réplicas se basan en una planificación de réplica. Primero debe tener un espacio de instantáneas y una planificación de instantáneas para el volumen de origen antes de poder replicar. Si intenta configurar la réplica y uno o el otro no está en su lugar, se le solicitará que compre más espacio o que establezca una planificación. Las réplicas se gestionan bajo **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

1. Pulse el volumen de almacenamiento.
2. Pulse **Réplica** y pulse **Adquirir una réplica**.
3. Seleccione la planificación de instantáneas existente que quiera que siga su réplica. La lista contiene todas las planificaciones de instantáneas activas. <br />
   Solo puede seleccionar una planificación, incluso si tiene una combinación de por hora, a diario y mensual. Todas las instantáneas capturadas desde el ciclo de réplica anterior se replicarán, independientemente de la planificación que las originó.<br />Si no tiene configuradas las instantáneas, se le solicitará que lo haga para poder solicitar una réplica. Consulte [Trabajar con instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots) para obtener más detalles.
   {:important}
3. Pulse **Ubicación** y seleccione el centro de datos que es su sitio de recuperación tras desastre.
4. Pulse **Continuar**.
5. Especifique un **Código promocional** si tiene uno y pulse **Recalcular**. Los otros campos de la ventana se completan de forma predeterminada.

   Los descuentos se aplican cuando se procesa el pedido.
   {:note}
6. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro…** y pulse **Realizar pedido**.


## Edición de una réplica existente

Puede editar la planificación de réplica y cambiar el espacio de réplica desde el separador **Primario** o **Réplica** de **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** en la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.


## Edición de la planificación de réplica

La planificación de réplica se basa en una planificación de instantáneas existente. Para cambiar la planificación de réplica de por hora a diaria o semanal, o viceversa, debe cancelar el volumen de réplica y configurar uno nueva.

Sin embargo, si desea cambiar la hora del día a la que se realiza la réplica **diaria**, puede ajustar la planificación existente en el separador Primario o Réplica.

1. Pulse **Acciones** en el separador **Primario** o **Réplica**.
2. Seleccione **Editar planificación de instantáneas**.
3. Busque en el marco **Instantánea** bajo **Planificar** para determinar qué planificación está utilizando para la réplica. Cambie la planificación que desea.
4. Pulse **Guardar**.


## Cambio del espacio de réplica

El espacio de instantáneas primario y el espacio de réplica deben ser el mismo. Si cambia el espacio en el separador **Primario** o **Réplica**, añade automáticamente espacio a los centros de datos de origen y de destino. El aumento del espacio de instantáneas desencadena también una actualización de réplica inmediata.

1. Pulse **Acciones** en el separador **Primario** o **Réplica**.
2. Seleccione **Añadir más espacio de instantáneas**.
3. Seleccione el tamaño de almacenamiento de la lista y pulse **Continuar**.
4. Especifique un **Código promocional** si tiene uno y pulse **Recalcular**. Los otros campos del recuadro de diálogo contienen información de forma predeterminada.
5. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro…** y pulse **Realizar pedido**.


## Visualización de los volúmenes de réplica en la lista de volúmenes

Puede visualizar los volúmenes de réplica en la página de {{site.data.keyword.blockstorageshort}}, en **Almacenamiento > {{site.data.keyword.blockstorageshort}}**. El **Nombre de LUN** muestra el nombre del volumen primario seguido de REP. El **Tipo** de réplica puede ser Resistente o Rendimiento. La **Dirección de destino** es N/D porque el volumen de réplica no está montado en el centro de datos y el **Estado** es Inactivo.


## Visualización de los detalles de un volumen replicado en el centro de datos de réplicas

Puede visualizar los detalles de un volumen de réplica en el separador **Réplica**, en **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}**. Otra opción es seleccionar el volumen de réplica desde la página de **{{site.data.keyword.blockstorageshort}}** y pulsar el separador **Réplica**.


## Aumento del espacio de instantáneas en el centro de datos de réplica cuando el espacio de instantáneas se incrementa en el centro de datos primario

Los tamaños de volumen deben ser los mismos para sus volúmenes de almacenamiento primario y de réplica. No puede haber uno mayor que otro. Cuando aumenta el espacio de instantáneas para su volumen primario, el espacio de réplica se aumenta automáticamente. El aumento del espacio de instantáneas desencadena una actualización de réplica inmediata. El aumento en ambos volúmenes se muestra como elementos de línea en su factura, y se prorratea en caso necesario.

Para obtener más información sobre cómo aumentar el espacio de instantáneas, consulte [Solicitud de instantáneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:tip}


## Visualización del historial de réplica

El historial de réplica se puede visualizar en **Registro de auditoría** en el separador **Cuenta**, en **Gestionar**. Tanto el volumen primario como el de réplica muestran el mismo historial de réplicas. El historial incluye los siguientes elementos.

- El tipo de réplica (migración tras error o restablecimiento).
- Cuándo se inició.
- La instantánea que se ha utilizado para la réplica.
- El tamaño de la réplica.
- La hora a la que se ha completado la réplica.


## Creación de un duplicado de una réplica

Puede crear un duplicado de un {{site.data.keyword.cloud}} {{site.data.keyword.blockstoragefull}} existente. El volumen duplicado hereda la capacidad y las opciones de rendimiento del volumen original de forma predeterminada y tiene una copia de los datos hasta el momento de la instantánea.

Los duplicados pueden crearse a partir de volúmenes primarios y de réplica. El nuevo duplicado se crea en el mismo centro de datos que el volumen original. Si crea un duplicado a partir de un volumen de réplica, el nuevo volumen se crea en el mismo centro de datos que el volumen de réplica.

Se puede acceder a los volúmenes duplicados mediante un host para lectura/escritura siempre y cuando el almacenamiento esté suministrado. Sin embargo, no se permiten instantáneas ni réplicas hasta que se completa la copia de datos del original en el duplicado.

Para obtener más información, consulte [Creación de un volumen de bloque duplicado](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Uso de réplicas para migración tras error en caso de desastre

Cuando realiza la migración tras error, está "cambiando el conmutador" de su volumen de almacenamiento del centro de datos primario al volumen de destino del centro de datos remoto. Por ejemplo, su centro de datos primario es Londres y el centro de datos secundario es Ámsterdam. Si se produjera un suceso de error, debería realizar la migración a Ámsterdam, conectando al ahora volumen primario desde una instancia de cálculo en Ámsterdam. Cuando su volumen de Londres se haya reparado, se realizará una instantánea del volumen de Ámsterdam para volver a Londres y al volumen primario de nuevo desde una instancia de cálculo de Londres.

* Si la ubicación primaria está en peligro inminente o se ha visto seriamente afectada, consulte [Migración tras error con un volumen primario accesible](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-accessible).
* Si la ubicación primaria está inactiva, consulte [Migración tras error con un volumen primario inaccesible](/docs/infrastructure/BlockStorage?topic=BlockStorage-dr-inaccessible).


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

 Puede esperar que la LUN siga resultando visible en la lista de almacenamiento durante al menos 24 horas (cancelación inmediata) o hasta la fecha de aniversario. Algunas características dejarán de estar disponibles, pero el volumen sigue resultando visible hasta que se reclame. Sin embargo, la facturación se detiene inmediatamente después de que pulse Suprimir/Cancelar.

 La activación de réplicas puede bloquear la reclamación del volumen de almacenamiento. Asegúrese de que el volumen ya no esté montado, de que las autorizaciones de host se hayan revocado y de que la réplica se haya cancelado antes de intentar cancelar el volumen original.


## Mandatos relacionados con la réplica en SLCLI
{: #clicommands}

* Listar los centros de datos de réplica disponibles para un volumen específico.
  ```
  # slcli block replica-locations --help
Uso: slcli block replica-locations [OPCIONES] ID_VOLUMEN

  Opciones:
  --sortby TEXTO  Columna por la que se debe ordenar
  --columns TEXTO Columnas que se deben visualizar. Opciones: ID, Long Name, Short Name
  -h, --help      Mostrar este mensaje y salir.
  ```

* Pedir un volumen de réplica de almacenamiento en bloque.
  ```
  # slcli block replica-order --help
  Uso: slcli block replica-order [OPCIONES] ID_VOLUMEN

  Opciones:
  -s, --snapshot-schedule [INTERVAL|HOURLY|DAILY|WEEKLY]
                                  Planificación de instantáneas que usar en la réplica,
                                  (INTERVAL | HOURLY | DAILY | WEEKLY)
                                  [necesario]
  -l, --location TEXTO            Nombre corto del centro de datos para el replicante
                                  (por ejemplo, dal09)  [necesario]
  --tier [0.25|2|4|10]            Nivel de almacenamiento resistente (IOPS por GB)
                                  del volumen primario para el que se pide un replicante
                                  [opcional]
  --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                  Tipo de sistema operativo (por ejemplo: LINUX) del
                                  volumen primario para el que se pide una réplica
                                  [opcional]
  -h, --help                      Mostrar este mensaje y salir.
  ```

* Listar volúmenes existentes replicantes para un volumen de bloque.
  ```
  # slcli block replica-partners --help
  Uso: slcli block replica-partners [OPCIONES] ID_VOLUMEN

  Opciones:
  --sortby TEXTO  Columna por la que se debe ordenar
  --columns TEXTO Columnas que se deben visualizar. Opciones: ID, Username, Account ID,
                  Capacity (GB), Hardware ID, Guest ID, Host ID
  -h, --help      Mostrar este mensaje y salir.
  ```

* Realizar la migración tras error de un volumen de bloques a un volumen replicante específico.
  ```
  # slcli block replica-failover --help
  Uso: slcli block replica-failover [OPCIONES] ID_VOLUMEN

  Opciones:
  --replicant-id TEXTO ID del volumen replicante
  --immediate          Migrar tras error al replicante de inmediato.
  -h, --help      Mostrar este mensaje y salir.
  ```

* Restablecer un volumen de bloques desde un volumen replicante específico.
  ```
  # slcli block replica-failback --help
  Uso: slcli block replica-failback [OPCIONES] ID_VOLUMEN

  Opciones:
  --replicant-id TEXTO ID del volumen replicante
  -h, --help           Mostrar este mensaje y salir.
  ```
