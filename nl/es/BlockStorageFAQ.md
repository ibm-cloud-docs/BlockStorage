---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-18"

---
{:new_window: target="_blank"}

# Preguntas más frecuentes

## ¿Cuántas instancias pueden compartir el uso de un volumen de {{site.data.keyword.blockstorageshort}}?
El límite predeterminado de número de autorizaciones por volumen de bloque es de ocho. Esto significa que se pueden autorizar hasta 8 hosts para acceder al LUN de almacenamiento en bloque. Para solicitar un aumento del límite, póngase en contacto con su representante de ventas.

## ¿Cuántos volúmenes se pueden pedir?
De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el límite de volumen, póngase en contacto con su representante de ventas. Para obtener más información, consulte [Gestión de los límites de almacenamiento](managing-storage-limits.html).

## ¿Cuántos volúmenes de {{site.data.keyword.blockstorageshort}} se pueden montar en un host?
Eso depende de lo que el sistema operativo del host pueda manejar, no se trata de una limitación de {{site.data.keyword.BluSoftlayer_full}}. Consulte la documentación de su sistema operativo para conocer los límites en la cantidad de volúmenes que se pueden montar.

## ¿Se aplica el límite de IOPS asignado por instancia o por volumen?
IOPS se aplica a nivel de volumen. Dicho de otro modo, dos hosts conectados a un volumen con 6000 IOPS comparten estos 6000 IOPS.

## Medición de IOPS
IOPS se mide en función de un perfil de carga de bloques de 16 KB con 50 % de lecturas y 50 % de escrituras aleatorias. Las cargas de trabajo que difieren de este perfil pueden experimentar un rendimiento inferior.

## ¿Qué ocurre cuando se utiliza un tamaño de bloque más pequeño para medir el rendimiento?
El máximo de IOPS todavía se puede obtener cuando se utilizan tamaños de bloque más pequeños. Sin embargo, el rendimiento se vuelve más pequeño. Por ejemplo, un volumen con 6000 IOPS tendrá el siguiente rendimiento en varios tamaños de bloque:

- 16 KB * 6000 IOPS == ~93,75 MB/seg. 
- 8 KB * 6000 IOPS == ~46,88 MB/seg.
- 4 KB * 6000 IOPS == ~23,44 MB/seg.

## ¿El volumen debe calentarse previamente para alcanzar el rendimiento esperado?
No es necesario ningún calentamiento previo. Puede observar el rendimiento especificado inmediatamente después de suministrar el volumen.

## ¿Se puede lograr más rendimiento mediante una conexión Ethernet más rápida?
Los límites de rendimiento están establecidos por volumen/LUN, por lo que utilizar una conexión de Ethernet más rápida no aumenta el límite establecido. Sin embargo, con una conexión Ethernet más lenta, el ancho de banda sí que puede ser un posible cuello de botella.

## ¿Los cortafuegos/grupos de seguridad afectan al rendimiento?
Es mejor ejecutar el tráfico de almacenamiento en una VLAN, que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software aumenta la latencia y afecta negativamente al rendimiento del almacenamiento.

## ¿Qué latencia se puede esperar de la publicación {{site.data.keyword.blockstorageshort}}?   
La latencia de destino en el almacenamiento es de <1 ms. El almacenamiento está conectado a instancias de cálculo en una red compartida, por lo que la latencia de rendimiento exacta depende del tráfico de red durante la operación.

## ¿Por qué se puede solicitar {{site.data.keyword.blockstorageshort}} con un nivel 10 de IOPS/GB de Resistencia en algunos centros de datos y no en otros?
El nivel 10 de IOPS/GB de tipo Resistencia {{site.data.keyword.blockstorageshort}} solo está disponible en centros de datos seleccionados, y se están añadiendo nuevos centros de datos gradualmente. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](new-ibm-block-and-file-storage-location-and-features.html).

## ¿Cómo podemos saber cuáles de los LUN/volúmenes de {{site.data.keyword.blockstorageshort}} están cifrados?
Al consultar la lista de {{site.data.keyword.blockstorageshort}} en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, puede ver un icono de bloqueo a la derecha del nombre de volumen/LUN para estos LUN/volúmenes que están cifrados.

## ¿Cómo sabemos cuándo estamos suministrando {{site.data.keyword.blockstorageshort}} en un centro de datos actualizado?
Al solicitar {{site.data.keyword.blockstorageshort}}, todos los centros de datos actualizados se marcan con un asterisco (`*`) en el formulario de pedido y una indicación de que está a punto de suministrar almacenamiento con cifrado. Una vez suministrado el almacenamiento, puede ver un icono en la lista de almacenamiento que muestra que dicho almacenamiento está cifrado. Todos los volúmenes cifrados y LUN se suministran únicamente en centros de datos actualizados. Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](new-ibm-block-and-file-storage-location-and-features.html).

## Si tenemos {{site.data.keyword.blockstorageshort}} no cifrado en un centro de datos actualizado recientemente, ¿podemos cifrar dicho {{site.data.keyword.blockstorageshort}}?
{{site.data.keyword.blockstorageshort}} suministrado antes de la actualización del centro de datos no se puede cifrar. 
El nuevo {{site.data.keyword.blockstorageshort}} suministrado en centros de datos actualizados se cifra automáticamente. No hay que elegir ningún valor de cifrado; es automático. 
Los datos que residen en almacenamiento no cifrado en un centro de datos actualizado se pueden cifrar creando un nuevo LUN de bloque para posteriormente copiar los datos al nuevo LUN cifrado con migración basada en host. Pulse [aquí](migrate-block-storage-encrypted-block-storage.html) para obtener instrucciones.

## ¿{{site.data.keyword.blockstorageshort}} da soporte a la reserva persistente SCSI-3 para implementar una barrera de E/S para Db2 pureScale?
Sí, {{site.data.keyword.blockstorageshort}} da soporte a las reservas persistentes SCSI-2 y SCSI-3.

## ¿Qué ocurre con los datos cuando se suprimen LUN de {{site.data.keyword.blockstorageshort}}?
{{site.data.keyword.blockstoragefull}} presenta volúmenes en bloque a los clientes en almacenamiento físico que se borra antes de cualquier reutilización. Los clientes con requisitos especiales de cumplimiento, como las directrices NIST 800-88 para el saneamiento de datos, deben realizar el procedimiento de saneamiento de datos antes de suprimir su almacenamiento.

## ¿Qué pasa a las unidades que quedan fuera de servicio en el centro de datos de nube?
Cuando las unidades quedan fuera de servicio, IBM las destruye antes de desecharlas. Las unidades se vuelven inutilizables. Los datos escritos en dicha unidad se vuelven inaccesibles.
