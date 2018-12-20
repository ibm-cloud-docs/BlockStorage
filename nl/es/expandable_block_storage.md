---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-13"

---
{:new_window: target="_blank"}

# Expandir la capacidad de almacenamiento en bloque

Con esta nueva característica, los usuarios actuales de {{site.data.keyword.blockstoragefull}} pueden ampliar el tamaño de su {{site.data.keyword.blockstorageshort}} existente en incrementos de GB de hasta 12 TB inmediatamente. No tienen que crear un duplicado ni migrar manualmente los datos a un volumen más grande. No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento.

La facturación del volumen se actualizará automáticamente para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. El nuevo importe completo se factura en el siguiente ciclo de facturación.

Esta característica está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).

## Ventajas de almacenamiento expandible

- **Gestión de costes**: Ya es consciente del potencial de crecimiento de sus datos, pero además necesita una menor cantidad de almacenamiento para empezar. La capacidad de ampliación permite a nuestros clientes ahorrar en costes de almacenamiento e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes**: los clientes con un rápido crecimiento necesitan disponer de un método rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## Efectos de ampliar la capacidad de almacenamiento en la Réplica

La acción de ampliar el almacenamiento primario genera un redimensionamiento automático de la réplica.

## Limitaciones

Esta característica está disponible para el almacenamiento suministrado en [determinados centros de datos](new-ibm-block-and-file-storage-location-and-features.html).

El almacenamiento suministrado a estos centros de datos antes de sacar al mercado esta característica, durante el periodo **Abril de 2017 - 14 de diciembre de 2017**, se puede incrementar hasta 10 veces su tamaño original y no más. El almacenamiento suministrado después del **14 de diciembre de 2017** se puede aumentar hasta alcanzar 12 TB.

Las limitaciones de tamaño actuales para {{site.data.keyword.blockstorageshort}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## Redimensionamiento de almacenamiento

1. En el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleccione el LUN de la lista y pulse **Acciones** > **Modificar LUN**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento está disponible en pocos minutos.
