---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: MPIO iSCSI LUNS, iSCSI Target, MPIO, multipath, block storage, LUN, mounting, mapping secondary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# Conexión a las LUN iSCSI en Microsoft Windows
{: #mountingWindows}

Antes de empezar, asegúrese de que el host que está accediendo al volumen de {{site.data.keyword.blockstoragefull}} se haya autorizado a través de la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic){: external}.

1. En la página de listado de {{site.data.keyword.blockstorageshort}}, localice el nuevo volumen y pulse **Acciones**. Pulse **Autorizar host**.
2. En la lista, seleccione el host o los hosts que accederán al volumen y pulse **Enviar**.

De manera alternativa, puede autorizar el host mediante SLCLI.
```
# slcli block access-authorize --help
Uso: slcli block access-authorize [OPCIONES] ID_VOLUMEN

Opciones:
  -h, --hardware-id TEXT    El ID del servidor de hardware que se va a autorizar.
  -v, --virtual-id TEXT     El ID de un servidor virtual que se va a autorizar.
  -i, --ip-address-id TEXT  El ID de una dirección IP que se va a autorizar.
  -p, --ip-address TEXT     Una dirección IP que se va a autorizar.
  --help                    Mostrar este mensaje y salir.
```
{:codeblock}

## Montaje de volúmenes de {{site.data.keyword.blockstorageshort}}
{: #mountWin}

Siga estos pasos para conectar una instancia de cálculo de {{site.data.keyword.cloud}} basada en Windows a un número de unidad lógica (LUN) de interfaz para pequeños sistemas (iSCSI) de E/S de multivía de acceso (MPIO). El ejemplo se basa en Windows Server 2012. Los pasos pueden ajustarse para otras versiones de Windows de acuerdo con la documentación del proveedor del sistema operativo (SO).

### Configuración de la característica MPIO

1. Inicie Server Manager y vaya a **Gestionar**, **Añadir roles y características**.
2. Pulse **Siguiente** para ir al menú de características.
3. Desplácese hacia abajo y marque **E/S de multivía de acceso**.
4. Pulse **Instalar** para instalar MPIO en el servidor del host.
![Adición de roles y características en Server Manager](/images/Roles_Features.png)

### Adición de soporte de iSCSI para MPIO

1. Abra la ventana Propiedades de MPIO pulsando **Inicio**, apuntando a **Herramientas administrativas** y pulsando **MPIO**.
2. Pulse **Descubrir multivías de acceso**.
3. Marque el recuadro de selección **Añadir soporte para dispositivos iSCSI** y, a continuación, pulse **Añadir**. Cuando se le solicite reiniciar el sistema, pulse **Sí**.

En Windows Server 2008, añadir soporte para iSCSI permite que Microsoft Device Specific Module (MSDSM) reclame todos los dispositivos iSCSI para MPIO, lo que requiere una conexión a un destino de iSCSI en primer lugar.
{:note}

### Configuración del iniciador iSCSI

1. Inicie el iniciador iSCSI desde Server Manager y seleccione **Herramientas**, **Iniciador iSCSI**.
2. Pulse el separador **Configuración**.
    - Puede que el campo de nombre de iniciador ya esté rellenado con una entrada similar a `iqn.1991-05.com.microsoft:`.
    - Pulse **Cambiar** para sustituir los valores existentes por su nombre calificado iSCSI (IQN).
    ![Propiedades del iniciador de iSCSI](/images/iSCSI.png)

      Encontrará el nombre de IQN en la pantalla Detalles de {{site.data.keyword.blockstorageshort}} de la [consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}/classic){: external}.
      {: tip}

    - Pulse **Descubrir** y luego pulse **Descubrir portal**.
    - Especifique la dirección IP del destino iSCSI y para el puerto el valor predeterminado, 3260.
    - Pulse **Avanzado** para abrir la ventana Configuración avanzada.
    - Seleccione **Habilitar inicio de sesión CHAP** para activar la autenticación CHAP.
    ![Habilitar inicio de sesión CHAP](/images/Advanced_0.png)

    Los campos Nombre y Secreto de destino distinguen entre mayúsculas y minúsculas.
    {:important}
         - En el campo **Nombre**, suprima las entradas existentes y especifique el nombre de usuario de la [consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}/classic/storage){: external}.
         - En el campo **Secreto de destino**, escriba la contraseña de la [consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}/classic/storage){: external}.
    - Pulse **Aceptar** en las ventanas **Configuración avanzada** y **Descubrir portal de destino** para volver a la pantalla principal de propiedades del iniciador de iSCSI. Si recibe errores de autenticación, compruebe las entradas de nombre de usuario y contraseña.
    ![Destino inactivo](/images/Inactive_0.png)

    El nombre de su destino aparece en la sección Destinos descubiertos con un estado `Inactivo`.
    {:note}


### Activación de destino

1. Pulse **Conectar** para conectarse al destino.
2. Marque el recuadro de selección **Habilitar multivía de acceso** para habilitar la E/S de multivía de acceso en el destino. <br/>
   ![Habilitar multivía de acceso](/images/Connect_0.png)
3. Pulse **Avanzado** y seleccione **Habilitar inicio de sesión CHAP**.
</br>
   ![Habilitar CHAP](/images/chap_0.png)
4. Especifique el nombre de usuario en el campo Nombre y especifique la contraseña en el campo secreto de destino.

   Los valores de los campos Nombre y Secreto de destino se pueden obtener en la pantalla Detalles de {{site.data.keyword.blockstorageshort}}.
   {:tip}
5. Pulse **Aceptar** hasta que aparezca la ventana **Propiedades del iniciador de iSCSI**. El estado del destino en la sección **Destinos descubiertos** pasa de **Inactivo** a **Conectado**.
![Estado Conectado](/images/Connected.png)


