---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# Preguntas más frecuentes

## ¿Cuántas instancias pueden compartir el uso de un volumen de {{site.data.keyword.blockstorageshort}}?
{: faq}

El límite predeterminado de número de autorizaciones por volumen de bloque es de ocho. Esto significa que se pueden autorizar hasta 8 hosts para acceder al LUN de almacenamiento en bloque. Para solicitar un aumento del límite, póngase en contacto con su representante de ventas.

## ¿Cuántos volúmenes se pueden pedir?
{: faq}

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el límite de volumen, póngase en contacto con su representante de ventas. Para obtener más información, consulte [Gestión de los límites de almacenamiento](managing-storage-limits.html).

## ¿Cuántos volúmenes de {{site.data.keyword.blockstorageshort}} se pueden montar en un host?
{: faq}

Eso depende de lo que el sistema operativo del host pueda manejar, no se trata de una limitación de {{site.data.keyword.BluSoftlayer_full}}. Consulte la documentación de su sistema operativo para conocer los límites en la cantidad de volúmenes que se pueden montar.

## ¿Qué versión de Windows debo elegir para mi LUN de almacenamiento en bloque?
{: faq}

Después de crear un LUN, debe especificar el tipo de SO. El tipo de SO se debe basar en el sistema operativo que utilizan los hosts que acceden al LUN. El tipo de SO no se puede modificar después de que se cree el LUN. El tamaño real del LUN puede variar ligeramente en función del tipo de SO del LUN.

**Windows 2008+**
- El LUN almacena datos de Windows para Windows 2008 y versiones posteriores. Utilice esta opción de SO si el sistema operativo del host es Windows Server 2008, Windows Server 2012 o Windows Server 2016. Se da soporte a los modelos de particionamiento MBR y GPT.

**Windows 2003**
- El LUN almacena un tipo de disco sin formato en un disco Windows de una sola partición que utiliza el estilo de particionamiento MBR (registro de arranque maestro). Utilice esta opción solo si el sistema operativo del host es Windows 2000 Server, Windows XP o Windows Server 2003 que utiliza el método de particionamiento MBR.

**Windows GPT**
-  El LUN almacena los datos de Windows utilizando el estilo de particionamiento GPT (tipo de partición GUID). Utilice esta opción si desea utilizar el método de particionamiento GPT y el host es capaz de utilizarlo. Windows Server 2003, Service Pack 1 y posteriores pueden utilizar el método de particionamiento GPT y todas las versiones de 64 bits de Windows le dan soporte.

## ¿Se aplica el límite de IOPS asignado por instancia o por volumen?
{: faq}

IOPS se aplica a nivel de volumen. Dicho de otro modo, dos hosts conectados a un volumen con 6000 IOPS comparten estos 6000 IOPS.

## Medición de IOPS
{: faq}

IOPS se mide en función de un perfil de carga de bloques de 16 KB con 50 % de lecturas y 50 % de escrituras aleatorias. Las cargas de trabajo que difieren de este perfil pueden experimentar un rendimiento inferior.

## ¿Qué ocurre cuando se utiliza un tamaño de bloque más pequeño para medir el rendimiento?
{: faq}

El máximo de IOPS todavía se puede obtener cuando se utilizan tamaños de bloque más pequeños. Sin embargo, el rendimiento se vuelve más pequeño. Por ejemplo, un volumen con 6000 IOPS tendrá el siguiente rendimiento en varios tamaños de bloque:

- 16 KB * 6000 IOPS == ~93,75 MB/seg.
- 8 KB * 6000 IOPS == ~46,88 MB/seg.
- 4 KB * 6000 IOPS == ~23,44 MB/seg.

## ¿El volumen debe calentarse previamente para alcanzar el rendimiento esperado?
{: faq}

No es necesario ningún calentamiento previo. Puede observar el rendimiento especificado inmediatamente después de suministrar el volumen.

## ¿Se puede lograr más rendimiento mediante una conexión Ethernet más rápida?
{: faq}

Los límites de rendimiento están establecidos por LUN, por lo que utilizar una conexión de Ethernet más rápida no aumenta el límite establecido. Sin embargo, con una conexión Ethernet más lenta, el ancho de banda sí que puede ser un posible cuello de botella.

