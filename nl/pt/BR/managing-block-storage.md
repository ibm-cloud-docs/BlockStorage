---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

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

É possível visualizar um resumo das informações chave para o LUN de armazenamento selecionado incluindo
recursos de captura instantânea e de replicação adicionais que foram incluídos no armazenamento.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique no nome do LUN apropriado na lista.

## Autorize hosts para acessar o {{site.data.keyword.blockstorageshort}}

Hosts “autorizados” são hosts que receberam direitos de acesso a um LUN específico. Sem autorização do host, não é possível acessar ou usar o armazenamento de seu sistema. A autorização de um
host para acesso ao seu LUN gera o nome de usuário, a senha e o nome qualificado de iSCSI (IQN), que são
necessários para montar a conexão iSCSI de E/S de caminhos múltiplos (MPIO).

**Nota**: é possível autorizar e conectar somente hosts que residem no mesmo data
center que o seu armazenamento. Se você tiver múltiplas contas, não será possível autorizar um host de uma conta
para acessar seu armazenamento em outra conta.

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role até a seção Hosts autorizados da página.
3. Clique no link **Autorizar host** no lado direito da página. Selecione os hosts
que podem acessar esse LUN específico.

 

## Visualize a lista de hosts autorizados para um LUN do {{site.data.keyword.blockstorageshort}}

Use as etapas a seguir para visualizar a lista de hosts autorizados para um LUN.

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role para baixo até a parte inferior da página para a seção Hosts autorizados.

Aqui você verá a lista de hosts que estão atualmente autorizados a acessar o LUN e, especificamente para
iSCSI, as informações de autenticação necessárias para fazer uma conexão - nome de conexão, senha e IQN do
host. O endereço de destino está na parte superior da página Detalhes do armazenamento. Para NFS, ele é descrito
como um nome DNS e, para iSCSI, como o endereço IP do portal de destino de descoberta.

 

## Visualize o {{site.data.keyword.blockstorageshort}} ao qual um host é autorizado

É possível visualizar os LUNs para os quais um host tem acesso, incluindo as informações necessárias para
fazer uma conexão - nome do LUN, tipo de armazenamento, endereço de destino, capacidade e local:

1. Clique em **Dispositivos** -> **Lista de dispositivos** no
[{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} e
clique no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.

Será apresentada uma lista a você de LUNs de armazenamento ao quais esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outros). Nos respectivos menus Ações, é possível autorizar armazenamento adicional ou remover o acesso.

 

## Montagem e desmontagem do {{site.data.keyword.blockstorageshort}}

Consulte os seguintes artigos com detalhes para montagem e desmontagem
do {{site.data.keyword.blockstorageshort}} de um host.

- [{{site.data.keyword.blockstorageshort}} no
Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} no
Microsoft Windows](accessing-block-storage-windows.html)

 

## Revogar o acesso de um host ao {{site.data.keyword.blockstorageshort}}

Se você deseja interromper o acesso de um host para um LUN de armazenamento específico, é possível
revogar o acesso. Após revogar o acesso, a conexão de host será eliminada do LUN e nem o sistema operacional
nem os aplicativos poderão se comunicar com o LUN.

**Nota**: para evitar problemas no lado do host, desmonte o LUN de armazenamento de seu
sistema operacional antes de revogar o acesso para evitar perda de unidades ou distorção de dados.

É possível revogar o acesso do Armazenamento na Lista de dispositivos ou
nas visualizações de Armazenamento.

### Revogar acesso na Lista de dispositivos:

1. Clique em **Dispositivos**, **Lista de dispositivos** no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e
dê um clique duplo no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.
3. Será apresentada uma lista a você de LUNs de armazenamento ao quais esse host específico tem acesso, todos agrupados por tipo de armazenamento (bloco, arquivo, outros). Selecione o menu Ação respectivo ao lado do LUN do qual você deseja revogar o acesso e clique em
**Revogar acesso**.
4. Será perguntado a você se deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

**Nota**: se você deseja desconectar múltiplos LUNs em um host específico, é
necessário repetir a ação Revogar acesso para cada LUN.


### Revogar acesso na visualização Armazenamento:

1. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** e selecione o LUN do qual você deseja
revogar o acesso.
2. Role para baixo até a seção **Hosts autorizados** da página.
3. Clique na seta suspensa **Ações** ao lado do host cujo acesso deve ser revogado e
selecione **Revogar acesso**.
4. Será perguntado a você se deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

**Nota**: se você desejar desconectar múltiplos hosts de um LUN específico, será
necessário repetir a ação Revogar acesso para cada host.

 

## Cancelar um LUN de armazenamento

Se você não precisar mais de um LUN específico, ele poderá ser cancelado. Para cancelar um LUN de
armazenamento, é necessário primeiramente revogar o acesso de quaisquer hosts.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique na seta suspensa **Ações** para o LUN a ser cancelado e
selecione **Cancelar o {{site.data.keyword.blockstorageshort}}**.
3. Será solicitado a confirmar se deseja cancelar o LUN imediatamente ou na data de aniversário de
quando o LUN foi provisionado. Clique em **Continuar** ou em **Fechar**.
**Nota**: se você selecionar a opção para cancelar o LUN em sua data de aniversário,
será possível cancelar a solicitação de cancelamento antes de sua data de aniversário.
4. Clique na caixa de seleção **Confirmação** e clique em
**Confirmar**.

 

