---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Creación de un volumen de bloque duplicado

Puede crear un duplicado de un {{site.data.keyword.blockstoragefull}} existente. El volumen duplicado hereda la capacidad y las opciones de rendimiento del LUN/volumen original de forma predeterminada y tiene una copia de los datos hasta el momento de la instantánea.   

Como el duplicado se basa en los datos en el momento específico de una instantánea, se necesita espacio de instantáneas en el volumen original antes de crear un duplicado. Para obtener más información sobre las instantáneas y cómo solicitar espacio de instantáneas, consulte la [documentación de instantáneas](snapshots.html).  

Los duplicados pueden crearse a partir de volúmenes **primarios** y **de réplica**. El nuevo duplicado se crea en el mismo centro de datos que el volumen original. Si crea un duplicado a partir de un volumen de réplica, el nuevo volumen se crea en el mismo centro de datos que el volumen de réplica.

Se puede acceder a los volúmenes duplicados mediante un host para lectura/escritura siempre y cuando el almacenamiento esté suministrado. Sin embargo, no se permiten instantáneas ni réplicas hasta que se completa la copia de datos del original en el duplicado. 

Una vez completada la copia de datos, el duplicado se puede gestionar y utilizar como un volumen completamente independiente. 

Esta característica está disponible en la mayoría de las ubicaciones. Pulse [aquí](new-ibm-block-and-file-storage-location-and-features.html) para ver la lista de centros de datos disponibles.

Algunos usos comunes para un volumen duplicado:
- **Prueba de recuperación tras desastre**: Cree un duplicado de su volumen de réplica para verificar que los datos estén intactos y puedan utilizarse en caso de desastre, sin interrumpir la réplica. 
- **Copia de oro**: Utilice un volumen de almacenamiento como copia de oro desde la que puede crear múltiples instancias para varios usos. 
- **Renovaciones de datos**: Cree una copia de los datos de producción para montar en su entorno de no producción para la realización de pruebas. 
- **Restaurar desde instantánea**: Restaure datos en el volumen original con archivos/fechas específicos de una instantánea sin sobrescribir todo el volumen original con una función de restauración de instantáneas. 
- **Desarrollo y prueba (dev/test)**: Cree hasta cuatro duplicados simultáneos de un volumen a la vez para crear datos duplicados para desarrollo y pruebas. 
- **Redimensionamiento del almacenamiento**: Cree un volumen con el nuevo tamaño, tasa de IOPS o ambos sin necesidad de mover los datos.  
	
Puede crear un volumen duplicado a través del [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} de un par de maneras.


## Creación de un duplicado a partir de un volumen específico en la lista de almacenamiento

1. Vaya a su lista de {{site.data.keyword.blockstorageshort}}
    - Desde el portal de clientes, pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}** O
    - Desde el catálogo de {{site.data.keyword.BluSoftlayer_full}}, pulse **Infraestructura** > **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**. 
2. Seleccione un volumen de la lista y pulse **Acciones** > **Duplicar LUN (Volumen)** 
3. Elija la opción de instantánea: 
    - Si solicita a partir de un volumen **no de réplica**,
      - Seleccione **Crear desde nueva instantánea**: Esta acción crea una instantánea que se utilizará para el duplicado. Utilice esta opción si actualmente el volumen no tiene instantáneas o si desea crear un duplicado inmediatamente.<br/>
      - Seleccione **Crear desde la última instantánea**: Esta acción crea un duplicado desde la instantánea más reciente que exista para este volumen. 
    - Si solicita a partir de un volumen **de réplica**, la única opción para la instantánea es utilizar la instantánea más reciente disponible. 
4. El tipo de almacenamiento y la ubicación siguen siendo los mismos que el volumen original.
5. Facturación mensual o por horas: puede elegir si suministrar el nuevo LUN duplicado con facturación mensual o por horas. El tipo de facturación para el volumen original se selecciona automáticamente. Si quiere elegir otro tipo de facturación para el almacenamiento de duplicado, puede seleccionarlo aquí. 
5. Si lo desea, puede especificar IOPS o el nivel de IOPS para el nuevo volumen. La designación de IOPS del volumen original se establece de forma predeterminada. Se mostrarán las combinaciones de tamaño y rendimiento disponibles.
    - Si su volumen original es de nivel de Resistencia de IOPS 0,25, no se puede realizar una nueva selección. 
    - Si el volumen original es de nivel de Resistencia de IOPS 2, 4 o 10, puede moverse entre estos niveles para el nuevo volumen. 
6. Puede actualizar el tamaño del nuevo volumen de modo que sea mayor que el original. El tamaño del volumen original se establece de forma predeterminada. 
    - **Nota**: {{site.data.keyword.blockstorageshort}} se puede redimensionar hasta 10 veces el tamaño original del volumen. 
7. Puede actualizar el espacio de instantáneas para el nuevo volumen para añadir más, menos o ningún espacio de instantáneas. El espacio de instantáneas del volumen original se establece de forma predeterminada. 
8. Pulse **Continuar** para realizar el pedido. 



## Creación de un duplicado a partir de una instantánea específica

1. Vaya a su lista de {{site.data.keyword.blockstorageshort}}
2. Pulse un **LUN/volumen** de la lista para visualizar la página de detalles. (Puede ser un volumen de réplica o sin réplica). 
3. Desplácese hacia abajo y seleccione una instantánea existente de la lista en la página de detalles y pulse **Acciones** > **Duplicar**.   
4. El Tipo de almacenamiento (Resistencia o Rendimiento) y la Ubicación son los mismos que el volumen original. 
5. Se mostrarán las combinaciones de tamaño y rendimiento disponibles. La designación de IOPS del volumen original se establece de forma predeterminada. Puede especificar IOPS o el nivel de IOPS para el nuevo volumen. 
    - Si su volumen original es de nivel de Resistencia de IOPS 0,25, no se puede realizar una nueva selección. 
    - Si el volumen original es de nivel de Resistencia de IOPS 2, 4 o 10, puede moverse entre estos niveles para el nuevo volumen. 
6. Puede actualizar el tamaño del nuevo volumen de modo que sea mayor que el original. El tamaño del volumen original se establece de forma predeterminada. 
    - **Nota**: {{site.data.keyword.blockstorageshort}} se puede redimensionar hasta 10 veces el tamaño original del volumen. 
7. Puede actualizar el espacio de instantáneas para el nuevo volumen para añadir más, menos o ningún espacio de instantáneas. El espacio de instantáneas del volumen original se establece de forma predeterminada. 
8. Pulse **Continuar** para realizar el orden de los duplicados. 


## Gestión del volumen duplicado

Mientras los datos se estén copiando desde el volumen original en el duplicado, puede ver un estado en la página de detalles que muestra que la duplicación está en curso. Durante este tiempo se puede conectar a un host y leer/escribir en el volumen, pero no puede crear planificaciones de instantáneas. Cuando se complete el proceso de duplicación, el nuevo volumen es independiente del original y se puede gestionar con instantáneas y réplica, como es habitual. 
