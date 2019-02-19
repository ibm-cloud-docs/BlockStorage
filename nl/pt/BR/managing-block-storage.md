---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gerenciando {{site.data.keyword.blockstorageshort}}
{: #managingstorage}

É possível gerenciar seus {{site.data.keyword.blockstoragefull}} volumes por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.

## Visualizando detalhes do  {{site.data.keyword.blockstorageshort}}  LUN

É possível visualizar um resumo das informações chave para o LUN de armazenamento selecionado, incluindo recursos extras de captura instantânea e de replicação que foram incluídos no armazenamento.

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique no nome do LUN apropriado na lista.

Como alternativa, é possível usar o comando a seguir na CLI do SL.
```
# slcli block volume-detail --help
Usage: slcli block volume-detail [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```

## Autorizando hosts para acessar o  {{site.data.keyword.blockstorageshort}}

Hosts "autorizados" são aqueles que receberam acesso a um LUN específico. Sem a autorização do host, não é possível acessar nem usar o armazenamento de seu sistema. A autorização de um host para acesso ao seu LUN gera o nome do usuário, a senha e o nome qualificado de iSCSI (IQN), que são necessários para montar a conexão iSCSI de multipath I/O (MPIO).

É possível autorizar e conectar os hosts localizados no mesmo data center que o seu armazenamento. É possível ter
múltiplas contas, mas não é possível autorizar um host de uma conta a acessar o armazenamento em outra conta.
{:important}

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role para a seção **Hosts autorizados** da página.
3. À direita, clique em **Autorizar host**. Selecione os hosts
que podem acessar esse LUN específico.

Como alternativa, é possível usar o comando a seguir na CLI do SL.
```
# slcli block access-authorize --help Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

## Visualizando a lista de hosts autorizados a acessar um LUN do {{site.data.keyword.blockstorageshort}}

1. Clique em **Armazenamento** -> **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.
2. Role para baixo até a seção **Hosts autorizados**.

Lá é possível ver a lista dos hosts que estão atualmente autorizados a acessar o LUN. Também é possível ver as informações sobre autenticação que são necessárias para fazer uma conexão - nome de usuário, senha e Host do IQN. O endereço de Destino é listado na página **Detalhe de armazenamento**. Para NFS, o endereço de destino é descrito como um nome DNS e, para iSCSI, é o endereço IP do portal de destino de descoberta.

Como alternativa, é possível usar o comando a seguir na CLI do SL.
```
# slcli block access-list --help
Usage: slcli block access-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, type,
                  private_ip_address, source_subnet, host_iqn, username,
                  password, allowed_host_id
  -h, --help      Show this message and exit.
```

## Visualizando o {{site.data.keyword.blockstorageshort}} ao qual um host está autorizado

É possível visualizar os LUNs aos quais um host tem acesso, incluindo as informações que são necessárias para fazer uma conexão - Nome do LUN, Tipo de armazenamento, Endereço de destino, capacidade e local:

1. Clique em **Dispositivos** -> **Lista de dispositivos** no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://control.softlayer.com/){:new_window} e clique no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.

É apresentada uma lista de LUNs de armazenamento ao quais esse host específico tem acesso. A lista é agrupada por tipo de armazenamento (bloco, arquivo, outro). É possível autorizar mais armazenamento ou remover o acesso clicando em **Ações**.

## Montando e desmontando o {{site.data.keyword.blockstorageshort}}

Com base no sistema operacional do host, siga as instruções apropriadas.

- [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurando o Block Storage para backup com cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurando o Block Storage para backup com Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)


## Revogando o acesso de um host ao  {{site.data.keyword.blockstorageshort}}

Se desejar parar o acesso de um host a um LUN de armazenamento específico, será possível revogá-lo. Ao revogar o acesso, a conexão de host é eliminada do LUN. O sistema operacional e os aplicativos nesse host não podem mais se comunicar com o LUN.

Para evitar problemas do lado do host, desmonte o LUN de armazenamento do sistema operacional antes de revogar
o acesso para evitar unidades ausentes ou distorção de dados.
{:important}

É possível revogar o acesso da **Lista de dispositivos** ou **Visualização de armazenamento**.

### Revogando o acesso a partir da Lista de Dis

1. Clique em **Dispositivos**, **Lista de dispositivos** por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window} e dê um clique duplo no dispositivo apropriado.
2. Selecione a guia **Armazenamento**.
3. É apresentada uma lista de LUNs de armazenamento ao quais esse host específico tem acesso. A lista é agrupada por tipo de armazenamento (bloco, arquivo, outro). Próximo ao nome do LUN, selecione **Ação"" e clique em **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

Se você desejar desconectar múltiplos LUNs de um host específico, será necessário repetir a ação Revogar
acesso para cada LUN.
{:tip}


### Revogando o acesso por meio da Visualização de armazenamento

1. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** e selecione o LUN do qual você deseja
revogar o acesso.
2. Role para **Hosts autorizados**.
3. Clique em **Ações** ao lado do host cujo acesso deve ser revogado e selecione **Revogar acesso**.
4. Confirme se você deseja revogar o acesso para um LUN porque a ação não pode ser desfeita. Clique em **Sim** para revogar acesso ao LUN ou **Não** para cancelar a ação.

Se você desejar desconectar múltiplos hosts de um LUN específico, será necessário repetir a ação Revogar acesso
para cada host.
{:tip}

### Revogando o acesso por meio da CLI do SL.

Como alternativa, é possível usar o comando a seguir na CLI do SL.
```
# slcli block access-revoke --help
Usage: slcli block access-revoke [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to revoke
                            authorization
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to revoke
                            authorization
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to revoke authorization
  --ip-address TEXT         An IP address to revoke authorization
  --help                    Show this message and exit.
```

## Cancelando um LUN de armazenamento

Se você não precisar mais de um LUN específico, será possível cancelá-lo a qualquer momento.

Para cancelar um LUN de armazenamento, é necessário revogar o acesso de quaisquer hosts primeiro.
{:important}

1. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Ações** para o LUN a ser cancelado e selecione **Cancelar o {{site.data.keyword.blockstorageshort}}**.
3. Confirme se deseja cancelar o LUN imediatamente ou na data de aniversário de quando o LUN foi provisionado.

   Se você selecionar a opção para cancelar o LUN em sua data de aniversário, será possível anular a solicitação
de cancelamento antes da data de aniversário.
   {:tip}
4. Clique em **Continuar** ou em **Fechar**.
5. Clique na caixa de seleção **Confirmação** e clique em **Confirmar**.

Como alternativa, é possível usar o comando a seguir na CLI do SL.
```
# slcli block volume-cancel --help
Usage: slcli block volume-cancel [OPTIONS] VOLUME_ID

Options:
  --reason TEXT  An optional reason for cancellation
  --immediate    Cancels the block storage volume immediately instead of on
                 the billing anniversary
  -h, --help     Show this message and exit.
```
