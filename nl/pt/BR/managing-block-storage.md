---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Gerenciando {{site.data.keyword.blockstorageshort}}

É possível gerenciar seus volumes do {{site.data.keyword.blockstoragefull}} por meio do
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}. Este artigo fornece instruções para as tarefas mais comuns.

## Consulte os detalhes de um LUN do {{site.data.keyword.blockstorageshort}} provisionado

É possível visualizar um resumo das informações chave para o LUN de armazenamento selecionado incluindo recursos de captura instantânea e de replicação extras que foram incluídos no armazenamento.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique no nome do LUN apropriado na lista.

## Autorize hosts para acessar o {{site.data.keyword.blockstorageshort}}

Hosts "autorizados" são hosts que receberam direitos de acesso a um LUN específico. Sem autorização do host, você não será capaz de acessar ou usar o armazenamento de seu sistema. A autorização de um host para acesso ao seu LUN gera o nome do usuário, a senha e o nome qualificado de iSCSI (IQN), que são necessários para montar a conexão iSCSI de multipath I/O (MPIO).

**Nota**: é possível autorizar e conectar somente hosts que residem no mesmo data center que o seu armazenamento. Se você tiver múltiplas contas, não será possível autorizar um host de uma conta para acessar seu armazenamento em outra conta.

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role para a seção **Hosts autorizados** da página.
3. À direita, clique em **Autorizar host**. Selecione os hosts
que podem acessar esse LUN específico.

 

## Visualize a lista de hosts autorizados para um LUN do {{site.data.keyword.blockstorageshort}}

Use as etapas a seguir para visualizar a lista de hosts autorizados para um LUN.

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role até a parte inferior da página para a seção **Hosts autorizados**.

Aqui é possível ver a lista de hosts que estão atualmente autorizados a acessar o LUN e, especificamente para iSCSI, as informações sobre autenticação necessárias para fazer uma conexão - nome do usuário, senha e host do IQN. O endereço de destino está na parte superior da página **Detalhe do armazenamento**. Para NFS, o endereço de destino é descrito como um nome DNS e, para iSCSI, é o endereço IP do portal de destino de descoberta.

 

## Visualize o {{site.data.keyword.blockstorageshort}} ao qual um host é autorizado

É possível visualizar os LUNs para os quais um host tem acesso, incluindo as informações necessárias para
fazer uma conexão - nome do LUN, tipo de armazenamento, endereço de destino, capacidade e local:

1. Clique em **Dispositivos** -> **Lista de dispositivos** no [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} e clique no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.

É apresentada uma lista de LUNs de armazenamento ao quais esse host específico tem acesso. A lista é agrupada por tipo de armazenamento (bloco, arquivo, outro). É possível autorizar mais armazenamento ou remover o acesso clicando em **Ações**.

 

## Montar e desmontar o {{site.data.keyword.blockstorageshort}}

Consulte os artigos a seguir com detalhes:

- [{{site.data.keyword.blockstorageshort}} no
Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} no
Microsoft Windows](accessing-block-storage-windows.html)

 

## Revogar o acesso de um host ao {{site.data.keyword.blockstorageshort}}

Se você deseja interromper o acesso de um host para um LUN de armazenamento específico, é possível
revogar o acesso. Após revogar o acesso, a conexão de host será eliminada do LUN. O sistema operacional e os aplicativos não serão mais capazes de se comunicarem com o LUN.

**Nota**: para evitar problemas no lado do host, desmonte o LUN de armazenamento de seu
sistema operacional antes de revogar o acesso para evitar perda de unidades ou distorção de dados.

É possível revogar o acesso da **Lista de dispositivos** ou **Visualização de armazenamento**.

### Revogar acesso na Lista de dispositivos:

1. Clique em **Dispositivos**, **Lista de dispositivos** no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e
dê um clique duplo no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.
3. É apresentada uma lista de LUNs de armazenamento ao quais esse host específico tem acesso. A lista é agrupada por tipo de armazenamento (bloco, arquivo, outro). Próximo ao nome do LUN, selecione **Ação"" e clique em **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

**Nota**: se você deseja desconectar múltiplos LUNs de um host específico, é necessário repetir a ação Revogar acesso para cada LUN.


### Revogar acesso na visualização Armazenamento:

1. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** e selecione o LUN do qual você deseja
revogar o acesso.
2. Role para **Hosts autorizados**.
3. Clique em **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

**Nota**: se você deseja desconectar múltiplos hosts de um LUN específico, é necessário repetir a ação Revogar acesso para cada host.

 

## Cancelar um LUN de armazenamento

Se você não precisar mais de um LUN específico, ele poderá ser cancelado. Para cancelar um LUN de
armazenamento, é necessário primeiramente revogar o acesso de quaisquer hosts.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Ações** para o LUN a ser cancelado e selecione **Cancelar o {{site.data.keyword.blockstorageshort}}**.
3. Confirme se deseja cancelar o LUN imediatamente ou na data de aniversário de quando o LUN foi provisionado. 
**Nota**: se você selecionar a opção para cancelar o LUN em sua data de aniversário,
será possível cancelar a solicitação de cancelamento antes de sua data de aniversário.
4. Clique em **Continuar** ou em **Fechar**. 
5. Clique na caixa de seleção **Confirmação** e clique em **Confirmar**.

 

