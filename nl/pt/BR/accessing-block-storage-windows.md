---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows

Antes de iniciar, certifique-se de que o host que está acessando o volume do {{site.data.keyword.blockstoragefull}} tenha sido autorizado por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Na página de listagem do {{site.data.keyword.blockstorageshort}}, localize o novo volume e clique em **Ações**. Clique em **Autorizar host**.
2. Na lista, selecione o host ou os hosts que devem acessar o volume e clique em **Enviar**.

## Montando  {{site.data.keyword.blockstorageshort}}  Volumes

A seguir estão as etapas necessárias para conectar uma instância de Cálculo do {{site.data.keyword.BluSoftlayer_full}} baseada no Windows a um número de unidade lógica (LUN) de Small Computer System Interface (iSCSI) da internet de Multipath input/output (MPIO). O exemplo é baseado no Windows Server 2012. As etapas podem ser ajustadas para outras versões do Windows de acordo com a documentação do fornecedor do sistema operacional (S.O.).

### Configurando o recurso MPIO

1. Inicie o Gerenciador do servidor e navegue para **Gerenciar**, **Incluir funções e recursos**.
2. Clique em **Avançar** para o menu Recursos.
3. Role para baixo e marque **Multipath I/O**.
4. Clique em **Instalar** para instalar o MPIO no servidor host.
![Incluindo funções e recursos no Server Manager](/images/Roles_Features.png)

### Incluindo suporte iSCSI para MPIO

1. Abra a janela Propriedades de MPIO clicando em **Iniciar**, apontando para **Ferramentas administrativas** e clicando em **MPIO**.
2. Clique em **Descobrir caminhos múltiplos**.
3. Marque **Incluir suporte para dispositivos iSCSI** e, em seguida, clique em **Incluir**. Quando solicitado a reiniciar o computador, clique em
**Sim**.

**Nota**: no Windows Server 2008, a inclusão de suporte para o iSCSI permite que o Microsoft Device Specific Module (MSDSM) solicite MPIO para todos os dispositivos iSCSI, o que requer uma conexão com um Destino iSCSI primeiro.

### Configurando o Inicializador iSCSI

1. Inicie o Inicializador iSCSI por meio do Gerenciador do servidor e selecione **Ferramentas**, **Inicializador iSCSI**.
2. Clique na guia **Configuração**.
    - O campo Nome do inicializador poderá já estar preenchido com uma entrada semelhante a `iqn.1991-05.com.microsoft:`.
    - Clique em **Mudar** para substituir os valores existentes pelo seu Nome
qualificado de iSCSI (IQN). O nome de IQN pode ser obtido na tela Detalhes do {{site.data.keyword.blockstorageshort}} no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
    ![Propriedades do inicializador iSCSI](/images/iSCSI.png)
    - Clique na guia **Descoberta** e clique em **Descobrir
portal**.
    - Insira o endereço IP de seu destino iSCSI e deixe a Porta no valor padrão de 3260. 
    - Clique em **Avançado** para abrir a janela Configurações avançadas.
    - Selecione **Ativar logon do CHAP** para ativar a autenticação do CHAP.
![Ativar login do CHAP](/images/Advanced_0.png)
    **Nota:** os campos Nome e Segredo de destino fazem distinção entre maiúsculas e minúsculas.
         - No campo **Nome**, exclua quaisquer entradas existentes e insira o nome do usuário do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
         - No campo **Segredo de destino**, insira a senha do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
    - Clique em **OK** nas janelas **Configurações avançadas** e **Descobrir portal de destino** para voltar à tela principal Propriedades do inicializador iSCSI. Se você receber erros de autenticação, verifique as entradas de nome de usuário e senha.
    ![Destino inativo](/images/Inactive_0.png)
    **Nota**: o nome de seu destino aparece na seção Destinos descobertos com um status Inativo. 

    
### Ativando o destino

