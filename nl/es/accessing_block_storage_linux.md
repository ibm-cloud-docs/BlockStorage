---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

keywords: MPIO, iSCSI LUNs, multipath configuration file, RHEL6, multipath, mpio, linux,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}


# Conexión a las LUN iSCSI en Linux
{: #mountingLinux}

Estas instrucciones se aplican principalmente a RHEL6 y CentOS6. Se han añadido notas para otros sistemas operativos, pero la documentación **no** cubre todas las distribuciones Linux. Si está utilizando otros sistemas operativos Linux, consulte la documentación de la distribución específica y asegúrese de que la multivía dé soporte a ALUA para la prioridad de vía de acceso.
{:note}

Por ejemplo, para obtener más información sobre temas específicos de Ubuntu, consulte [Configuración del iniciador de iSCSI](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){: external} y [DM-Multipath](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){: external}.
{: tip}

Antes de empezar, asegúrese de que se ha autorizado al host que accede al volumen de {{site.data.keyword.blockstoragefull}} mediante la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.
{:important}

1. Inicie una sesión en la [consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}/){: external}. En el **menú**, seleccione **Infraestructura clásica**.
2. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
3. Localice el nuevo volumen y pulse **...**.
4. Pulse **Autorizar host**.
5. Para ver la lista de dispositivos o direcciones IP disponibles, primero seleccione si desea autorizar el acceso basado en tipo de dispositivo o subredes.
   - Si elige Dispositivos, puede seleccionar entre instancias de Servidor nativo o Servidor virtual.
   - Si elige Dirección IP, primero seleccione la subred donde reside su host.
6. En la lista filtrada, seleccione uno o más hosts que pueden acceder al volumen y pulse **Guardar**.

De manera alternativa, puede autorizar el host mediante SLCLI.
```
# slcli block access-authorize --help
Uso: slcli block access-authorize [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXTO   El ID del servidor de hardware que se va a autorizar.
  -v, --virtual-id TEXTO    El ID de un servidor virtual que se va a autorizar.
  -i, --ip-address-id TEXTO El ID de una dirección IP que se va a autorizar.
  -p, --ip-address TEXTO    Una dirección IP que se va a autorizar.
  --help                    Mostrar este mensaje y salir.
```
{:codeblock}

## Montaje de volúmenes de {{site.data.keyword.blockstorageshort}}
{: #mountLin}

Siga estos pasos para conectar una instancia de cálculo de {{site.data.keyword.cloud}} basada en Linux a un número de unidad lógica (LUN) de interfaz para pequeños sistemas (iSCSI) de E/S de multivía de acceso (MPIO).

Encontrará el IQN de host, el nombre de usuario, la contraseña y la dirección de destino a los que se hace referencia en las instrucciones en la pantalla **Detalle de {{site.data.keyword.blockstorageshort}}** de la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic/storage){: external}.
{: tip}

Es mejor ejecutar el tráfico de almacenamiento en una VLAN, que omita el cortafuegos. La ejecución del tráfico de almacenamiento a través de cortafuegos de software aumenta la latencia y afecta negativamente al rendimiento del almacenamiento.
{:important}

1. Instale los programas de utilidad multivía e iSCSI en el host.
  - RHEL y CentOS

    ```
    yum install iscsi-initiator-utils device-mapper-multipath
    ```
    {: pre}

  - Ubuntu y Debian

    ```
    sudo apt-get update
   sudo apt-get install multipath-tools
    ```
    {: pre}

2. Cree o edite el archivo de configuración multivía si se necesita.
  - RHEL 6 y CENTOS 6
    * Edite **/etc/multipath.conf** con la siguiente configuración mínima.

      ```
      defaults {
      user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
      # Todos los datos bajo blacklist deben ser específicos de su sistema.
      blacklist {
      wwid "SAdaptec*"
   devnode "^hd[a-z]"
   devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]*"
      devnode "^cciss.*"  
      }
      devices {
      device {
      vendor "NETAPP"
   product "LUN"
   path_grouping_policy group_by_prio
   features "3 queue_if_no_path pg_init_retries 50"
   prio "alua"
   path_checker tur
   failback immediate
   path_selector "round-robin 0"
   hardware_handler "1 alua"
   rr_weight uniform
   rr_min_io 128
   }
      }
      ```
      {: codeblock}

    - Reinicie los servicios `iscsi` e `iscsid` para que los cambios entren en vigor.

      ```
      service iscsi restart
      service iscsid restart
      ```
      {: pre}

  - En RHEL7 y CentOS7, `multipath.conf` puede estar en blanco porque el sistema operativo tiene configuraciones integradas.
  - Ubuntu no utiliza `multipath.conf` porque se basa en `multipath-tools`.

