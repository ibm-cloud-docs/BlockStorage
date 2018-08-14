---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}

# Gerenciando {{site.data.keyword.blockstorageshort}}

É possível gerenciar os volumes do  {{site.data.keyword.blockstoragefull}}  por meio do  [ {{site.data.keyword.slportal}} ](https://control.softlayer.com/){:new_window}.

## Visualizando detalhes do  {{site.data.keyword.blockstorageshort}}  LUN

É possível visualizar um resumo das informações chave para o LUN de armazenamento selecionado, incluindo recursos extras de captura instantânea e de replicação que foram incluídos no armazenamento.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique no nome do LUN apropriado na lista.

## Autorizando hosts para acessar o  {{site.data.keyword.blockstorageshort}}

Hosts "autorizados" são aqueles que receberam acesso a um LUN específico. Sem a autorização do host, não é possível acessar nem usar o armazenamento de seu sistema. A autorização de um host para acesso ao seu LUN gera o nome do usuário, a senha e o nome qualificado de iSCSI (IQN), que são necessários para montar a conexão iSCSI de multipath I/O (MPIO).

**Nota**: é possível autorizar e conectar hosts que estão localizados no mesmo data center que seu armazenamento. Se você tiver múltiplas contas, não será possível autorizar um host de uma conta para acessar seu armazenamento em outra conta.

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role para a seção **Hosts autorizados** da página.
3. À direita, clique em **Autorizar host**. Selecione os hosts
que podem acessar esse LUN específico.

 

## Visualizando a lista de hosts autorizados a acessar um LUN do {{site.data.keyword.blockstorageshort}}

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role para baixo até a seção **Hosts autorizados**.

Lá é possível ver a lista dos hosts que estão atualmente autorizados a acessar o LUN. Também é possível ver as informações sobre autenticação que são necessárias para fazer uma conexão - nome de usuário, senha e Host do IQN. O endereço de Destino é listado na página **Detalhe de armazenamento**. Para NFS, o endereço de destino é descrito como um nome DNS e, para iSCSI, é o endereço IP do portal de destino de descoberta.

 

## Visualizando o {{site.data.keyword.blockstorageshort}} ao qual um host está autorizado

É possível visualizar os LUNs aos quais um host tem acesso, incluindo as informações que são necessárias para fazer uma conexão - Nome do LUN, Tipo de armazenamento, Endereço de destino, capacidade e local:

1. Clique em **Dispositivos** -> **Lista de dispositivos** no [{{site.data.keyword.slportal}}](http://control.softlayer.com/){:new_window} e clique no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.

É apresentada uma lista de LUNs de armazenamento ao quais esse host específico tem acesso. A lista é agrupada por tipo de armazenamento (bloco, arquivo, outro). É possível autorizar mais armazenamento ou remover o acesso clicando em **Ações**.

 

## Montando e desmontando  {{site.data.keyword.blockstorageshort}}

Consulte os artigos a seguir com detalhes:

- [{{site.data.keyword.blockstorageshort}} no
Linux](accessing_block_storage_linux.html)

- [{{site.data.keyword.blockstorageshort}} no
Microsoft Windows](accessing-block-storage-windows.html)

 

## Revogando o acesso de um host ao  {{site.data.keyword.blockstorageshort}}

Se desejar parar o acesso de um host a um LUN de armazenamento específico, será possível revogá-lo. Ao revogar o acesso, a conexão de host é eliminada do LUN. O sistema operacional e os aplicativos nesse host não podem mais se comunicar com o LUN.

**Nota**: para evitar problemas do lado do host, desmonte o LUN de armazenamento de seu sistema operacional antes de revogar o acesso para evitar unidades ausentes ou distorção de dados.

É possível revogar o acesso da **Lista de dispositivos** ou **Visualização de armazenamento**.

### Revogando o acesso a partir da Lista de Dis

1. Clique em **Dispositivos**, **Lista de dispositivos** no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} e
dê um clique duplo no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.
3. É apresentada uma lista de LUNs de armazenamento ao quais esse host específico tem acesso. A lista é agrupada por tipo de armazenamento (bloco, arquivo, outro). Próximo ao nome do LUN, selecione **Ação"" e clique em **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

**Nota**: se você deseja desconectar múltiplos LUNs de um host específico, é necessário repetir a ação Revogar acesso para cada LUN.


### Revogando o acesso por meio da Visualização de armazenamento

1. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** e selecione o LUN do qual você deseja
revogar o acesso.
2. Role para **Hosts autorizados**.
3. Clique em **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

**Nota**: se você deseja desconectar múltiplos hosts de um LUN específico, é necessário repetir a ação Revogar acesso para cada host.

 

## Cancelando um LUN de armazenamento

Se você não precisar mais de um LUN específico, ele poderá ser cancelado. Para cancelar um LUN de armazenamento, é necessário revogar o acesso de quaisquer hosts primeiro.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Ações** para o LUN a ser cancelado e selecione **Cancelar o {{site.data.keyword.blockstorageshort}}**.
3. Confirme se deseja cancelar o LUN imediatamente ou na data de aniversário de quando o LUN foi provisionado. 
**Nota**: se você selecionar a opção para cancelar o LUN em sua data de aniversário, será possível anular a solicitação de cancelamento antes dessa data.
4. Clique em **Continuar** ou em **Fechar**. 
5. Clique na caixa de seleção **Confirmação** e clique em **Confirmar**.
