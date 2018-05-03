---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows

Antes de iniciar, verifique se o host que está acessando o volume do {{site.data.keyword.blockstoragefull}} foi autorizado por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. Na página de listagem do {{site.data.keyword.blockstorageshort}}, clique no
botão **Ações** associado ao volume recém-provisionado e clique em
**Autorizar host**.
2. Selecione os hosts desejados na lista e clique em **Enviar**. Isso autoriza que eles acessem o volume.

## Como montar os volumes do {{site.data.keyword.blockstorageshort}}

A seguir estão as etapas necessárias para conectar uma instância
do {{site.data.keyword.BluSoftlayer_full}} Compute baseada em Windows a um número da unidade lógica
(LUN) Internet Small Computer System Interface (iSCSI) para E/S de caminhos múltiplos (MPIO). O exemplo é baseado no Windows Server 2012. As etapas podem ser ajustadas para outras versões do Windows de
acordo com a documentação do fornecedor do sistema operacional (S.O.).

### Configure o recurso MPIO

1. Ative o Server Manager e navegue para **Gerenciar**, **Incluir funções e
recursos**.
2. Clique em **Avançar** para o menu Recursos.
3. Role para baixo e clique na caixa de seleção **E/S de caminhos múltiplos**.
4. Clique em **Instalar** para instalar o MPIO no servidor host.
![Incluindo funções e recursos](/images/Roles_Features.png)

### Incluir suporte iSCSI para MPIO

1. Abra as Propriedades do MPIO. Para abrir as Propriedades do MPIO, clique em
**Iniciar**, aponte para Ferramentas administrativas e, em seguida, clique em
**MPIO**.
2. Clique na guia **Descobrir caminhos múltiplos**
3. Marque a caixa de seleção **Incluir suporte para dispositivos iSCSI**
e, em seguida, clique em **Incluir**. Quando solicitado a reiniciar o computador, clique em
**Sim**.

**Nota**: no caso do Windows Server 2008, a inclusão de suporte para iSCSI permite que
o Microsoft Device Specific Module (MSDSM) solicite todos os dispositivos iSCSI para MPIO, que primeiramente
requer uma conexão com um Destino iSCSI.

### Configurar o Inicializador iSCSI

1. Ative o Inicializador iSCSI do Server Manager e selecione **Ferramentas**,
**Inicializador iSCSI**.
2. Clique na guia **Configuração**.
    - O campo Nome do inicializador já pode ser preenchido com uma entrada semelhante a
iqn.1991-05.com.microsoft:.
    - Clique em **Mudar** para substituir os valores existentes pelo seu Nome
qualificado de iSCSI (IQN). O nome de IQN pode ser obtido na tela
Detalhes do {{site.data.keyword.blockstorageshort}} no portal.
![Propriedades do Inicializador iSCSI](/images/iSCSI.png)
    - Clique na guia **Descoberta** e clique em **Descobrir
portal**.
    - Insira o endereço IP de seu destino iSCSI e deixe a Porta no valor padrão de 3260. 
    - Clique em **Avançado** para ativar a janela Configurações avançadas.
    - Selecione **Ativar logon do CHAP** para ativar a autenticação do CHAP.
![Ativar login do CHAP](/images/Advanced_0.png)
    **Nota:** os campos secretos Nome e Destino fazem distinção entre maiúsculas e
minúsculas.
         - Exclua quaisquer entradas existentes no campo Nome e insira o nome de usuário no portal.
         - Insira a senha por meio do portal no campo Segredo de destino.<br/>
         **Nota:** os valores de Nome e Segredo de destino podem ser obtidos na tela Detalhes
do {{site.data.keyword.blockstorageshort}} no portal como Nome do usuário e Senha, respectivamente.
    - Clique em **OK** nas janelas Configurações avançadas e Descobrir portal de
destino para voltar à tela principal Propriedades do Inicializador iSCSI. Se você receber erros de
autenticação, verifique novamente as entradas de nome de usuário e senha.
![Destino inativo](/images/Inactive_0.png)
    Observe que o nome do seu destino aparece na seção Destinos descobertos com um status Inativo. 
    
    As etapas a seguir mostram como ativar o destino.
    
### Ativar destino