## ¿Los cortafuegos y los grupos de seguridad afectan al rendimiento?
{: faq}

Es mejor ejecutar el tráfico de almacenamiento en una VLAN, que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software aumenta la latencia y afecta negativamente al rendimiento del almacenamiento.

## ¿Qué latencia se puede esperar de la publicación {{site.data.keyword.blockstorageshort}}?   
{: faq}

La latencia de destino en el almacenamiento es de <1 ms. El almacenamiento está conectado a instancias de cálculo en una red compartida, por lo que la latencia de rendimiento exacta depende del tráfico de red durante la operación.

## ¿Por qué se puede solicitar {{site.data.keyword.blockstorageshort}} con un nivel 10 de IOPS/GB de Resistencia en algunos centros de datos y no en otros?
{: faq}

El nivel 10 de IOPS/GB de tipo Resistencia {{site.data.keyword.blockstorageshort}} solo está disponible en centros de datos seleccionados, y se están añadiendo nuevos centros de datos gradualmente. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](new-ibm-block-and-file-storage-location-and-features.html).

## ¿Cómo podemos saber cuáles de los volúmenes de {{site.data.keyword.blockstorageshort}} están cifrados?
{: faq}

Al consultar la lista de {{site.data.keyword.blockstorageshort}} en el [{{site.data.keyword.slportal}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){:new_window}, puede ver un icono de bloqueo junto al nombre de volumen de los LUN que están cifradas.

## ¿Cómo sabemos cuándo estamos suministrando {{site.data.keyword.blockstorageshort}} en un centro de datos actualizado?
{: faq}

Al solicitar {{site.data.keyword.blockstorageshort}}, todos los centros de datos actualizados se marcan con un asterisco (`*`) en el formulario de pedido y una indicación de que está a punto de suministrar almacenamiento con cifrado. Una vez suministrado el almacenamiento, puede ver un icono en la lista de almacenamiento que muestra que dicho almacenamiento está cifrado. Todos los volúmenes cifrados y LUN se suministran únicamente en centros de datos actualizados. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](new-ibm-block-and-file-storage-location-and-features.html).

## Si tenemos {{site.data.keyword.blockstorageshort}} no cifrado en un centro de datos actualizado recientemente, ¿podemos cifrar dicho {{site.data.keyword.blockstorageshort}}?
{: faq}

{{site.data.keyword.blockstorageshort}} suministrado antes de la actualización del centro de datos no se puede cifrar.
El nuevo {{site.data.keyword.blockstorageshort}} que se suministra en centros de datos actualizados se cifra automáticamente. No hay que elegir ningún valor de cifrado; es automático.
Los datos que residen en almacenamiento no cifrado en un centro de datos actualizado se pueden cifrar creando un nuevo LUN de bloque para posteriormente copiar los datos al nuevo LUN cifrado con migración basada en host. Pulse [aquí](migrate-block-storage-encrypted-block-storage.html) para obtener instrucciones.

## ¿{{site.data.keyword.blockstorageshort}} da soporte a la reserva persistente SCSI-3 para implementar una barrera de E/S para Db2 pureScale?
{: faq}

Sí, {{site.data.keyword.blockstorageshort}} da soporte a las reservas persistentes SCSI-2 y SCSI-3.

## ¿Qué ocurre con los datos cuando se suprimen LUN de {{site.data.keyword.blockstorageshort}}?
{: faq}

{{site.data.keyword.blockstoragefull}} presenta volúmenes en bloque a los clientes en almacenamiento físico que se borra antes de cualquier reutilización. Los clientes con requisitos especiales de cumplimiento, como las directrices NIST 800-88 para el saneamiento de datos, deben realizar el procedimiento de saneamiento de datos antes de suprimir su almacenamiento.

## ¿Qué pasa a las unidades que quedan fuera de servicio en el centro de datos de nube?
{: faq}

Cuando las unidades quedan fuera de servicio, IBM las destruye antes de desecharlas. Las unidades se vuelven inutilizables. Los datos escritos en dicha unidad se vuelven inaccesibles.
