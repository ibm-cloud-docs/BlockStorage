---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Realizar pedidos de instantáneas

Para crear instantáneas de su volumen de almacenamiento, automática o manualmente, necesita adquirir espacio para mantenerlas. Puede adquirir capacidad hasta la cantidad de su volumen de almacenamiento (durante la adquisición de volumen inicial o posteriormente siguiendo los pasos descritos en este artículo).

1. Acceda a su LUN de almacenamiento a través del separador **Almacenamiento**, **{{site.data.keyword.blockstorageshort}}** del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Pulse **Añadir espacio de instantáneas** en el marco Instantáneas.
3. Seleccione la cantidad de espacio que necesita.
4. Pulse **Continuar**.
5. Especifique un **código promocional** si lo tiene y pulse **Recalcular**. Los campos Cargos para este pedido y Revisión de pedido contienen información de forma predeterminada.
6. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**. El espacio de instantáneas se suministrará en unos minutos.

## Cómo determinar la cantidad de espacio de instantáneas que solicitar

Como norma general, el espacio de instantáneas que utilizan las instantáneas se basa en dos asuntos clave:
- cuántos cambios tiene su sistema de archivos activo,
- cuánto tiempo tiene previsto retener las instantáneas.  

Básicamente, la forma de calcular la cantidad de espacio necesario es **(Tasa de Cambio)** x **(número de horas/días/semanas/meses que se retienen los datos)**.  
**Nota**: la primera instantánea utiliza una cantidad insignificante de espacio porque solo es una copia de los metadatos (punteros) que indica los bloques del sistema de archivos activo. 

Un volumen con mucho cambio de datos (como una base de datos con una tasa de cambio alta) y un periodo largo de retención de instantáneas va a necesitar más espacio para las instantáneas que un volumen con una cantidad moderada de cambios (como un almacén de datos de máquina virtual) y una planificación de retención de instantáneas más moderada. 

Si tuviera que realizar 12 instantáneas por hora de un volumen de 500 GB de datos reales y observara un 1 por ciento de cambio entre cada instantánea, necesitaría 60 GB para instantáneas.

*(5 G de tasa de cambio) x (12 instantáneas por hora) = (60 GB de espacio utilizado)*

Por el contrario, si en estos 500 GB de datos reales, con 12 instantáneas por hora, se observara un 10 por ciento de cambios cada hora, necesitaría 600 GB.

*(50 GB de tasa de cambio) x (12 instantáneas por hora) = (600 GB de espacio utilizado)*

Por tanto, a la hora de determinar cuánto espacio de instantáneas necesita, preste atención a la tasa de cambio. Influye mucho sobre el espacio de instantáneas necesario. Mientras que el tamaño de un volumen probablemente significará una cantidad mayor de cambio, un volumen de 500 GB con 5 GB de tasa de cambio y un volumen de 10 TB con 5 GB de tasa de cambio requerirán el mismo espacio de instantáneas.

Además, para la mayoría de las cargas de trabajo, cuanto mayor sea un volumen, menor será el espacio inicial necesario para instantáneas. Esto es debido principalmente a la eficiencia de los datos subyacentes de nuestra plataforma, así como al modo en que las instantáneas trabajan en nuestro entorno.



