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


# Windows 2012 R2 - múltiplos dispositivos iSCSI
{: #troubleshootingWin12}

Se você usar mais de dois dispositivos iSCSI, poderá considerar esse procedimento útil; especialmente se todas as quatro cotas iSCSI forem do mesmo dispositivo de armazenamento. Se você vir apenas dois dispositivos no Disk Manager, será necessário se conectar manualmente a cada dispositivo no inicializador iSCSI em cada nó do servidor.

1. Abra o inicializador iSCSI do Windows.
2. Clique na guia **Destinos** e, em seguida, clique em **Dispositivos**.

   ![Propriedades do inicializador iSCSI](/images/win12-ts1.png)
3. Confirme o número de dispositivos que são mostrados. Se você vir dois dispositivos, em vez dos quatro que foram autorizados, continue com a próxima etapa.
4. Clique em **Destinos** e, em seguida, em **Conectar**.
5. Selecione **Caminhos múltiplos** e, em seguida, **Avançado**.
6. Selecione Microsoft iSCSI Initiator como o adaptador Local. O IP do inicializador pertence ao seu servidor.
7. Selecione o primeiro dos endereços IP que são mostrados na lista IP do portal de destino.

   ![Configurações avançadas, endereços IP](/images/win12-ts3.png)

   É necessário repetir essa etapa para todos os endereços IP listados.
   {:tip}

8. Selecione a caixa **Ativar CHAP** e insira o ID do CHAP e a senha do servidor.

   ![Configurações avançadas, CHAP](/images/win12-ts4.png)
9. Clique em **OK**.
10. Repita as etapas 5 a 9 para cada IP que você inseriu no Inicializador iSCSI. Ao terminar, clique na guia **Dispositivos** e revise os resultados. Espere ver cada LUN que você configurou listado duas vezes.

    ![Guia Dispositivos](/images/win12-ts5.png)
11. Clique em **OK**.
12. Abra o Disk Manager e verifique se as unidades estão sendo mostrados agora.

    ![Gerenciador de dispositivos](/images/win12-ts6.png)