3. Cargue el módulo multivía, inicie los servicios de multivía y configúrelo para iniciarse al arrancar.
  - RHEL 6
    ```
    modprobe dm-multipath
    ```
    {: pre}

    ```
    service multipathd start
    ```
    {: pre}

    ```
    chkconfig multipathd on
    ```
    {: pre}

  - CentOS 7
    ```
    modprobe dm-multipath
    ```
    {: pre}

    ```
    systemctl start multipathd
    ```
    {: pre}

    ```
    systemctl enable multipathd
    ```
    {: pre}

  - Ubuntu
    ```
    service multipath-tools start
    ```
    {: pre}

  - Para otras distribuciones, consulte la documentación del proveedor del sistema operativo.

4. Verifique que la multivía esté funcionando.
  - RHEL 6
    ```
    multipath -l
    ```
    {: pre}

    Si se devuelve en blanco, está funcionando.
  - CentOS 7
    ```
    multipath -ll
    ```
    {: pre}

    RHEL 7 y CentOS 7 pueden devolver el mensaje No fc_host device, que se puede pasar por alto.

5. Actualice el archivo `/etc/iscsi/initiatorname.iscsi` con el nombre calificado iSCSI (IQN) que obtendrá en la consola de {{site.data.keyword.cloud}}. Escriba el valor en minúsculas.

   ```
   InitiatorName=<valor-del-Portal>
   ```
   {: pre}

6. Edite los valores de CHAP en `/etc/iscsi/iscsid.conf` con el nombre de usuario y la contraseña que verá en la consola de {{site.data.keyword.cloud}}. Utilice mayúsculas para los nombres de CHAP.
   ```
   node.session.auth.authmethod = CHAP
    node.session.auth.username = <Valor-nombreUsuario-del-Portal>
    node.session.auth.password = <Valor-contraseña-del-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Valor-nombreUsuario-del-Portal>
    discovery.sendtargets.auth.password = <Valor-contraseña-del-Portal>
   ```
   {: codeblock}

   Deje los otros valores de CHAP que se han comentado. El almacenamiento de {{site.data.keyword.cloud}} utiliza únicamente autenticación unidireccional. No habilite Mutual CHAP.
   {:important}

   Si es usuario de Ubuntu, cuando consulte el archivo `iscsid.conf`, mire si el valor `node.startup` es manual o automatic. Si es manual, cámbielo por automatic.
   {:tip}

7. Establezca iSCSI para que se inicie al arrancar e inícielo ahora.
  - RHEL 6
    ```
    chkconfig iscsi on
    ```
    {: pre}

    ```
    chkconfig iscsid on
    ```
    {: pre}

    ```
    service iscsi start
    ```
    {: pre}

    ```
    service iscsid start
    ```
    {: pre}

  - CentOS 7
    ```
    systemctl enable iscsi
    ```
    {: pre}

    ```
    systemctl enable iscsid
    ```
    {: pre}

    ```
    systemctl start iscsi
    ```
    {: pre}

    ```
    systemctl start iscsid
    ```
    {: pre}

   - Para otras distribuciones, consulte la documentación del proveedor del sistema operativo.

8. Descubra el dispositivo utilizando la dirección IP de destino obtenida de la consola de {{site.data.keyword.cloud}}.

   A. Ejecute el descubrimiento en la matriz de iSCSI.
    ```
    iscsiadm -m discovery -t sendtargets -p <ip-value-from-IBM-Cloud-console>
    ```
    {: pre}

   B. Establezca el host para que inicie sesión automáticamente en la matriz de iSCSI.
    ```
    iscsiadm -m node -L automatic
    ```
    {: pre}

