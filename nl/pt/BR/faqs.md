---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# FAQ

## Quantas instâncias podem compartilhar o uso de um volume do {{site.data.keyword.blockstorageshort}}?
{: faq}

O limite padrão do número de autorizações por volume de bloco é oito. Isso significa que até oito hosts podem ser autorizados a acessar o LUN do Block Storage. Para solicitar um aumento de limite, entre em contato com o representante de vendas.

## Quantos volumes podem ser solicitados?
{: faq}

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar seu limite de volume, entre em contato com o representante de vendas. Para obter mais informações, veja [Gerenciando limites de armazenamento](managing-storage-limits.html).

## Quantos volumes do {{site.data.keyword.blockstorageshort}} podem ser montados em um host?
{: faq}

Isso depende do que o sistema operacional do host é capaz de manipular, não é algo limitado pelo {{site.data.keyword.BluSoftlayer_full}}. Consulte a documentação do S.O. para conhecer os limites com relação ao número de volumes que podem ser montados.

## Qual versão do Windows devo escolher para o LUN do meu Block Storage?
{: faq}

Ao criar um LUN, deve-se especificar o tipo de S.O. O tipo de S.O. deve se basear no sistema operacional que é usado pelos hosts que acessam o LUN. O tipo de S.O. não pode ser modificado depois da criação do LUN. O tamanho real do LUN
pode variar um pouco com base no tipo de S.O. do LUN.

** Windows 2008 ou mais recente                                     **
- O LUN armazena dados do Windows para o Windows 2008 e versões mais recentes. Use essa opção de S.O. se o
sistema operacional do host for Windows Server 2008, Windows Server 2012 e Windows Server 2016. Os métodos de
particionamento MBR e GPT são suportados.

**Windows 2003**
- O LUN armazena um tipo de disco rígido em um disco do Windows de partição única que usa o estilo de particionamento Master Boot Record (MBR). Use essa opção apenas se o sistema operacional do host for o Windows 2000 Server, o Windows XP ou o Windows Server 2003 que usa o método de particionamento MBR.

**GPT do Windows**
-  O LUN armazena dados do Windows usando o estilo de particionamento GUID Partition Type (GPT). Use
essa opção se você deseja usar o método de particionamento GPT e seu host pode usá-lo. O Windows
Server 2003, Service Pack 1 e mais recente pode usar o método de particionamento GPT e todas as versões
de 64 bits do Windows suportam isso.

## O limite de IOPS alocado é aplicado por instância ou por volume?
{: faq}

O IOPS é cumprido no nível de volume. Em outras palavras, dois hosts conectados a um volume
com 6.000 IOPS compartilham essas 6.000 IOPS.

## Medindo IOPS
{: faq}

O IOPS é medido com base em um perfil de carregamento de blocos de 16 KB com 50% de leitura e 50% de gravações aleatórias. As cargas de trabalho que diferirem desse perfil poderão enfrentar desempenho inferior.

## O que acontece quando um tamanho de bloco menor é usado para medir o desempenho?
{: faq}

O IOPS máximo ainda poderá ser obtido quando você usar tamanhos de bloco menores. No entanto, o rendimento torna-se menor. Por exemplo, um volume com 6.000 IOPS teria o rendimento a seguir em vários tamanhos de bloco:

- 16 KB * 6.000 IOPS == ~93,75 MB/s
- 8 KB * 6.000 IOPS == ~46,88 MB/s
- 4 KB * 6.000 IOPS == ~23,44 MB/s

## O volume precisa estar pré-aquecido para atingir o rendimento esperado?
{: faq}

Não há necessidade de pré-aquecimento. É possível observar o rendimento especificado imediatamente ao provisionar o volume.

## É possível obter mais rendimento usando uma conexão Ethernet mais rápida?
{: faq}

Os limites de rendimento são configurados em um nível por LUN, portanto, o uso de uma conexão Ethernet mais rápida não aumenta esse limite configurado. No entanto, com uma conexão Ethernet mais lenta, sua
largura da banda pode ser um gargalo potencial.

## Os firewalls e os grupos de segurança afetam o desempenho?
{: faq}

