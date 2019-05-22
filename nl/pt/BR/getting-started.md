---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-07"

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

Para obter mais informações sobre a oferta do {{site.data.keyword.blockstorageshort}}, consulte [Sobre o {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-About).

## Considerações de Fornecimento

### Tamanho do bloco

O IOPS para Duração e Desempenho é baseado em um tamanho de bloco de 16 KB com uma carga de trabalho aleatória/sequencial 50/50 de leitura/gravação 50/50. Um bloco de 16 KB equivale a uma gravação no volume.
{:important}

O tamanho do bloco usado por seu aplicativo afetará diretamente o desempenho do armazenamento. Se o tamanho do bloco usado por seu aplicativo for menor que 16 KB, o limite do IOPS será realizado antes do limite do rendimento. Por outro lado, se o tamanho do bloco usado por seu aplicativo for maior que 16 KB, o limite de rendimento será realizado antes do limite do IOPS.

<table>
  <caption>A Tabela 4 mostra exemplos de como o tamanho do bloco e o IOPS afetam o rendimento.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>Tamanho de bloco (KB)</th>
            <th>IOPS</th>
            <th>Rendimento (MB/s)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (típico para Linux)</td>
            <td>1.000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (típico para Oracle)</td>
            <td>1.000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1.000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (típico para o SQL Server)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

### Hosts autorizados

Outro fator a ser considerado é o número de hosts que estão usando seu volume. Se houver um único host acessando o volume, poderá ser difícil realizar o IOPS máximo disponível, especialmente em contagens extremas de IOPS (10.000s). Se a sua carga de trabalho requerer alto rendimento, será melhor configurar pelo menos alguns servidores para acessar seu volume para evitar um gargalo de servidor único.

### Conexão de rede

A velocidade da sua conexão de Ethernet deve ser mais rápida do
que o rendimento máximo esperado de seu volume. Em geral, não espere saturar sua conexão Ethernet além de 70% da largura de banda disponível. Por exemplo, se você tiver 6.000 IOPS e estiver usando um tamanho de bloco de 16 KB, o volume poderá manipular o rendimento de aproximadamente 94 MBps. Se você tiver uma conexão Ethernet de 1 Gbps com seu LUN, ela se tornará um gargalo quando seus servidores tentarem usar o rendimento máximo disponível. Isso porque 70% do limite teórico de uma conexão Ethernet de 1 Gbps (125 MB por segundo) permitiria 88 MB por segundo apenas.

Para obter o máximo de IOPS, recursos de rede adequados precisam estar em vigor. Outras considerações incluem o uso
de rede privada fora do armazenamento e os ajustes específicos do aplicativo e do lado do host (pilha
IP ou [profundidades da fila](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings) e
outras configurações).

O tráfego de armazenamento é incluído no uso total de rede de Virtual Servers Públicos. Para obter mais informações sobre os limites que podem ser impostos pelo serviço, consulte a [Documentação do Virtual Server](/docs/vsi?topic=virtual-servers-public-virtual-servers).
{:tip}

## Enviando sua Ordem
{: #submitorder}

Quando você estiver pronto para enviar seu pedido, poderá fazer isso por meio do [Console](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) ou da [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI).

## Conectando seu novo armazenamento
{: #mountingstorage}

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional do seu host, siga o link apropriado.
- [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Configurando o Block Storage para backup com cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurando o Block Storage para backup com Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Gerenciando seu novo Armazenamento

Por meio do portal ou do SLCLI, é possível gerenciar vários aspectos de seu Armazenamento de arquivos, tais como autorizações e cancelamentos do host. Para obter mais informações, consulte [Gerenciando o {{site.data.keyword.blockstorageshort}}](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage).