1. Clique em **Conectar** para conectar-se ao destino.
2. Selecione **Ativar caminhos múltiplos** para ativar E/S de
caminhos múltiplos para o destino. ![Ativar caminhos múltiplos](/images/Connect_0.png)
3. Clique em **Avançado** e selecione **Ativar logon do CHAP**.
![Ativar o CHAP](/images/chap_0.png)
4. Insira o nome de usuário no campo Nome e insira a senha no campo Segredo de destino.<br/>
**Nota:** os valores dos campos Nome e Segredo de destino podem ser obtidos na tela Detalhes
do {{site.data.keyword.blockstorageshort}} no portal como Nome do usuário e Senha, respectivamente.
5. Clique em **OK** até que a janela Propriedades do Inicializador iSCSI seja
exibida. O status do destino na seção Destinos descobertos muda de Inativo para Conectado.
![Status conectado](/images/Connected.png) 


### Configure o MPIO no Inicializador iSCSI

1. Ative o Inicializador iSCSI e clique em **Propriedades** na guia Destinos.
2. Clique em **Incluir sessão** na janela Propriedades para ativar a janela
Conectar-se ao destino.
3. Selecione **Ativar caminhos múltiplos** e clique em
**Avançado...**.
![Destino](/images/Target.png) 
  
4. Na janela Configurações avançadas
   - Deixe o Padrão como o valor para os campos Adaptador local e IP do inicializador. Para servidores
host com múltiplas interfaces no iSCSI, será necessário escolher o valor apropriado para o campo IP do
inicializador.
   - Selecione o IP de seu armazenamento iSCSI na lista suspensa IP do portal de destino.
   - Clique na caixa de seleção **Ativar logon do CHAP**
   - Insira os valores de Nome e Segredo de destino obtidos do portal e clique em
**OK**.
   - Clique em **OK** na janela Conectar-se ao destino para voltar para a janela
Propriedades. A janela Propriedades agora deve exibir mais de uma sessão dentro da janela Identificador. Agora
você tem mais de uma sessão para o armazenamento iSCSI.
![Configurações](/images/Settings.png) 
   
5. Na janela Propriedades, clique em **Dispositivos** e ative a janela Dispositivos. O nome da interface de dispositivo deve ter mpio no início do nome do dispositivo. <br/>
  ![Dispositivos](/images/Devices.png) 
  
6. Clique em **MPIO** para ativar a janela Detalhes do dispositivo. Essa janela
permite escolher políticas de balanceamento de carga para MPIO e também mostra os caminhos para o iSCSI. Neste
exemplo, dois caminhos são mostrados como disponíveis para o MPIO com uma política de balanceamento de carga
Round robin com subconjunto.
![DeviceDetails](/images/DeviceDetails.png) 
  
7. Clique em **OK** várias vezes para sair do Inicializador iSCSI.



## Como verificar se o MPIO está configurado corretamente em sistemas operacionais Windows

Para verificar se o MPIO está configurado no Windows, primeiramente assegure-se de que o Complemento do MPIO
esteja ativado e que a reinicialização obrigatória tenha sido concluída:

![Roles_Features_0](/images/Roles_Features_0.png)

Quando a reinicialização é concluída e o dispositivo de armazenamento de desempenho é incluído,
é possível verificar se o MPIO está configurado e funcionando. Para fazer isso, consulte **Detalhes do
dispositivo de destino** e clique em **MPIO**:
![DeviceDetails_0](/images/DeviceDetails_0.png)

Quando o MPIO não é configurado em seu dispositivo de armazenamento de desempenho e as
equipes do {{site.data.keyword.BluSoftlayer_full}} executam manutenção ou uma indisponibilidade de
rede ocorre, seu dispositivo de armazenamento é desconectado e se torna indisponível. O MPIO assegurará
um nível extra de conectividade durante esses eventos e manterá uma sessão estabelecida com leituras/gravações
ativas indo para o LUN.

## Como desmontar volumes do {{site.data.keyword.blockstorageshort}}

A seguir estão as etapas necessárias para desconectar uma instância de cálculo do Bluemix baseada no
Windows em um LUN iSCSI de MPIO. O exemplo é baseado no Windows Server 2012. As etapas podem ser ajustadas
para outras versões do Windows de acordo com a documentação do fornecedor do S.O.

### Ative o Inicializador iSCSI.

1. Clique na guia **Destinos**.
2. Selecione os destinos que você deseja remover e clique em **Desconectar**.

### Opcional se você não precisa mais acessar os destinos iSCSI:

1. Clique na guia **Descoberta** no Inicializador iSCSI.
2. Destaque o portal de destino associado ao volume de armazenamento e clique em
**Remover**.