9. Verifique que el host ha iniciado sesión en la matriz de iSCSI y que ha mantenido sus sesiones.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Este mandato informa de las vías de acceso.

10. Verifique que el dispositivo está conectado. De forma predeterminada, el dispositivo se conecta a `/dev/mapper/mpathX`, donde X es el ID generado del dispositivo conectado.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}

  Este mandato informa de algo similar al ejemplo siguiente.
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 bytes
    ```

  El volumen ahora está montado y es accesible en el host.

## Creación de un sistema de archivos (opcional)

Siga estos pasos para crear un sistema de archivos en el volumen montado recientemente. Un sistema de archivos es necesario para que la mayoría de las aplicaciones utilicen el volumen. Utilice [`fdisk` para unidades inferiores a 2 TB](#fdisk) y [`parted` para discos de más de 2 TB](#parted).

### Creación de un sistema de archivos con `fdisk`
{: #fdisk}

1. Obtenga el nombre de disco.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   El nombre de disco que se devuelve tiene un aspecto similar a `/dev/mapper/XXX`.

2. Cree una partición en el disco.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   XXX representa el nombre del disco que se devuelve en el Paso 1. <br />

   Desplácese hacia abajo por los códigos de mandatos que se muestran en la tabla de mandatos de `fdisk`.
   {: tip}

3. Cree un sistema de archivos en la nueva partición.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - La nueva partición se lista con el disco, similar a `XXXlp1`, seguido por el tamaño, tipo (83) y Linux.
   - Anote el nombre de partición, lo necesita en el siguiente paso. (XXXlp1 representa el nombre de partición)
   - Cree el sistema de archivos:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Cree un punto de montaje para el sistema de archivos y móntelo.
   - Cree un nombre de partición `PerfDisk` o donde quiera montar el sistema de archivos.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Monte el almacenamiento con el nombre de la partición.
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Compruebe que ve el nuevo sistema de archivos que aparece en la lista.
     ```
     df -h
     ```
     {: pre}

5. Añada el nuevo sistema de archivos al archivo `/etc/fstab` del sistema para habilitar el montaje automático al arrancar.
   - Añada la siguiente línea al final de `/etc/fstab` (con el nombre de partición del Paso 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Tabla de mandatos `fdisk`

| Mandato | Resultado |
|-----|-----|
| `Command: n`| Crea una partición. * |
| `Command action: p` | Hace que la partición sea la primaria. |
| `Partition number (1-4): 1` | Se convierte en la partición 1 del disco. |
| `First cylinder (1-8877): 1 (default)` | Inicia en el cilindro 1. |
| `Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)` | Pulse Intro para ir al último cilindro. |
| `Command: t` | Configura el tipo de partición. * |
| `Select partition 1.` | Selecciona la partición 1 para configurarla como un tipo específico. |
| `Hex code: 83` | Selecciona Linux como el Tipo (83 es el código hexadecimal para Linux). ** |
| `Command: w` | Escribe la información de la nueva partición en el disco. ** |
{: caption="Tabla 1 - La tabla de mandatos <code>fdisk</code> muestra los mandatos a la izquierda y los resultados esperados a la derecha." caption-side="top"}

(`*`) Escriba m para obtener ayuda.

(`**`) Escriba L para ver la lista de códigos hexadecimales

### Creación de un sistema de archivos con `parted`
{: #parted}

En muchas distribuciones Linux, la opción `parted` viene preinstalada. Si no se incluye en su distribución, puede instalarla con:
- Debian y Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL y CentOS
  ```
  yum install parted  
  ```
  {: pre}


Para crear un sistema de archivos con `parted`, siga estos pasos.

1. Ejecute `parted`.

   ```
   parted
   ```
   {: pre}

2. Cree una partición en el disco.
   1. A menos que se especifique lo contrario, `parted` utiliza la unidad primaria, que la mayoría de las veces es `/dev/sda`. Cambie al disco en el que quiera la partición utilizando el mandato **select**. Sustituya **XXX** por el nuevo nombre de dispositivo.

      ```
      select /dev/mapper/XXX
      ```
      {: pre}

   2. Ejecute `print` para confirmar que está en el disco correcto.

      ```
      print
      ```
      {: pre}

   3. Cree una tabla de partición GPT.

      ```
      mklabel gpt
      ```
      {: pre}

   4. `Parted` se puede utilizar para crear particiones de disco lógicas y primarias, los pasos a seguir son los mismos. Para crear una partición, `parted` utiliza `mkpart`. Puede especificar otros parámetros como **primaria** o **lógica** en función del tipo de partición que quiera crear.<br />

   El valor predeterminado de las unidades de la lista es megabytes (MB). Para crear una partición de 10 GB, deberá empezar en 1 y acabar en 10000. También puede cambiar las unidades de tamaño a terabytes especificando `unit TB`, si lo desea.
   {: tip}

      ```
      mkpart
      ```
      {: pre}

   5. Salga de `parted` con `quit`.

      ```
      quit
      ```
      {: pre}

3. Cree un sistema de archivos en la nueva partición.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   Es importante seleccionar el disco y la partición adecuados al ejecutar este mandato.<br />Compruebe el resultado imprimiendo la tabla de partición. En la columna del sistema de archivos, puede ver ext3.
   {:important}

4. Cree un punto de montaje para el sistema de archivos y móntelo.
   - Cree un nombre de partición `PerfDisk` o donde quiera montar el sistema de archivos.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Monte el almacenamiento con el nombre de la partición.

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Compruebe que ve el nuevo sistema de archivos que aparece en la lista.

     ```
     df -h
     ```
     {: pre}

5. Añada el nuevo sistema de archivos al archivo `/etc/fstab` del sistema para habilitar el montaje automático al arrancar.
   - Añada la siguiente línea al final de `/etc/fstab` (utilizando el nombre de partición del Paso 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}




## Verificación de la configuración de MPIO
{: #verifyMPIOLinux}

1. Para comprobar si la multivía está recogiendo dispositivos, liste los dispositivos. Si se ha configurado correctamente, solo se mostrarán dos dispositivos NETAPP.

  ```
  multipath -l
  ```
  {: pre}

  ```
  root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
  6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
  7:0:0:101 sde 8:64 active undef running
  ```

2. Compruebe que los discos estén presentes. Debería ver dos discos con el mismo identificador, y un listado `/dev/mapper` del mismo tamaño con el mismo identificador. El dispositivo `/dev/mapper` es el que configura la multivía.
  ```
  fdisk -l | grep Disk
  ```
  {: pre}

  - Ejemplo de salida de una configuración correcta.

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```
  - Salidas de ejemplo de una configuración incorrecta.

    ```
    No multipath output root@server:~# multipath -l root@server:~#
    ```

    ```
    root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
    ```

