---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Realizar pedidos de instantáneas

Para crear instantáneas de su volumen de almacenamiento, automática o manualmente, necesita adquirir espacio para mantenerlas. Puede adquirir capacidad hasta la cantidad de su volumen de almacenamiento (durante la adquisición de volumen inicial o posteriormente siguiendo los pasos siguientes).

1. Acceda a su LUN de almacenamiento a través del separador **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Pulse el enlace **Añadir espacio de instantáneas** en el marco Instantáneas.
3. Seleccione la cantidad de espacio que necesita pulsando el botón de selección situado junto a la cantidad adecuada.
4. Pulse **Continuar**.
5. Especifique cualquier código promocional que tenga y pulse **Recalcular**. Las opciones Cargos para este pedido y Revisión del pedido tendrán valores predeterminados.
6. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas se suministrará en unos minutos.

## Cómo determinar cuánto espacio de instantáneas pedir

Por desgracia, no existe una mejor práctica, sino más bien una recomendación basada en la aplicación. Como norma general, el espacio de instantáneas se consume con instantáneas basadas en dos informaciones clave:
- cuántos cambios tiene su sistema de archivos activo, y 
- cuánto tiempo tiene previsto retener las instantáneas.  

Básicamente, la forma de calcular la cantidad de espacio necesario es **(Tasa de Cambio)** x **(número de horas/días/semanas/meses de retención)**.  
**Nota**: la primera instantánea consume una cantidad insignificante de espacio porque solo es una copia de los metadatos (punteros) que indica los bloques del sistema de archivos activo. 

Un volumen con mucho cambio de datos (por ejemplo, una base de datos con una tasa de cambio alta) y un periodo largo de retención de instantáneas va a necesitar más espacio para las instantáneas que un volumen con una cantidad moderada de cambios (por ejemplo, un almacén de datos de máquina virtual) y una planificación de retención de instantáneas más moderada. 

En el caso de un volumen que tiene 500 GB de datos reales, si fuera a realizar 12 instantáneas por hora, y tuviera un 1% de cambios entre cada instantánea, acabaría con (5 G de tasa de cambio) x (12 instantáneas por hora) = 60 GB para instantáneas.

Por el contrario, si estos 500 GB de datos reales, con 12 instantáneas por hora, tuvieran un 10% de cambios cada hora, terminaría con (50 GB de tasa de cambio) x (12 instantáneas por hora) = 600 GB.

Por tanto, a la hora de determinar cuánto espacio de instantáneas necesita, preste atención a la tasa de cambio. Influye mucho sobre el espacio de instantáneas necesario.  Mientras que el tamaño de un volumen probablemente significará una cantidad mayor de cambio, un volumen de 500 G con 5 G de tasa de cambio y un volumen de 10 TB con 5 G de tasa de cambio requerirán el mismo espacio de instantáneas.

Además, para la mayoría de las cargas de trabajo, cuanto mayor sea un volumen, menor será el espacio inicial necesario para instantáneas.  Esto es debido principalmente a la eficiencia de los datos subyacentes de nuestra plataforma, así como al modo en que las instantáneas trabajan en nuestro entorno.



