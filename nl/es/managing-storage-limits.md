---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gestión de los límites de almacenamiento
{: #managingstoragelimits}

De forma predeterminada, puede suministrar un total combinado de 250 volúmenes de {{site.data.keyword.blockstorageshort}} y de {{site.data.keyword.filestorage_short}} globalmente.

Si no está seguro de cuántos volúmenes tiene, puede listar los volúmenes de cada centro de datos utilizando el siguiente mandato de `slcli`.
```
# slcli block volume-count --help
Uso: slcli block volume-count [OPCIONES]

Opciones:
  -d, --datacenter TEXTO Nombre corto del centro de datos
  --sortby TEXTO         Columna por la que se debe ordenar
  -h, --help             Mostrar este mensaje y salir.
```

Puede solicitar un aumento del límite enviando una incidencia en el [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. Cuando se aprueba la solicitud, se obtiene un límite de volumen que se establece para un centro de datos específico.  

Para solicitar un aumento del límite, abra una incidencia y diríjala a su representante de ventas.

En la incidencia, proporcione la siguiente información:

- **Asunto de la incidencia**: Solicitud de aumento del límite de almacenamiento de recuento de volumen del centro de datos

- **¿En que casos de uso se solicitan volúmenes adicionales?** <br />
*Por ejemplo, su respuesta podría ser similar a un nuevo almacén de datos de VMware, un nuevo entorno de desarrollo y pruebas (dev/test), una base de datos SQL o un registro.*

- **¿Cuántos volúmenes de bloque adicionales se necesitan por tipo, tamaño, IOPS y ubicación?** <br />
*Por ejemplo, su respuesta podría ser similar a "25x Resistencia 2 TB @ 4 IOPS en DAL09" o "25x Rendimiento 4 TB @ 2 IOPS en WDC04".*

- **¿Cuántos volúmenes de archivos adicionales se necesitan por tipo, tamaño, IOPS y ubicación?** <br />
*Por ejemplo, su respuesta podría ser similar a "25x Rendimiento 20 GB @ 10 IOPS en DAL09" o "50x Resistencia 2 TB @ 0,25 IOPS en SJC03".*

- **Proporcione una estimación de cuándo espera o planea suministrar todo el aumento de volumen solicitado.** <br />
 "*Por ejemplo, su respuesta podría ser similar a "90 días".*

- **Proporcione una previsión de 90 días del promedio de uso de capacidad esperado de estos volúmenes.** <br />
*Por ejemplo, su respuesta podría ser similar a "esperar una utilización del 25 % en 30 días, 50 % en 60 días y 75 % en 90 días".*

Responda a todas las preguntas y sentencias de la solicitud. Son necesarias para el proceso y la aprobación.
{:important}

Se le notificará la actualización de sus límites a través del proceso de incidencia.
