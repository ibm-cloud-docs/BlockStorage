---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Realizar pedidos de instantáneas

Para crear instantáneas de su volumen de almacenamiento, automática o manualmente, necesita adquirir espacio para mantenerlas. Puede adquirir capacidad hasta la cantidad de su volumen de almacenamiento (durante la adquisición de volumen inicial o posteriormente siguiendo los pasos descritos aquí).

1. Inicie una sesión en la [consola de {{site.data.keyword.cloud_notm}}](https://console.bluemix.net/catalog/){:new_window} y pulse el icono **Menú** de la parte superior izquierda. Seleccione **Infraestructura clásica**.

   También puede iniciar la sesión en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Acceda a su LUN de almacenamiento a través de **Almacenamiento** >**{{site.data.keyword.blockstorageshort}}**.
2. Pulse **Cambiar espacio de instantáneas** en el marco Instantáneas.
3. Seleccione la cantidad de espacio que necesita y el método de pago.
4. Pulse **Continuar**.
5. Especifique cualquier **código promocional** que tenga y pulse **Recalcular**. Los campos Cargos para este pedido y Revisión de pedido contienen información de forma predeterminada.
6. Marque el recuadro **He leído el Acuerdo de Servicio Maestro y acepto sus condiciones.** y pulse **Realizar pedido**. El espacio de instantáneas se suministra en pocos minutos.

## Determinación de la cantidad de espacio de instantáneas que se debe pedir

Como norma general, el espacio de instantáneas que utilizan las instantáneas se basa en dos factores clave
- Cuánto cambia el sistema de archivos activo a lo largo del tiempo,
- Cuánto tiempo tiene previsto retener las instantáneas.  

La forma de calcular la cantidad de espacio que necesita es **(Tasa de Cambio)** x **(número de horas/días/semanas/meses que se retienen los datos)**.

La primera instantánea utiliza una cantidad insignificante de espacio porque solo es una copia de los metadatos (punteros) que indica los bloques del sistema de archivos activo.
{:note}

Un volumen con muchos cambios y un periodo largo de retención necesita más espacio que un volumen con una cantidad moderada de cambios y una planificación de retención moderada. Un ejemplo para el primer tipo es una base de datos de tasas de cambio alta. Un ejemplo para el segundo tipo es un almacén de datos VMware.

Si realiza 12 instantáneas por hora de 500 GB de datos reales y hay un 1 por ciento de cambio entre cada instantánea, necesita 60 GB para instantáneas.

*(5 G de tasa de cambio) x (12 instantáneas por hora) = (60 GB de espacio utilizado)*

Por el contrario, si en estos 500 GB de datos reales, con 12 instantáneas por hora, se observara un 10 por ciento de cambios cada hora, el espacio de instantáneas que se utiliza es de 600 GB.

*(50 GB de tasa de cambio) x (12 instantáneas por hora) = (600 GB de espacio utilizado)*

Por tanto, a la hora de determinar cuánto espacio de instantáneas necesita, preste atención a la tasa de cambio. Influye mucho sobre el espacio de instantáneas necesario. Un volumen más grande es más probable que cambie más a menudo. Sin embargo, un volumen de 500 GB con 5 GB de tasa de cambio y un volumen de 10 TB con 5 GB de tasa de cambio utilizan la misma cantidad de espacio de instantáneas.

Además, para la mayoría de las cargas de trabajo, cuanto mayor sea un volumen, menor será el espacio inicial necesario. Esto es debido principalmente a la eficiencia de los datos subyacentes, así como al modo en que las instantáneas funcionan en el entorno.