É melhor executar o tráfego de armazenamento em uma VLAN, que efetua bypass do firewall. A execução do tráfego de armazenamento por meio de firewalls de software aumenta a latência e afeta negativamente o desempenho do armazenamento.

## Qual latência pode ser esperada do {{site.data.keyword.blockstorageshort}}?   
{: faq}

A latência de destino dentro do armazenamento é <1 ms. O armazenamento é conectado a instâncias de cálculo em uma rede compartilhada, portanto, a latência exata de desempenho depende do tráfego de rede durante a operação.

## Por que o {{site.data.keyword.blockstorageshort}} com uma camada de 10 IOPS/GB do Endurance pode ser pedido em alguns data centers e em outros não?
{: faq}

A camada de 10 IOPS/GB do Endurance tipo {{site.data.keyword.blockstorageshort}} está disponível somente em data centers selecionados e novos data centers estão sendo incluídos gradualmente. É possível localizar uma lista completa de
data centers submetidos a upgrade e de recursos disponíveis
[aqui](new-ibm-block-and-file-storage-location-and-features.html).

## Como podemos dizer quais volumes do {{site.data.keyword.blockstorageshort}} são criptografados?
{: faq}

Ao olhar para a sua lista do {{site.data.keyword.blockstorageshort}} no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}, é possível ver um ícone de bloqueio ao lado do nome do volume para os LUNs que estão criptografados.

## Como sabemos quando estamos provisionando o {{site.data.keyword.blockstorageshort}} em um data center submetido a upgrade?
{: faq}

Ao pedir o {{site.data.keyword.blockstorageshort}}, todos os data centers submetidos a upgrade são denotados com um asterisco (`*`) no formulário de pedido e uma indicação de que você está prestes a provisionar armazenamento com criptografia. Quando o armazenamento é provisionado, é possível ver um ícone na lista de armazenamento mostrando esse armazenamento como criptografado. Todos os volumes criptografados e os LUNs são provisionados somente em data centers submetidos a upgrade. É possível localizar uma lista completa de
data centers submetidos a upgrade e de recursos disponíveis
[aqui](new-ibm-block-and-file-storage-location-and-features.html).

## Se possuímos um {{site.data.keyword.blockstorageshort}} não criptografado em um data center que tenha sido submetido a upgrade recentemente, podemos criptografar esse {{site.data.keyword.blockstorageshort}}?
{: faq}

O {{site.data.keyword.blockstorageshort}} que é provisionado antes do upgrade do data center não pode ser criptografado.
O novo {{site.data.keyword.blockstorageshort}} que é fornecido em data centers com upgrade é criptografado automaticamente. Não há configuração de criptografia para escolher, é automático.
Os dados em armazenamento não criptografado em um data center submetido a upgrade podem ser criptografados
criando um novo LUN de bloco e, em seguida, copiando os dados para o novo LUN criptografado com migração baseada em host. Clique [aqui](migrate-block-storage-encrypted-block-storage.html) para obter instruções.

## O {{site.data.keyword.blockstorageshort}} suporta a Reserva Persistente SCSI-3
para implementar o fence de E/S para Db2 pureScale?
{: faq}

Sim, o {{site.data.keyword.blockstorageshort}} suporta as reservas persistentes SCSI-2 e SCSI-3.

## O que acontece com os dados quando os LUNs do {{site.data.keyword.blockstorageshort}} são excluídos?
{: faq}

O {{site.data.keyword.blockstoragefull}} apresenta volumes de bloco aos clientes em armazenamento físico que é limpo antes de qualquer reutilização. Os clientes com necessidades especiais de conformidade, como as Diretrizes para sanitização de mídias NIST 800-88, devem executar o procedimento de sanitização de dados antes de excluir seu armazenamento.

## O que acontece com as unidades que são desatribuídas do centro de dados de nuvem?
{: faq}

Quando as unidades são desatribuídas, a IBM as destrói antes de elas serem descartadas. As unidades se tornam inutilizáveis. Quaisquer dados gravados nessas unidades se tornam inacessíveis.
