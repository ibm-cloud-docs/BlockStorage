---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Protección de los datos: cifrado en reposo gestionado por el proveedor

## Cifrado en reposo de {{site.data.keyword.blockstorageshort}} 

{{site.data.keyword.BluSoftlayer_full}} se toma en serio la seguridad y comprende la importancia de cifrar los datos para mantenerlos protegidos. Mediante el cifrado gestionado por proveedor, los datos de {{site.data.keyword.blockstoragefull}} suministrados con la opción de Resistencia o de Rendimiento se cifran de forma predeterminada sin coste adicional ni impacto sobre el rendimiento.

La característica de cifrado en reposo gestionado por proveedor utiliza los siguientes protocolos estándar del sector:

* Cifrado AES-256 estándar del sector
* Las claves se gestionan internamente con el protocolo estándar del sector Key Management Interoperability Protocol (KMIP)
* El almacenamiento cumple con los estándares Federal Information Processing Standard (FIPS) Publication 140-2 validated for Federal Information Security Management Act (FISMA) y Health Insurance Portability and Accountability Act (HIPAA). También se ha validado el cumplimiento del almacenamiento con los estándares Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) y EU Data Protection Directive 95/46/EC.

## Cifrado en reposo para instantáneas o almacenamiento replicado  

Todas las instantáneas y réplicas de {{site.data.keyword.blockstorageshort}} cifrados también se cifran de forma predeterminada. Esta característica no se puede desactivar por volumen.

## Suministro del almacenamiento con cifrado

La característica de cifrado en reposo gestionado por el proveedor solo está disponible para {{site.data.keyword.blockstorageshort}} suministrado en [determinados centros de datos](new-ibm-block-and-file-storage-location-and-features.html). Todo el almacenamiento suministrado en estos centros de datos se suministra automáticamente con el cifrado de datos en reposo.

Al realizar el pedido de {{site.data.keyword.blockstorageshort}}, seleccione un centro de datos marcado con un asterisco (*) (`*`). Verá un icono de bloqueo a la derecha del campo Nombre de volumen/LUN, que indica que está cifrado.

![El icono de bloqueo indica que el LUN está cifrado](/images/encryptedstorage.png)
<caption>Figura 1. Ejemplo del icono de bloqueo que indica que el LUN está cifrado.</caption>



**Nota**: el almacenamiento no cifrado suministrado antes de que se actualizara el centro de datos **no** se cifrará automáticamente. Si tiene almacenamiento no cifrado en un centro de datos actualizado, tendrá que crear un nuevo LUN o volumen y realizar una migración de datos. El siguiente artículo puede proporcionarle orientación:

* [Migración de {{site.data.keyword.blockstorageshort}} en centros de datos actualizados](migrate-block-storage-encrypted-block-storage.html)