3. Confirme que los discos locales no se incluyen en los dispositivos multivía. El siguiente mandato muestra los dispositivos que están en la lista negra.
   ```
   multipath -l -v 3 | grep sd <date and time>
   ```
   {: pre}

   ```
   root@server:~# multipath -l -v 3 | grep sd Feb 17 19:55:02
| sda: device node name blacklisted Feb 17 19:55:02
| sdb: device node name blacklisted Feb 17 19:55:02
| sdc: device node name blacklisted Feb 17 19:55:02
| sdd: device node name blacklisted Feb 17 19:55:02
| sde: device node name blacklisted Feb 17 19:55:02
   ```

## Desmontaje de volúmenes de {{site.data.keyword.blockstorageshort}}
{: #unmountingLin}

1. Desmonte el sistema de archivos.
   ```
   umount /dev/mapper/XXXlp1 /PerfDisk
   ```
   {: pre}

2. Si no tiene ningún otro volumen en el portal de destino, puede finalizar la sesión del destino.
   ```
   iscsiadm -m node -t <TARGET NAME> -p <PORTAL IP:PORT> --logout
   ```
   {: pre}

3. Si no tiene ningún otro volumen en el portal de destino, suprima el registro del portal de destino para evitar futuros intentos de inicio de sesión.
   ```
   iscsiadm -m node -o delete -t <TARGET IQN> -p <PORTAL IP:PORT>
   ```
   {: pre}

   Para obtener más información, consulte el manual [`iscsiadm`](https://linux.die.net/man/8/iscsiadm).
   {:tip}
