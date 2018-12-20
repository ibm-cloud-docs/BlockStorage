---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Solicitudes de {{site.data.keyword.blockstorageshort}} mediante la CLI de SL

Puede utilizar la CLI de SL para realizar pedidos de productos que normalmente se solicitan a través del [{{site.data.keyword.slportal}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://control.softlayer.com/){:new_window}. En la API de SL, un pedido puede consistir en varios contenedores de pedidos. La CLI de pedidos funciona con un solo contenedor de pedidos.

Para obtener más información sobre cómo instalar y utilizar la CLI de SL, consulte [Cliente de API de Python ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Búsqueda de las ofertas disponibles de {{site.data.keyword.blockstorageshort}}

El primer componente que se debe buscar cuando se realiza un pedido es un paquete. Los paquetes se dividen entre los distintos productos de nivel superior que están disponibles para solicitar en {{site.data.keyword.BluSoftlayer_full}}. Algunos paquetes de ejemplo son CLOUD_SERVER para VSI, BARE_METAL_SERVER para servidores nativos y STORAGE_AS_A_SERVICE_STAAS para {{site.data.keyword.blockstorageshort}} y {{site.data.keyword.filestorage_short}}.

Dentro de un paquete, algunos elementos se subdividen en categorías. Algunos paquetes tienen elementos preestablecido para su comodidad y en otros es necesario especificar los elementos individualmente. Si se necesita una categoría de un paquete, se debe seleccionar un elemento de dicha categoría para solicitar el paquete. Dependiendo de la categoría, algunos elementos dentro de la categoría pueden ser mutuamente excluyentes.

Cada pedido debe tener una ubicación asociada (centro de datos). Cuando solicite {{site.data.keyword.blockstorageshort}}, asegúrese de que se suministre en la misma ubicación que sus instancias de cálculo.
{:important}

Puede utilizar el mandato `slcli order package-list` para encontrar el paquete que desea solicitar. Se proporciona una opción `–keyword` para realizar una búsqueda y un filtrado simples. Esta opción hace que sea más fácil encontrar el paquete que necesita.

```
$ slcli order package-list --help
Uso: slcli order package-list [OPTIONS]

  Listar paquetes que se pueden solicitar mediante la API placeOrder.

  Ejemplo:
      # Lista de todos los paquetes para la solicitud
      slcli order package-list

  Las palabras clave también se pueden utilizar para algunas funciones de filtrado simple para que sea más fácil encontrar un paquete.

  Ejemplo:
     # Lista de todos los paquetes con "server" en el nombre
      slcli order package-list --keyword server

Opciones:
  --keyword TEXT  Una palabra (o serie) que se utiliza para filtrar nombres de paquetes.
  -h, --help      Mostrar este mensaje y salir.
```

*Instrucciones necesarias sobre cómo encontrar el paquete 759 de almacenamiento como servicio*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         name        :       keyName       :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               name               :    categoryCode   : isRequired :
:..................................:...................:............:
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:              Example             :        ???        :     Y      :
:..................................:...................:............:
```

Seleccione el resto de los elementos del pedido con el mandato `item-list`. Los paquetes suelen tener diversos elementos entre los que elegir; utilice la opción `–category` para recuperar solo los elementos de la categoría en la que está interesado.

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         keyName          :                description                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

Para obtener más información sobre cómo solicitar {{site.data.keyword.blockstorageshort}} a través de la API, consulte [order_block_volume ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}.
Para poder acceder a todas las nuevas características, solicite `el paquete 759 de almacenamiento como servicio`.
{:tip}

## Verificación del pedido

Si alguna vez no está seguro de las categorías necesarias que puede que falten en su pedido, puede utilizar el mandato `place` con el distintivo `-verify`. Si falta alguna categoría, se imprime en la pantalla.


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                keyName                       :                   description                   : cost :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

La salida muestra cada elemento que se está solicitando, junto con el coste asociado a dicho elemento. Si el pedido supera la verificación, significa que no hay elementos en conflicto y que todas las categorías relacionadas tienen un elemento que está especificado en el pedido.

## Realización del pedido

El siguiente paso es realizar el pedido.

```
$ slcli order place .....

Esta acción implicará cargos en su cuenta. ¿Desea continuar? [s/N]: s

Respuesta de API
```

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}}. Para aumentar el número de volúmenes, póngase en contacto con el representante de ventas. Para obtener más información sobre el aumento de los límites, consulte [Gestión de límites de almacenamiento](managing-storage-limits.html).
{:important}

## Autorización de los hosts para acceder al nuevo almacenamiento

Por determinar

Para obtener más información sobre la autorización de los hosts para acceder a {{site.data.keyword.blockstorageshort}} mediante la API, consulte [authorize_host_to_volume ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}
{:tip}

Para obtener información sobre el límite en autorizaciones simultáneas, consulte las [Preguntas más frecuentes](faqs.html).
{:important}

## Conexión del nuevo almacenamiento

En función del sistema operativo del host, siga el enlace adecuado.
- [Conexión a los LUN iSCSI de MPIO en Linux](accessing_block_storage_linux.html)
- [Conexión a los LUN de iSCSI de MPIO en CloudLinux](configure-iscsi-cloudlinux.html)
- [Conexión a los LUN de iSCSI de MPIO en Microsoft Windows](accessing-block-storage-windows.html)
- [Configuración de almacenamiento en bloque para la copia de seguridad con cPanel](configure-backup-cpanel.html)
- [Configuración de almacenamiento en bloque para la copia de seguridad con Plesk](configure-backup-plesk.html)