### Configuración de MPIO en el iniciador iSCSI

1. Inicie el iniciador iSCSI y, en el separador Destinos, pulse **Propiedades**.
2. Pulse **Añadir sesión** en la ventana Propiedades.
3. En el recuadro de diálogo Conectar con destino, marque el recuadro de selección **Habilitar multivía de acceso** y pulse **Avanzado**.
  ![Destino](/images/Target.png)

4. En la ventana Configuración avanzada ![Configuración](/images/Settings.png)
   - En la lista de adaptadores locales, seleccione Microsoft iSCSI Initiator.
   - En la lista de IP de iniciador, seleccione la dirección IP del host.
   - En la lista de IP de portal de destino, seleccione la IP de la interfaz del dispositivo.
   - Marque el recuadro de selección **Habilitar inicio de sesión CHAP**
   - Escriba los valores secretos Nombre y Destino obtenidos de la consola y pulse **Aceptar**.
   - Pulse **Aceptar** en la ventana Conectar a destino para volver a la ventana Propiedades.

5. Pulse **Propiedades**. En el recuadro de diálogo Propiedades, vuelva a pulsar **Añadir sesión** para añadir la segunda vía de acceso.
6. En la ventana Conectar con destino, marque el recuadro de selección **Habilitar multivía**. Pulse **Avanzado**.
7. En la ventana Configuración avanzada,
   - En la lista de adaptadores locales, seleccione Microsoft iSCSI Initiator.
   - En la lista de IP de iniciador, seleccione la dirección IP correspondiente al host. En este caso, va a conectar dos interfaces de red del dispositivo de almacenamiento a una sola interfaz de red del host. Por lo tanto, la interfaz es la misma que la proporcionada para la primera sesión.
   - En la lista de IP de portal de destino, seleccione la dirección IP de la segunda interfaz de datos que está habilitada en el dispositivo de almacenamiento.

     Encontrará la segunda dirección IP en la pantalla Detalles de {{site.data.keyword.blockstorageshort}} de la [consola de {{site.data.keyword.cloud_notm}}](https://{DomainName}/classic/storage){: external}.
      {: tip}
   - Marque el recuadro de selección **Habilitar inicio de sesión CHAP**
   - Escriba los valores secretos Nombre y Destino obtenidos de la consola y pulse **Aceptar**.
   - Pulse **Aceptar** en la ventana Conectar a destino para volver a la ventana Propiedades.
8. Ahora la ventana Propiedades muestra más de una sesión dentro del panel Identificador. Tiene más de una sesión en el almacenamiento de iSCSI.

   Si el host tiene varias interfaces que desea conectar con el almacenamiento ISCSI, puede configurar otra conexión con la dirección IP del otro NIC en el campo de IP del iniciador. Sin embargo, asegúrese de autorizar a la segunda dirección IP del iniciador en la [consola de {{site.data.keyword.cloud}}](https://{DomainName}/classic/storage){: external} antes de intentar establecer la conexión.
   {:note}
9. En la ventana Propiedades, pulse **Dispositivos** para abrir la ventana Dispositivos. El nombre de la interfaz de dispositivo empieza por `mpio`. <br/>
  ![Dispositivos](/images/Devices.png)

10. Pulse **MPIO** para abrir la ventana **Detalles de dispositivo**. Puede elegir las políticas de equilibrio de carga para MPIO en esta ventana, que también muestra las vías de acceso a iSCSI. En este ejemplo, dos vías de acceso se muestran como disponibles para MPIO con una política de equilibrio de carga Round Robin con subconjunto.
  ![La ventana Detalles del dispositivo muestra dos vías de acceso disponibles para MPIO con una política de equilibrio de carga Round Robin con subconjunto](/images/DeviceDetails.png)

11. Pulse **Aceptar** varias veces para salir del iniciador iSCSI.



## Verificación de si MPIO se ha configurado correctamente en sistemas operativos Windows
{: #verifyMPIOWindows}

Para verificar si MPIO de Windows está configurado, primero debe asegurarse de que el complemento MPIO esté habilitado y debe reiniciar el servidor.

![Características_Roles_0](/images/Roles_Features_0.png)

Una vez completado el reinicio y añadido el dispositivo de almacenamiento, puede verificar si MPIO se ha configurado y funciona. Para ello, vaya a **Detalles del dispositivo de destino** y pulse **MPIO**:
![DetallesDispositivo_0](/images/DeviceDetails_0.png)

Si MPIO no se ha configurado correctamente, el dispositivo de almacenamiento se puede desconectar y aparecer inhabilitado cuando se produzca un corte en la red o cuando los equipos de {{site.data.keyword.cloud}} realizan tareas de mantenimiento. MPIO garantiza un nivel adicional de conectividad durante estos casos y mantiene una sesión establecida con operaciones de lectura/escritura activas en la LUN.

## Desmontaje de volúmenes de {{site.data.keyword.blockstorageshort}}
{: #unmountingWin}

A continuación se describen los pasos necesarios para desconectar una instancia de cálculo de {{site.data.keyword.Bluemix_short}} basada en Windows a un LUN de iSCSI de MPIO. El ejemplo se basa en Windows Server 2012. Los pasos pueden ajustarse para otras versiones de Windows de acuerdo con la documentación del proveedor del sistema operativo.

### Inicio del iniciador iSCSI

1. Pulse **Destinos**.
2. Seleccione los destinos que desee eliminar y pulse **Desconectar**.

### Eliminar destinos
Este paso es opcional, para cuando ya no necesite acceder a los destinos iSCSI.

1. Pulse **Descubrimiento** en el iniciador de iSCSI.
2. Marque el portal de destino asociado a su volumen de almacenamiento y pulse **Eliminar**.
