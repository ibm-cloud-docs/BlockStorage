---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}
{:ui-linked}

# Tutorial de Introdução
{: #getting-started}

O {{site.data.keyword.blockstoragefull}} é um armazenamento iSCSI persistente e de alto desempenho provisionado e gerenciado independentemente de instâncias de cálculo. Os LUNs do {{site.data.keyword.blockstorageshort}} baseados em iSCSI são conectados a dispositivos autorizados por meio de conexões Multi-path I/O (MPIO).

O {{site.data.keyword.blockstorageshort}} traz os melhores níveis de durabilidade e disponibilidade com um conjunto de recursos incomparável. Ele é construído usando padrões de mercado e melhores práticas. O {{site.data.keyword.blockstorageshort}} foi projetado para proteger a integridade dos dados e manter a disponibilidade por meio de eventos de manutenção e falhas não planejadas, além de fornecer uma linha de base de desempenho consistente.
{:shortdesc}

## Antes
de Começar
{: #prereqs}

Os LUNs do {{site.data.keyword.blockstorageshort}} podem ser provisionados de 20 GB a 12 TB com duas opções: <br/>
- Provisiona camadas do **Endurance** que apresentam níveis de desempenho predefinidos e outros recursos, como capturas instantâneas e replicação.
- Construa um ambiente de **Desempenho** poderoso com operações de
entrada/saída por segundo (IOPS) alocadas.

Para obter mais informações sobre a oferta do {{site.data.keyword.blockstorageshort}},
veja [Aprenda sobre o {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

## Considerações de Fornecimento

### Tamanho do bloco

O IOPS para Duração e Desempenho é baseado em um tamanho de bloco de 16 KB com uma carga de trabalho aleatória/sequencial 50/50 de leitura/gravação 50/50. Um bloco de 16 KB equivale a uma gravação no volume.
{:important}

O tamanho do bloco usado por seu aplicativo afetará diretamente o desempenho do armazenamento. Se o tamanho do bloco usado por seu aplicativo for menor que 16 KB, o limite do IOPS será realizado antes do limite do rendimento. Por outro lado, se o tamanho do bloco usado por seu aplicativo for maior que 16 KB, o limite de rendimento será realizado antes do limite do IOPS.

| Tamanho de bloco (KB) | IOPS | Rendimento (MB/s) |
|-----|-----|-----|
| 4 | 1.000 | 16 |
| 8 | 1.000 | 16 |
| 16 | 1.000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="A Tabela 1 mostra exemplos de como o tamanho de bloco e o IOPS afetam o rendimento.<br/>Tamanho médio de E/S x IOPS = rendimento em MB/s." caption-side="top"}

### Hosts autorizados

Outro fator a ser considerado é o número de hosts que estão usando seu volume. Se houver um único host acessando o volume, poderá ser difícil realizar o IOPS máximo disponível, especialmente em contagens extremas de IOPS (10.000s). Se a sua carga de trabalho requerer alto rendimento, será melhor configurar pelo menos alguns servidores para acessar seu volume para evitar um gargalo de servidor único.

### Conexão de rede

A velocidade da sua conexão de Ethernet deve ser mais rápida do
que o rendimento máximo esperado de seu volume. Em geral, não espere saturar sua conexão Ethernet além de 70% da largura de banda disponível. Por exemplo, se você tiver 6.000 IOPS e estiver usando um tamanho de bloco de 16 KB, o volume poderá manipular o rendimento de aproximadamente 94 MBps. Se você tiver uma conexão Ethernet de 1 Gbps com seu LUN, ela se tornará um gargalo quando seus servidores tentarem usar o rendimento máximo disponível. Isso porque 70% do limite teórico de uma conexão Ethernet de 1 Gbps (125 MB por segundo) permitiria 88 MB por segundo apenas.

Para obter o máximo de IOPS, recursos de rede adequados precisam estar em vigor. Outras considerações incluem o uso
de rede privada fora do armazenamento e os ajustes específicos do aplicativo e do lado do host (pilha
IP ou [profundidades da fila](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings) e
outras configurações).

O tráfego de armazenamento deve ser isolado de outros tipos de tráfego e não deve ser direcionado por meio de firewalls e roteadores. Para obter mais informações, consulte as [Perguntas mais frequentes](/docs/BlockStorage?topic=block-storage-faqs#isolatedstoragetraffic).

O tráfego de armazenamento é incluído no uso total de rede de Virtual Servers Públicos. Para obter mais informações sobre os limites que podem ser impostos pelo serviço, consulte a [Documentação do Virtual Server](/docs/vsi?topic=virtual-servers-about-public-virtual-servers#about-public-virtual-servers).
{:tip}

## Enviando sua Ordem
{: #submitorder}

Quando você estiver pronto para enviar seu pedido, será possível fazê-lo por meio do [Console](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole), da [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI) ou da [CLI do IBM Cloud](/docs/cli/reference/ibmcloud?topic=cloud-cli-sl-block-storage#sl_block_volume_order).

Para obter informações sobre como pedir o {{site.data.keyword.blockstorageshort}} por meio da API, consulte [order_block_volume](https://softlayer-python.readthedocs.io/en/latest/api/managers/block/#SoftLayer.managers.block.BlockStorageManager.order_block_volume){: external}.
Para poder acessar todos os novos recursos, peça o `Storage-as-a-Service Package 759`.
{:tip}

## Conectando seu novo armazenamento
{: #mountingstorage}

Quando a solicitação de provisionamento for concluída, autorize seus hosts a acessar o novo armazenamento e configure sua conexão. Dependendo do sistema operacional do seu host, siga o link apropriado.
- [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurando o Block Storage para backup com cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurando o Block Storage para backup com Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Gerenciando seu novo Armazenamento

Por meio do portal ou do SLCLI, é possível gerenciar vários aspectos de seu {{site.data.keyword.blockstorageshort}}, tais como autorizações e cancelamentos de host. Para obter mais informações, consulte [Gerenciando o {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
