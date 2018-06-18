---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacidad de almacenamiento en bloque ampliable

Con esta nueva característica, nuestros usuarios actuales de {{site.data.keyword.blockstoragefull}} podrán ampliar el tamaño de su {{site.data.keyword.blockstorageshort}} existente en incrementos de GB de hasta 12 TB de forma inmediata, sin necesidad de crear un duplicado o de migrar datos manualmente a un volumen más grande. No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento. 

La facturación del volumen se actualizará automáticamente para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. El nuevo importe completo se factura en el siguiente ciclo de facturación.

Esta característica solo está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 

## ¿Por qué utilizar el almacenamiento ampliable?

- **Gestión de costes**: ya es consciente del potencial de crecimiento de sus datos, pero además necesita una menor cantidad de almacenamiento para empezar. La capacidad de ampliación permite a nuestros clientes ahorrar en costes de almacenamiento e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes**: los clientes con un rápido crecimiento necesitan disponer de un método rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## ¿Cómo afecta a la réplica la capacidad de ampliación del almacenamiento?

La acción de ampliar el almacenamiento primario generará un redimensionamiento automático de la réplica. 

## ¿Existe alguna limitación?

Esta característica solo está disponible para el almacenamiento suministrado en [determinados centros de datos](new-ibm-block-and-file-storage-location-and-features.html). 

El almacenamiento suministrado en estos centros de datos antes de sacar al mercado esta característica (14 de diciembre de 2017) solo se puede incrementar hasta 10 veces su tamaño original. El almacenamiento suministrado después de esta fecha se puede aumentar hasta alcanzar un tamaño de 12 TB. 

Las limitaciones de tamaño actuales para {{site.data.keyword.blockstorageshort}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## ¿Cómo puedo saber si mi almacenamiento está cifrado?

El almacenamiento suministrado con funciones mejoradas siempre incluye cifrado en reposo. Puede saber fácilmente si es aplicable a su almacenamiento si tiene un icono de bloqueo en la interfaz de usuario del portal. 

## ¿Cómo puedo cambiar el tamaño de mi almacenamiento?

1. En el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleccione el LUN de la lista y pulse **Acciones** > **Modificar LUN**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento debería estar disponible en unos minutos.
  
