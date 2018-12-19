---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-30"

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Duplicación de volúmenes de réplica para la recuperación tras desastre

En el caso de que se produzca una anomalía catastrófica o que un desastre ocasione la caída del sitio principal, los clientes pueden llevar a cabo las siguientes acciones para acceder rápidamente a sus datos en el sitio secundario.

## Migración tras error con un duplicado de un volumen de réplica en el sitio secundario

1. Inicie una sesión en [la consola de IBM Cloud](https://{DomainName}/catalog/){:new_window} y pulse el icono de **Menú** en la parte superior izquierda. Seleccione **Infraestructura clásica**.

   Como alternativa, puede iniciar una sesión en el [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
2. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
3. Pulse la réplica de la LUN en la lista para ver su página **Detalles**.
4. En la página **Detalles**, desplácese hacia abajo y seleccione una instantánea existente; luego pulse **Acciones** > **Duplicar**.
5. Realice las actualizaciones necesarias en la capacidad (para aumentar el tamaño) o en IOPS para el nuevo volumen.
6. Actualice el espacio de instantánea para el nuevo volumen si es necesario.
7. Pulse **Continuar** para realizar el pedido del duplicado.

En cuanto se cree el volumen, se puede adjuntar a un host y realizar operaciones de lectura y escritura. Mientras los datos se estén copiando desde el volumen original en el duplicado, la página de detalles muestra que la duplicación está en curso. Cuando se complete el proceso de duplicación, el nuevo volumen se independiza completamente del original y se puede gestionar con instantáneas y réplica, como es habitual.

## Restablecimiento en el sitio primario original

Si desea devolver la producción al sitio primario original, debe seguir los pasos siguientes.

1. Inicie una sesión en [la consola de IBM Cloud](https://{DomainName}/catalog/){:new_window} y pulse el icono de **Menú** en la parte superior izquierda. Seleccione **Infraestructura clásica**.

   Como alternativa, puede iniciar una sesión en el [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
2. Pulse **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
3. Pulse el nombre de la LUN y cree una planificación de instantánea (si no hay ya una).

   Para obtener más información sobre planificaciones de instantánea, consulte [Gestión de instantáneas](working-with-snapshots.html#adding-a-snapshot-schedule).
   {:tip}
4. Pulse **Réplica** y pulse **Adquirir una réplica**.
5. Seleccione la planificación de instantáneas existente que quiera que siga la réplica. La lista contiene todas las planificaciones de instantáneas activas.
6. Pulse **Ubicación** y seleccione el centro de datos que era el sitio de producción original.
7. Pulse **Continuar**.
8. Marque el recuadro de selección **He leído el Acuerdo de Servicio Maestro…** y pulse **Realizar pedido**.

Una vez finalizada la réplica, tiene que crear un volumen duplicado de la nueva réplica.
{:important}

1. Vuelva a **Almacenamiento** > **{{site.data.keyword.blockstorageshort}}**.
2. Pulse la réplica de la LUN en la lista para ver su página **Detalles**.
3. En la página **Detalles**, desplácese hacia abajo y seleccione una instantánea existente; luego pulse **Acciones** > **Duplicar**.
4. Realice las actualizaciones necesarias en la capacidad (para aumentar el tamaño) o en IOPS para el nuevo volumen.
5. Actualice el espacio de instantánea para el nuevo volumen si es necesario.
6. Pulse **Continuar** para realizar el orden de los duplicados.

Cuando finalice el proceso de duplicación, puede cancelar la réplica y los volúmenes utilizados para devolver los datos al sitio primario original. El duplicado se convierte en el almacenamiento primario y se puede volver a establecer la réplica en el sitio secundario original.