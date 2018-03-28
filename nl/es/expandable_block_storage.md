---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacidad de almacenamiento en bloque ampliable

Con esta nueva característica, nuestros usuarios de {{site.data.keyword.blockstoragefull}} actuales podrán ampliar el tamaño de su {{site.data.keyword.blockstorageshort}} existente en incrementos de GB hasta 12 TB sobre la marcha, sin necesidad de crear un duplicado o migrar datos manualmente a un volumen más grande.  No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento. 

La facturación del volumen se actualizará automáticamente para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual y posteriormente se facturará el nuevo importe total en el próximo ciclo de facturación.

Esta característica solo está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 

## ¿Por qué utilizar el almacenamiento ampliable?

- **Gestión de costes** – Es consciente del potencial de crecimiento de sus datos, pero necesitara una cantidad de almacenamiento inferior para empezar. La capacidad de ampliar permite a nuestros clientes ahorrar en costes de almacenamiento e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes** - Los clientes con un rápido crecimiento necesitan un modo rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## ¿Cómo afecta la capacidad de almacenamiento ampliable a la réplica?

La acción de ampliar el almacenamiento primario generará un redimensionamiento automático de la réplica. 

## ¿Existe alguna limitación?

Esta característica solo está disponible para el almacenamiento que se suministra en [centros de datos](new-ibm-block-and-file-storage-location-and-features.html) con funciones ampliadas. 

El almacenamiento que se suministra en el almacenamiento actualizado en estos centros de datos antes de publicar esta característica (14 de diciembre de 2017) solo se puede incrementar hasta 10 veces su tamaño original.  Todos los demás almacenamientos suministrados, tras esta fecha, se pueden incrementar hasta un tamaño máximo de 12 TB. 

Las limitaciones de tamaño actuales para {{site.data.keyword.blockstorageshort}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## ¿Cómo puedo saber si mi almacenamiento suministrado es ampliable?

El almacenamiento suministrado con funciones mejoradas siempre incluye cifrado en descanso.  Puede saber fácilmente si es aplicable a su almacenamiento si tiene un icono de bloqueo en la interfaz de usuario del portal. 

## ¿Cómo puedo redimensionar mi almacenamiento?

1. Desde el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleccione el LUN de la lista y pulse **Acciones** > **Modificar LUN**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento debería estar disponible en unos minutos.
  
