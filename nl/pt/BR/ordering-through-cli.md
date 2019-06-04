---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Pedindo o {{site.data.keyword.blockstorageshort}} por meio da SLCLI
{: #orderingthroughCLI}

É possível usar a SLCLI para fazer pedidos de produtos que normalmente são pedidos por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}. Na API do SL, um pedido pode consistir em múltiplos contêineres de pedido. A CLI do pedido funciona com apenas um contêiner de pedido.

Para obter mais informações sobre como instalar e usar a SLCLI, consulte [Cliente da API do Python](https://softlayer-python.readthedocs.io/en/latest/cli/){: external}.
{:tip}

## Procurando ofertas disponíveis do {{site.data.keyword.blockstorageshort}}

O primeiro componente a ser procurado quando você faz um pedido é um pacote. Os pacotes são divididos entre os diferentes produtos de nível superior que estão disponíveis para pedido no {{site.data.keyword.cloud}}. Alguns pacotes de exemplo são CLOUD_SERVER para VSIs, BARE_METAL_SERVER para servidores bare metal e STORAGE_AS_A_SERVICE_STAAS para o {{site.data.keyword.blockstorageshort}} e o {{site.data.keyword.filestorage_short}}.

Dentro de um pacote, alguns itens são subdivididos em categorias. Alguns pacotes têm pré-configurações para a sua conveniência e outros requerem que os itens sejam especificados individualmente. Se a categoria de um pacote for necessária, um item dessa categoria deverá ser escolhido para pedir o pacote. Dependendo da categoria, alguns itens dentro dela podem ser mutuamente exclusivos.

Cada pedido deve ter uma localização associada (data center). Ao pedir o {{site.data.keyword.blockstorageshort}}, certifique-se de que ele seja fornecido na mesma localização que as suas instâncias de cálculo.
{:important}

É possível usar o comando `slcli order package-list` para localizar o pacote
que você deseja pedir. Uma opção `-keyword` é fornecida para executar procura e filtragem simples. Essa opção facilita localizar o pacote necessário. Procure **Storage-as-a-Service Package 759**.

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

Options:
  --keyword TEXT  A word (or string) used to filter package names.
  -h, --help      Show this message and exit.
```

Também é possível usar o comando `slcli block volume-order`.

```
# slcli block volume-order --help
Usage: slcli block volume-order [OPTIONS]

 Order a block storage volume.

Options:
 --storage-type [performance|endurance]
                                 Type of block storage volume  [required]
 --size INTEGER                  Size of block storage volume in GB.
                                 Permitted Sizes:
                                 20, 40, 80, 100, 250, 500,
                                 1000, 2000, 4000, 8000, 12000  [required]
 --iops INTEGER                  Performance Storage IOPs, between 100 and
                                 6000 in multiples of 100  [required for
                                 storage-type performance]
 --tier [0.25|2|4|10]            Endurance Storage Tier (IOP per GB)
                                 [required for storage-type endurance]
 --os-type [HYPER_V|LINUX|VMWARE|WINDOWS_2008|WINDOWS_GPT|WINDOWS|XEN]
                                 Operating System  [required]
 --location TEXT                 Datacenter short name (e.g.: dal09)
                                 [required]
 --snapshot-size INTEGER         Optional parameter for ordering snapshot
                                 space along with endurance block storage;
                                 specifies the size (in GB) of snapshot space
                                 to order
 --service-offering [storage_as_a_service|enterprise|performance]
                                 The service offering package to use for
                                 placing the order [optional, default is
                                 'storage_as_a_service']
 --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                 to monthly)
 -h, --help                      Show this message and exit.
```

Para obter mais informações sobre o pedido do {{site.data.keyword.blockstorageshort}} por meio da API, consulte [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Para poder acessar todos os novos recursos, peça o `Storage-as-a-Service Package 759`.
{:tip}


## Fazendo o pedido

O exemplo a seguir mostra como solicitar um volume do {{site.data.keyword.blockstorageshort}} de 80 GB com espaço de captura instantânea de 20 GB e 0,25 IOPS por GB.

```
slcli block volume-order --storage-type endurance --size 80 --tier 0.25 --os-type LINUX --location dal09 --snapshot-size 20
Order #15547457 placed successfully!
 > Endurance Storage
 > Block Storage
 > 0.25 IOPS per GB
 > 80 GB Storage Space
 > 20 GB Storage Space (Snapshot Space)
```

Por padrão, é possível provisionar um total combinado de 250 volumes {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}. Para aumentar o número de seus volumes, entre em contato com o representante de vendas. Para obter mais informações sobre o aumento dos limites, consulte [Gerenciando os limites de armazenamento](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).
{:important}

## Autorizando o acesso dos hosts ao novo armazenamento

```
slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

  Authorizes hosts to access a given volume

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```

Para obter mais informações sobre como autorizar o acesso dos hosts ao {{site.data.keyword.blockstorageshort}} por meio da API, consulte [authorize_host_to_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.authorize_host_to_volume){: external}
{:tip}

Para o limite de autorizações simultâneas, veja as [FAQs](/docs/infrastructure/BlockStorage?topic=BlockStorage-faqs).
{:important}

## Conectando seu novo armazenamento
{: #mountingCLI}

Dependendo do sistema operacional do seu host, siga o link apropriado.
- [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurando o Block Storage para backup com cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurando o Block Storage para backup com Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)
