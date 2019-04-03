---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block storage, auxiliary storage, missing routes, mpio, multipath, windows, troubleshooting

subcollection: BlockStorage

---

{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Windows 2012 R2 - varios dispositivos iSCSI
{: #troubleshootingWin12}

Si utiliza más de dos dispositivos iSCSI, puede que este procedimiento le resulte útil, especialmente si las cuatro asignaciones iSCSI son del mismo dispositivo de almacenamiento. Si ve sólo dos dispositivos en el gestor de discos, deberá conectarse manualmente a cada dispositivo en el iniciador iSCSI en cada nodo de servidor.

1. Abra el iniciador iSCSI de Windows.
2. Pulse el separador **Destinos** y, a continuación, pulse **Dispositivos**.

   ![propiedades del iniciador iSCSI](/images/win12-ts1.png)
3. Confirme el número de dispositivos que se muestran. Si ve dos dispositivos, en lugar de los cuatro que estaban autorizados, continúe en el paso siguiente.
4. Pulse **Destinos** y luego **Conectar**.
5. Seleccione **Multivía de acceso** y, a continuación, **Avanzado**.
6. Seleccione Iniciador iSCSI de Microsoft como adaptador local. La IP del iniciador pertenece al servidor.
7. Seleccione la primera de las direcciones IP que se muestran en la lista de IP del portal de destino.

   ![Configuración avanzada, direcciones IP](/images/win12-ts3.png)

   Tiene que repetir este paso para todas las direcciones IP que aparecen en la lista.
   {:tip}

8. Seleccione el recuadro **Habilitar CHAP** y escriba el ID y la contraseña de CHAP del servidor.

   ![Configuración avanzada, CHAP](/images/win12-ts4.png)
9. Pulse **Aceptar**.
10. Repita los pasos 5-9 para cada IP que haya especificado en el iniciador iSCSI. Cuando haya terminado, pulse el separador **Dispositivos** y revise los resultados. Debería ver todos los LUN que ha configurado enumerados dos veces.

    ![Separador Dispositivos](/images/win12-ts5.png)
11. Pulse **Aceptar**.
12. Abra el gestor de discos y verifique que aparezcan ahora las unidades.

    ![Gestor de dispositivos](/images/win12-ts6.png)
