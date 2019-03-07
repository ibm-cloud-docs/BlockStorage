---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Expandir la capacidad de almacenamiento en bloque
{: #expandingcapacity}

Con esta nueva característica, los usuarios actuales de {{site.data.keyword.blockstoragefull}} pueden ampliar el tamaño de su {{site.data.keyword.blockstorageshort}} existente en incrementos de GB de hasta 12 TB inmediatamente. No es necesario que creen un duplicado ni que migren los datos manualmente a un volumen más grande. No se producen paradas ni falta de acceso al almacenamiento mientras se realiza el redimensionamiento.

Los datos de facturación del volumen se actualizan automáticamente para añadir al ciclo de facturación actual la diferencia prorrateada del nuevo precio. El nuevo importe completo se factura en el siguiente ciclo de facturación.

Esta característica está disponible en [centros de datos seleccionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

## Ventajas de almacenamiento expandible

- **Gestión de costes**: Ya es consciente del potencial de crecimiento de sus datos, pero además necesita una menor cantidad de almacenamiento para empezar. La capacidad de ampliación permite a nuestros clientes ahorrar en costes de almacenamiento e ir creciendo en función de sus necesidades.  

- **Necesidades de almacenamiento crecientes**: los clientes con un rápido crecimiento necesitan disponer de un método rápido y sencillo de incrementar su almacenamiento para gestionar este crecimiento.

## Efectos de ampliar la capacidad de almacenamiento en la Réplica

La acción de ampliar el almacenamiento primario genera un redimensionamiento automático de la réplica.

## Limitaciones
{: #limitsofexpandingstorage}

Esta característica está disponible para el almacenamiento suministrado en [determinados centros de datos](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

El almacenamiento suministrado a estos centros de datos antes de sacar al mercado esta característica, durante el periodo **Abril de 2017 - 14 de diciembre de 2017**, se puede incrementar hasta 10 veces su tamaño original y no más. El almacenamiento suministrado después del **14 de diciembre de 2017** se puede aumentar hasta alcanzar 12 TB.

Las limitaciones de tamaño actuales para {{site.data.keyword.blockstorageshort}} suministrado con Resistencia aún se aplican (hasta 4 TB para IOPS de nivel 10 y hasta 12 TB para los demás niveles).

## Redimensionamiento de almacenamiento
{: #resizingsteps}

1. En el portal de {{site.data.keyword.slportal}}, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**, o bien desde la consola de {{site.data.keyword.BluSoftlayer_full}} pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Seleccione el LUN de la lista y pulse **Acciones** > **Modificar LUN**
3. Especifique el nuevo tamaño de almacenamiento en GB.
4. Revise su selección y el nuevo precio.
5. Marque el recuadro de selección **He leído el Acuerdo de servicio maestro...** y pulse **Realizar pedido**.
6. La nueva asignación de almacenamiento está disponible en pocos minutos.

De manera alternativa, puede redimensionar el volumen a través de la SLCLI.

```
# slcli block volume-modify --help
Uso: slcli block volume-modify [OPCIONES] ID_VOLUMEN

Opciones:
  -c, --new-size ENTERO         Nuevo tamaño del volumen de bloque en GB. ***Si no se
                                especifica tamaño, se usará el tamaño original
                                del volumen.***
                                Tamaños potenciales: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Mínimo: [el tamaño original del volumen]
  -i, --new-iops ENTERO         IOPS de almacenamiento de rendimiento, entre 100
                                y 6000 en múltiplos de 100 [sólo para volúmenes de
                                rendimiento] ***Si no se especifica valor de IOPS,
                                se usará el valor de IOPS original del volumen.***
                                Requisitos: [Si el IOPS/GB del volumen es menor
                                que 0,3, el nuevo IOPS/GB debe ser también menor que
                                0,3. Si el IOPS/GB original del volumen es mayor o
                                igual que 0,3 el nuevo IOPS/GB del volumen debe ser
                                también mayor o igual que 0,3.]
  -t, --new-tier [0.25|2|4|10]  Nivel de almacenamiento resistente (IOPS por GB)
                                [sólo para volúmenes de resistencia] ***Si no se
                                especifica nivel, se usará el nivel original del
                                volumen.***
                                Requisitos: [Si el IOPS/GB original del volumen es
                                0,25, el nuevo IOPS/GB del volumen debe ser
                                también 0,25. Si el IOPS/GB original del volumen es
                                mayor que 0,25 el nuevo IOPS/GB del volumen debe ser
                                también mayor que 0,25.]
  -h, --help      Mostrar este mensaje y salir.
```
{:codeblock}

Para obtener más información sobre cómo ampliar el sistema de archivos (y las particiones, si las hay) en el volumen para utilizar el nuevo espacio, consulte la documentación del sistema operativo.
{:tip}
