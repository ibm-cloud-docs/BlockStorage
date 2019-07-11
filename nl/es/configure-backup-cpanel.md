/---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: Almacenamiento en bloque, cPanel, copias de seguridad, punto de montaje, ISCSI

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configuración de {{site.data.keyword.blockstorageshort}} para la copia de seguridad con cPanel
{: #cPanelBackups}

Puede seguir las siguientes instrucciones para configurar sus copias de seguridad en cPanel para que se almacenen en {{site.data.keyword.blockstoragefull}}. Suponemos que está disponible el acceso de SSH sudo o root y de WebHost Manager (WHM) completo. Estas instrucciones se basan en un host **CentOS 7**.

Para obtener más información, consulte [cPanel - Configuración del directorio de copia de seguridad](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){: external}.
{:tip}

1. Conéctese al host a través de SSH.

2. Asegúrese de que un exista un destino de punto de montaje. <br />
   De forma predeterminada, el sistema cPanel guarda los archivos de copia de seguridad en local, en el directorio `/backup`. Para los fines de este documento, se supone que `/backup` existe y contiene copias de seguridad, y `/backup2` se utiliza como el nuevo punto de montaje.
   {:note}

3. Configure su {{site.data.keyword.blockstorageshort}} como se describe en [Conexión a las LUN de iSCSI en Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Asegúrese de montarlo en `/backup2` y configurarlo en `/etc/fstab` para habilitar el montaje en el inicio.

4. **Opcional**: Copie las copias de seguridad existentes en el nuevo almacenamiento. Puede utilizar `rsync`.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Este mandato comprime y transfiere los datos, a la vez que conserva todo lo posible, excepto los enlaces fijos. Proporciona información sobre los archivos que se están transfiriendo, además de un breve resumen final.
    {:tip}

5. Inicie sesión en WHM y vaya a la configuración de copia de seguridad pulsando **Inicio** > **Copia de seguridad** > **Configuración de copia de seguridad**.

6. Edite la configuración para guardar las copias seguridad en el nuevo punto de montaje.
    - Cambie el directorio de copia de seguridad predeterminado especificando la vía de acceso absoluta a la nueva ubicación en lugar del directorio /backup/.
    - Seleccione **Habilitar el montaje de una unidad de copia de seguridad**. Este valor hace que el proceso de configuración de copia de seguridad compruebe el archivo `/etc/fstab` para el montaje de la copia de seguridad (`/backup2`). <br />

    Si existe un montaje con el mismo nombre que el directorio intermedio, el proceso de configuración de copia de seguridad monta la unidad y realiza la copia de seguridad de la información en la unidad. Cuando el proceso de copia de seguridad finaliza, desmonta la unidad.
    {:note}

7. Aplique los cambios pulsando **Guardar configuración**.

8. **Opcional**: en función de sus necesidades específicas, elimine el almacenamiento antiguo del servidor y cancele desde la cuenta.
