---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ajuste de IOPS

Con esta nueva característica, nuestros usuarios de almacenamiento {{site.data.keyword.blockstoragefull}} podrán ajustar IOPS de su {{site.data.keyword.blockstorageshort}} existente de inmediato, sin tener que crear un duplicado o migrar los datos manualmente al nuevo almacenamiento. Los usuarios no experimentarán ningún tipo de parada o falta de acceso al almacenamiento mientras se realiza el ajuste. 

La facturación del almacenamiento se actualizará para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. El nuevo importe completo se factura en el siguiente ciclo de facturación.

Esta característica solo está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html). 

## ¿Por qué utilizar IOPS ajustable?

- Gestión de costes – Algunos de nuestros clientes solo necesitan IOPS altos durante picos de uso. Por ejemplo, un almacén de una tienda de gran tamaño tiene picos de uso durante las vacaciones y necesitará IOPS más alto en el almacenamiento en ese momento que a mediados de verano. Esta característica les permite gestionar sus costes y pagar más IOPS solo cuando realmente lo necesitan.

## ¿Existe alguna limitación?

Esta característica solo está disponible para el almacenamiento que se suministra en [centros de datos](new-ibm-block-and-file-storage-location-and-features.html) con funciones ampliadas. 

Los clientes no pueden cambiar entre Resistencia y Rendimiento al ajustar su IOPS. Los usuarios pueden especificar un nuevo nivel de IOPS para su almacenamiento en función de los siguientes criterios/restricciones: 

- Si el volumen original es Resistencia nivel 0,25, el nivel de IOPS no se puede actualizar.
- Si el volumen original es Rendimiento con < 0,30 IOPS/GB, las opciones disponibles solo deberían incluir el tamaño y combinaciones de IOPS que resulten en < 0,30 IOPS/GB. 
- Si el volumen original es Rendimiento con >= 0,30 IOPS/GB, las opciones disponibles solo deberían incluir el tamaño y combinaciones de iops que resulten en >= 0,30 IOPS/GB. Tamaño (mayor o igual que el volumen original)



## ¿Cómo afecta a la réplica el ajuste de IOPS?

Si el volumen tiene réplica, la réplica se actualizará automáticamente para coincidir con la selección de IOPS de la primaria. 

## ¿Cómo puedo ajustar las IOPS en mi almacenamiento?

1. En el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleccione el LUN de la lista y pulse **Acciones** > **Modificar LUN**
3. En las **Opciones IOPS de almacenamiento**, realice una nueva selección:
    - Resistencia (IOPS por niveles): seleccione un nivel de IOPS superior a 0,25 IOPS/GB de su almacenamiento. Puede aumentar el nivel de IOPS en cualquier momento. Sin embargo, la disminución solo está disponible una vez al mes.
    - Rendimiento (IOPS asignado): especifique la nueva opción de IOPS para su almacenamiento especificando un valor entre comprendido entre 100 y 48.000 IOPS. (Asegúrese de comprobar los límites específicos requeridos por tamaño en el formulario de pedido).
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento debería estar disponible en unos minutos.
