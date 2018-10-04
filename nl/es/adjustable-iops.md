---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-12"

---
{:new_window: target="_blank"}

# Ajuste de IOPS

Con esta nueva característica, los usuarios de almacenamiento {{site.data.keyword.blockstoragefull}} podrán ajustar IOPS de su {{site.data.keyword.blockstorageshort}} existente de inmediato. No tienen que crear un duplicado ni copiar los datos manualmente en el almacenamiento nuevo. Los usuarios no experimentan ningún tipo de parada ni falta de acceso al almacenamiento mientras se realiza el ajuste. 

La facturación del almacenamiento se actualiza para añadir la diferencia prorrateada del nuevo precio en el ciclo de facturación actual. El nuevo importe completo se factura en el siguiente ciclo de facturación.


## Ventajas de IOPS ajustables

- Gestión de costes – Algunos clientes pueden necesitar alta IOPS solo durante picos de uso. Por ejemplo, un almacén de una tienda de gran tamaño tiene picos de uso durante las vacaciones y podría necesitar más IOPS en el almacenamiento en ese momento. Sin embargo, no necesitan más IOPS a mediados de verano. Esta característica les permite gestionar sus costes y pagar más IOPS cuando lo necesitan.

## Limitaciones

Esta característica solo está disponible en [centros de datos seleccionados](new-ibm-block-and-file-storage-location-and-features.html).

Los clientes no pueden cambiar entre Resistencia y Rendimiento al ajustar su IOPS. Sin embargo, pueden especificar un nuevo nivel de IOPS para su almacenamiento en función de los siguientes criterios/restricciones: 

- Si el volumen original es Resistencia nivel 0,25, el nivel de IOPS no se puede actualizar.
- Si el volumen original es Rendimiento con menos que o igual a 0,30 IOPS/GB, las opciones disponibles solo incluyen el tamaño y las combinaciones de IOPS que resulten en menos que o igual a 0,30 IOPS/GB.
- Si el volumen original es Rendimiento con más de 0,30 IOPS/GB, las opciones disponibles solo incluyen el tamaño y combinaciones de IOPS que resulten en más de 0,30 IOPS/GB.

## Efecto del ajuste de IOPS en la réplica

Si el volumen tiene réplica, la réplica se actualiza automáticamente para coincidir con la selección de IOPS de la primaria. 

## Ajuste de la IOPS en el almacenamiento

1. Vaya a su lista de {{site.data.keyword.blockstorageshort}}
   - En el {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**
   - Desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleccione el LUN de la lista y pulse **Acciones** > **Modificar LUN**
3. En las **Opciones IOPS de almacenamiento**, realice una nueva selección:
    - Resistencia (IOPS por niveles): seleccione un nivel de IOPS superior a 0,25 IOPS/GB de su almacenamiento. Puede aumentar el nivel de IOPS en cualquier momento. Sin embargo, la disminución solo está disponible una vez al mes.
    - Rendimiento (IOPS asignado): Especifique la nueva opción de IOPS para su almacenamiento especificando un valor en el rango de 100 a 48.000 IOPS. (Asegúrese de comprobar los límites específicos requeridos por tamaño en el formulario de pedido).
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento está disponible en pocos minutos.
