---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Pedindo o {{site.data.keyword.blockstorageshort}} por meio da CLI do SL

É possível usar a CLI do SL para fazer pedidos para produtos que normalmente são pedidos por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}. Na API do SL, um pedido pode consistir em múltiplos contêineres de pedido. A CLI do pedido funciona com apenas um contêiner de pedido.

Para obter mais informações sobre como instalar e usar a CLI do SL, consulte [Cliente da API da Python ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/cli.html){:new_window}.
{:tip}

## Procurando ofertas disponíveis do {{site.data.keyword.blockstorageshort}}

O primeiro componente a ser procurado quando você faz um pedido é um pacote. Os pacotes são divididos entre os diferentes produtos de nível superior que estão disponíveis para pedido no {{site.data.keyword.BluSoftlayer_full}}. Alguns pacotes de exemplo são CLOUD_SERVER para VSIs, BARE_METAL_SERVER para servidores bare metal e STORAGE_AS_A_SERVICE_STAAS para o {{site.data.keyword.blockstorageshort}} e o {{site.data.keyword.filestorage_short}}.

Dentro de um pacote, alguns itens são subdivididos em categorias. Alguns pacotes têm pré-configurações para a sua conveniência e outros requerem que os itens sejam especificados individualmente. Se a categoria de um pacote for necessária, um item dessa categoria deverá ser escolhido para pedir o pacote. Dependendo da categoria, alguns itens dentro dela podem ser mutuamente exclusivos.

Cada pedido deve ter uma localização associada (data center). Ao pedir o {{site.data.keyword.blockstorageshort}}, certifique-se de que ele seja fornecido na mesma localização que as suas instâncias de cálculo.
{:important}

É possível usar o comando `slcli order package-list` para localizar o pacote que você deseja pedir. Uma opção `-keyword` é fornecida para executar procura e filtragem simples. Essa opção facilita localizar o pacote necessário.

```
$ slcli order package-list --help
Uso: slcli order package-list [OPTIONS]

  Liste os pacotes que podem ser pedidos por meio da API placeOrder.

  Exemplo:
      # Listar todos os pacotes para pedido
      slcli order package-list

  As palavras-chave também podem ser usadas para alguma funcionalidade de filtragem simples para ajudar a localizar um pacote mais facilmente.

  Exemplo:
     # Listar todos os pacotes com "servidor" no nome
      slcli order package-list --keyword server

Opções:
  --keyword TEXT  Uma palavra (ou sequência) usada para filtrar nomes de pacote.
  -h, --help      Mostrar essa mensagem e sair.
```

*Precisa de instruções para como localizar o Storage-as-a-Service Package 759*

```
$ slcli order package-list --keyword "Storage"
:.....................:.....................:
:         Nome        :       Nome da chave :
:.....................:.....................:
: ???                 : ???                 :
: ???                 : ???                 :
:.....................:.....................:
```

```
$ slcli order category-list STORAGE_AS_A_SERVICE_STAAS --required
:..................................:...................:............:
:               Nome               :    Código da categoria   : É requerido :
:..................................:...................:............:
:              Exemplo             :        ???        :     Y      :
:              Exemplo             :        ???        :     Y      :
:              Exemplo             :        ???        :     Y      :
:              Exemplo             :        ???        :     Y      :
:..................................:...................:............:
```

Selecione o restante de seus itens para o pedido usando o comando `item-list`. Normalmente, os pacotes têm vários itens para escolher, portanto, use a opção `–category` para recuperar os itens apenas da categoria na qual está interessado.

```
$ slcli order item-list STORAGE_AS_A_SERVICE_STAAS --category ??
:..........................:..............................................:
:         Nome da chave          :                Descrição                   :
:..........................:..............................................:
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:           ???            :                    ????                      :
:..........................:..............................................:
```

Para obter mais informações sobre como pedir o {{site.data.keyword.blockstorageshort}} por meio da API, consulte [order_block_volume ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.order_block_volume){:new_window}.
Para poder acessar todos os novos recursos, peça o `Storage-as-a-Service Package 759`.
{:tip}

## Verificando o pedido

Se você não tiver certeza das categorias necessárias que podem estar ausentes de seu pedido, será possível usar o comando `place` com o sinalizador `-verify`. Se alguma categoria estiver ausente, ela será impressa na tela.


```
$ slcli order place --verify blablabla
:..............................................:.................................................:......:
:                Nome da chave                       :                   Descrição               : Custo :
:..............................................:.................................................:......:
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:                  ???                         :                 yadi yadi yada                  :  0   :
:..............................................:.................................................:......:
```

A saída mostra cada item que está sendo pedido, juntamente com o custo associado a esse item. Se o pedido passar na verificação, isso significará que não há itens em conflito e todas as categorias necessárias têm um item que está especificado no pedido.

## Fazendo o pedido

A próxima etapa é fazer o pedido

```
$ slcli order place .....

Essa ação incorrerá em encargos na sua conta. Continuar ? [s/n]: s

Resposta da API
```

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o representante de vendas. Para obter mais informações sobre o aumento dos limites, consulte [Gerenciando os limites de armazenamento](managing-storage-limits.html).
{:important}

## Autorizando o acesso dos hosts ao novo armazenamento

TBD

Para obter mais informações sobre como autorizar o acesso dos hosts ao {{site.data.keyword.blockstorageshort}} por meio da API, consulte [authorize_host_to_volume![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://softlayer-python.readthedocs.io/en/latest/api/managers/block.html#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){:new_window}
{:tip}

Para o limite de autorizações simultâneas, veja as [FAQs](faqs.html).
{:important}

## Conectando seu novo armazenamento

Dependendo do sistema operacional do seu host, siga o link apropriado.
- [Conectando-se a LUNs iSCSI de MPIO no Linux](accessing_block_storage_linux.html)
- [Conectando-se a LUNs do iSCSI de MPIO no CloudLinux](configure-iscsi-cloudlinux.html)
- [Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows](accessing-block-storage-windows.html)
- [Configurando o Block Storage para backup com cPanel](configure-backup-cpanel.html)
- [Configurando o Block Storage para backup com Plesk](configure-backup-plesk.html)
