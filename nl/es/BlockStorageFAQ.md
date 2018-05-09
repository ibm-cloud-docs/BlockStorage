---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-24"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Preguntas más frecuentes sobre {{site.data.keyword.blockstorageshort}}

## ¿Cuántas instancias pueden compartir el uso de un volumen de {{site.data.keyword.blockstorageshort}} suministrado?
El límite predeterminado para el número de autorizaciones por volumen de bloque es de 8. Póngase en contacto con su representante de ventas para aumentar el límite.

## Al suministrar el {{site.data.keyword.blockstorageshort}} de Rendimiento o Resistencia, ¿las IOPS asignadas se aplican por instancia o por volumen?
Los IOPS se aplican a nivel de volumen. Dicho de otro modo, dos hosts conectados a un volumen con 6000 IOPS comparten estos 6000 IOPS.

## ¿Cómo se miden las IOPS?
Los IOPS se miden en función de un perfil de carga de bloques de 16 KB con 50% de lecturas y 50% de escrituras aleatorias. Las cargas de trabajo que difieren de este perfil pueden experimentar un rendimiento inferior.

## ¿Qué ocurre si utilizo un tamaño de bloque inferior al medir el rendimiento?
El máximo de IOPS todavía se puede obtener si se utilizan tamaños de bloque más pequeños, aunque el rendimiento será inferior. Por ejemplo; un volumen con 6000 IOPS tendrá el siguiente rendimiento en varios tamaños de bloque:

- 16 KB * 6000 IOPS == ~93,75 MB/seg. 
-  8 KB * 6000 IOPS == ~46,88 MB/seg.
-  4 KB * 6000 IOPS == ~23,44 MB/seg.

## ¿El volumen debe calentarse previamente para alcanzar el rendimiento esperado?
No es necesario ningún calentamiento previo. Observará el rendimiento especificado inmediatamente después de suministrar el volumen.

## ¿Por qué puedo suministrar {{site.data.keyword.blockstorageshort}} con un nivel 10 de IOPS de Resistencia en algunos centros de datos y en otros no?
El nivel 10 IOPS/GB de tipo almacenamiento de Resistencia de {{site.data.keyword.blockstorageshort}} está disponible en centros de datos seleccionados, pronto se añadirán nuevos centros de datos.  Puede consultar una lista completa de centros de datos actualizados y características disponibles aquí.

## ¿Cómo puedo saber cuáles de mis volúmenes/LUN de {{site.data.keyword.blockstorageshort}} están cifrados?
Al consultar la lista de {{site.data.keyword.blockstorageshort}} en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, verá un icono de bloqueo a la derecha del nombre de volumen/LUN en aquellos que estén cifrados.

## ¿Cómo sé si estoy suministrando el {{site.data.keyword.blockstorageshort}} en un centro de datos actualizado?
Al suministrar el {{site.data.keyword.blockstorageshort}}, todos los centros de datos actualizados se marcan con un asterisco (`*`) en el formulario del pedido y una indicación de que suministrará almacenamiento con cifrado. Una vez suministrado el almacenamiento, verá un icono en la lista de almacenamiento que muestra dicho volumen como cifrado. Todos los volúmenes cifrados y volúmenes se suministran únicamente en centros de datos actualizados. Puede consultar una lista completa de centros de datos actualizados y características disponibles aquí.

## ¿Por qué puedo suministrar {{site.data.keyword.blockstorageshort}} con un nivel 10 de IOPS de Resistencia en algunos centros de datos y en otros no?
El nivel 10 IOPS/GB de tipo Resistencia está disponible en centros de datos seleccionados, pronto se añadirán nuevos centros de datos.  Puede consultar una lista completa de centros de datos actualizados y características disponibles [aquí](new-ibm-block-and-file-storage-location-and-features.html).

## Si tengo {{site.data.keyword.blockstorageshort}} no cifrado suministrado en el centro de datos que ha sido actualizado para el cifrado, ¿puedo cifrar mi {{site.data.keyword.blockstorageshort}}?
El {{site.data.keyword.blockstorageshort}} que se haya suministrado antes de una actualización del centro de datos no puede cifrarse. 
El nuevo {{site.data.keyword.blockstorageshort}} suministrado en centros de datos actualizados se cifra automáticamente; no hay que elegir ningún valor de cifrado, es automático. 
Los datos que residen en almacenamiento no cifrado en un centro de datos actualizado se pueden cifrar creando un nuevo LUN de bloque para posteriormente copiar los datos al nuevo LUN cifrado con migración basada en host. Consulte este [artículo](migrate-block-storage-encrypted-block-storage) para obtener instrucciones sobre cómo realizar la migración.

## ¿Cuántos volúmenes puedo suministrar?
De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}.  Póngase en contacto con su representante de ventas para aumentar el número de volúmenes.

## ¿Podré incrementar el rendimiento si utilizo una conexión de Ethernet más rápida?
Los límites de rendimiento están establecidos por volumen/LUN, por lo que utilizar una conexión de Ethernet más rápida no incrementará el límite establecido. Sin embargo, con una conexión Ethernet más lenta, el ancho de banda sí que puede ser un posible cuello de botella.

## ¿Los cortafuegos/grupos de seguridad afectarán al rendimiento?
Recomendamos que ejecute el tráfico de almacenamiento en una red de área local virtual que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software incrementará la latencia e incidirá negativamente sobre el rendimiento del almacenamiento.

## ¿Qué latencia de rendimiento puedo esperar de mi {{site.data.keyword.blockstorageshort}}?   

La latencia de destino en el almacenamiento es de <1 ms. Nuestro almacenamiento está conectado a instancias de cálculo en una red compartida, por lo que la latencia de rendimiento exacta dependerá del tráfico de la red dentro de un plazo determinado.

## ¿{{site.data.keyword.blockstorageshort}} da soporte a la reserva persistente SCSI-3 para implementar una barrera de E/S para Db2 pureScale?
Sí, {{site.data.keyword.blockstorageshort}} da soporte a las reservas persistentes SCSI-2 y SCSI-3.

## ¿Qué ocurre con mis datos cuando se suprimen LUN de {{site.data.keyword.blockstorageshort}}?

Cuando se suprime el almacenamiento, se eliminan todos los punteros a los datos en dicho volumen, por lo que los datos serán completamente inaccesibles. Si el almacenamiento físico se vuelve a suministrar a otra cuenta, se asignará un nuevo conjunto de punteros. La nueva cuenta no tiene ningún modo de acceder a los datos que pudieran haberse guardado en el almacenamiento físico, el nuevo conjunto de punteros muestra todo 0. Cuando se escriben nuevos datos en el volumen/LUN, se sobrescriben los datos inaccesibles que aún existan.

## ¿Qué pasa a las unidades que quedan fuera de servicio en el centro de datos de nube?

Cuando las unidades quedan fuera de servicio, IBM las destruye antes de desecharlas, por lo que quedan inservibles. Todos los datos escritos en esa unidad serán inaccesibles.
