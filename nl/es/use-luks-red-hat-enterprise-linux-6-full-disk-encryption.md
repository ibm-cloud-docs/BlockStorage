---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Uso de LUKS en Red Hat Enterprise Linux para el cifrado de disco completo

Linux Unified Key Setup-on-disk-format (LUKS) le permite cifrar particiones en Red Hat Enterprise Linux 6 (servidor), lo que resulta especialmente importante cuando se trata de sistemas móviles y soportes extraíbles. LUKS permite que múltiples claves de usuario descifren una clave maestra que se utiliza para el cifrado masivo de la partición.

## Qué hace LUKS

- Cifrar dispositivos de bloque enteros y, por tanto, es ideal para proteger el contenido de dispositivos móviles, como soportes de almacenamiento extraíbles o unidades de disco de portátiles.
    - El contenido subyacente del dispositivo de bloque cifrado es arbitrario, por lo que resulta útil para cifrar dispositivos de intercambio. El cifrado también es útil con determinadas bases de datos que utilizan dispositivos de bloque con formato especial para el almacenamiento de datos.
- Utilizar el subsistema correlacionador de dispositivos de correlacionador de dispositivos existente.
- Proporcionar refuerzo de contraseña para proteger frente a conexiones de diccionario.
- Permitir a los usuarios añadir claves de copia de seguridad porque los dispositivos LUKS contienen múltiples ranuras de claves.


## Qué no hace LUKS

- Permitir que las aplicaciones que requieren muchos usuarios (más de ocho) tengan claves de acceso distintas al mismo dispositivo.
- Trabajar con aplicaciones que requieren cifrado a nivel de archivos, [más información](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Security_Guide/sec-Encryption.html){:new_window}.

## Cómo configurar un nuevo volumen cifrado de LUKS con {{site.data.keyword.blockstorageshort}} de Resistencia

En estos pasos se supone que el servidor ya tiene acceso a un nuevo volumen de {{site.data.keyword.blockstoragefull}} no cifrado que no se ha formateado ni montado. Pulse [aquí](accessing_block_storage_linux.html) para consultar cómo acceder a {{site.data.keyword.blockstorageshort}} con Linux.

Tenga en cuenta que al realizar el cifrado de datos se crea una carga en el host que podría afectar potencialmente al rendimiento.

1. Escriba lo siguiente en el indicador de shell como usuario root para instalar el paquete necesario:   <br/>
   ```
   # yum install cryptsetup-luks
   ```
   {: pre}
2. Obtenga el ID de disco:<br/>
   ```
   # fdisk –l | grep /dev/mapper
   ```
   {: pre}
3. Localice el volumen en el listado.
4. Cifre el dispositivo de bloque; 

   1. este mandato inicializa el volumen y le permite establecer una contraseña: <br/>
   
      ```
      # cryptsetup -y -v luksFormat /dev/mapper/3600a0980383034685624466470446564
      ```
      {: pre}
      
   2. Responda con YES (todo en mayúsculas).
   
   3. El dispositivo ahora aparecerá como un volumen cifrado: 
   
      ```
      # blkid | grep LUKS
      /dev/mapper/3600a0980383034685624466470446564: UUID="46301dd4-035a-4649-9d56-ec970ceebe01" TYPE="crypto_LUKS"
      ```
      
5. Abra el volumen y cree una correlación:   <br/>
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
   ```
   {: pre}
6. Escriba la contraseña que se le ha proporcionado anteriormente.
7. Verifique la correlación y compruebe el estado del volumen cifrado:   <br/>
   ```
   # cryptsetup -v status cryptData
   /dev/mapper/cryptData is active.
     type:  LUKS1
     cipher:  aes-cbc-essiv:sha256
     keysize: 256 bits
     device:  /dev/mapper/3600a0980383034685624466470446564
     offset:  4096 sectors
     size:    41938944 sectors
     mode:    read/write
     Command successful
   ```
8. Escriba datos aleatorios en el dispositivo cifrado /dev/mapper/cryptData¡. Esto garantiza que el mundo exterior verá esto como datos aleatorios, lo que significa que está protegido contra la divulgación de patrones de uso. Tenga en cuenta que este paso puede tardar un tiempo.<br/>
    ```
    # shred -v -n1 /dev/mapper/cryptData
    ```
    {: pre}
9. De formato al volumen:<br/>
   ```
   # mkfs.ext4 /dev/mapper/cryptData
   ```
   {: pre}
10. Monte el volumen:<br/>
   ```
   # mkdir /cryptData
   ```
   {: pre}
   ```
   # mount /dev/mapper/cryptData /cryptData
   ```
   {: pre}
   ```
   # df -H /cryptData
   ```
   {: pre}

### Cómo desmontar y cerrar el volumen cifrado de forma segura
   ```
   # umount /cryptData
   # cryptsetup luksClose cryptData
   ```
   {: codeblock}

### Cómo volver a montar y montar una partición cifrada de LUKS existente
   ```
   # cryptsetup luksOpen /dev/mapper/3600a0980383034685624466470446564 cryptData
      Enter the password previously provided.
   # mount /dev/mapper/cryptData /cryptData
   # df -H /cryptData
   # lsblk
   NAME                                       MAJ:MIN RM  SIZE RO TYPE  MOUNTPOINT
   xvdb                                       202:16   0    2G  0 disk
   +-xvdb1                                    202:17   0    2G  0 part  [SWAP]
   xvda                                       202:0    0   25G  0 disk
   +-xvda1                                    202:1    0  256M  0 part  /boot
   +-xvda2                                    202:2    0 24.8G  0 part  /
   sda                                          8:0    0   20G  0 disk
   +-3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   +-cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   sdb                                          8:16   0   20G  0 disk
   +-3600a0980383034685624466470446564 (dm-0) 253:0    0   20G  0 mpath
   +-cryptData (dm-1)                       253:1    0   20G  0 crypt /cryptData
   ```