1. Clique em **Conectar** para conectar-se ao destino.
2. Marque a caixa de seleção **Ativar caminhos múltiplos** para ativar a E/S de caminhos múltiplos para o destino.
![Ativar caminhos múltiplos](/images/Connect_0.png)
3. Clique em **Avançado** e selecione **Ativar logon do CHAP**.
![Ativar o CHAP](/images/chap_0.png)
4. Insira o nome do usuário no campo Nome e insira a senha no campo Segredo de destino.<br/>
**Nota:** os valores dos campos Nome e Segredo de destino podem ser obtidos na tela Detalhes do {{site.data.keyword.blockstorageshort}}.
5. Clique em **OK** até que a janela **Propriedades do inicializador iSCSI** seja exibida. O status do destino na seção **Destinos descobertos** muda de **Inativo** para **Conectado**.
![Status Conectado](/images/Connected.png) 


### Configurando o MPIO no Inicializador iSCSI

1. Inicie o Inicializador iSCSI e, na guia Destinos, clique em **Propriedades**.
2. Clique em **Incluir sessão** na janela Propriedades para abrir a janela Conectar ao destino.
3. Marque a caixa de seleção **Ativar caminhos múltiplos** e clique em **Avançado...**.
![Destino](/images/Target.png) 
  
4. Na janela Configurações avançadas
   - Deixe o Padrão como o valor para os campos Adaptador local e IP do inicializador. Para servidores host com múltiplas interfaces no iSCSI, é necessário escolher o valor apropriado para o campo IP do Inicializador.
   - Selecione o IP de seu armazenamento iSCSI na lista suspensa **IP do portal de destino**.
   - Clique na caixa de seleção **Ativar logon do CHAP**
   - Insira os valores secretos de Nome e Destino obtidos no portal e clique em **OK**.
   - Clique em **OK** na janela Conectar-se ao destino para voltar para a janela
Propriedades. Agora a janela Propriedades exibe mais de uma sessão dentro da área de janela Identificador. Agora
você tem mais de uma sessão para o armazenamento iSCSI.
![Configurações](/images/Settings.png) 
   
5. Na janela Propriedades, clique em **Dispositivos** para abrir a janela Dispositivos. O nome da interface do dispositivo começa com `mpio`. <br/>
  ![Dispositivos](/images/Devices.png) 
  
6. Clique em **MPIO** para abrir a janela **Detalhes do dispositivo**. É possível escolher as políticas de balanceamento de carga para o MPIO nessa janela e ela mostrará os caminhos para o iSCSI. Neste exemplo, dois caminhos são mostrados como disponíveis para o MPIO com uma política de balanceamento de carga de Round Robin com Subconjunto.
  ![A janela Detalhes do dispositivo mostra dois caminhos disponíveis para o MPIO com uma política de balanceamento de carga de Round Robin com Subconjunto](/images/DeviceDetails.png) 
  
7. Clique em **OK** várias vezes para sair do Inicializador iSCSI.



## Verificando se o MPIO está configurado corretamente em sistemas operacionais Windows

Para verificar se o MPIO do Windows está configurado, deve-se primeiro assegurar que o Complemento MPIO esteja ativado e reiniciar o servidor.

![Roles_Features_0](/images/Roles_Features_0.png)

Quando a reinicialização estiver completa e o Dispositivo de armazenamento incluído, será possível verificar se o MPIO está configurado e funcionando. Para fazer isso, consulte **Detalhes do
dispositivo de destino** e clique em **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Se o MPIO não tiver sido configurado corretamente, seu dispositivo de armazenamento será desconectado e se tornará indisponível quando ocorrer uma indisponibilidade de rede ou quando as Equipes do {{site.data.keyword.BluSoftlayer_full}} executarem manutenção. O MPIO assegura um nível extra de conectividade durante esses eventos e mantém uma sessão estabelecida com leituras/gravações ativas indo para o LUN.

## Desmontando volumes  {{site.data.keyword.blockstorageshort}}

A seguir estão as etapas necessárias para desconectar uma instância de cálculo do {{site.data.keyword.Bluemix_short}} baseada no Windows de um LUN do iSCSI de MPIO. O exemplo é baseado no Windows Server 2012. As etapas podem ser ajustadas para outras versões do Windows de acordo com a documentação do fornecedor do S.O.

### Iniciando o Inicializador iSCSI

1. Clique na guia **Destinos**.
2. Selecione os destinos que você deseja remover e clique em **Desconectar**.

### Removendo destinos
Isso é opcional, para quando você não precisar mais acessar os destinos iSCSI.

1. Clique em **Descoberta** no Inicializador iSCSI.
2. Destaque o portal de destino que está associado a seu volume de armazenamento e clique em **Remover**.
