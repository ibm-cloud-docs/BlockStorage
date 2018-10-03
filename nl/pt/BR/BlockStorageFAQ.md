---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-18"

---
{:new_window: target="_blank"}

# FAQ

## Quantas instâncias podem compartilhar o uso de um volume do {{site.data.keyword.blockstorageshort}}?
O limite padrão do número de autorizações por volume de bloco é oito. Isso significa que até oito hosts podem ser autorizados a acessar o LUN do Block Storage. Para solicitar um aumento de limite, entre em contato com o representante de vendas.

## Quantos volumes podem ser solicitados?
Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar seu limite de volume, entre em contato com o representante de vendas. Para obter mais informações, veja [Gerenciando limites de armazenamento](managing-storage-limits.html).

## Quantos volumes do {{site.data.keyword.blockstorageshort}} podem ser montados em um host?
Isso depende do que o sistema operacional do host é capaz de manipular, não é algo limitado pelo {{site.data.keyword.BluSoftlayer_full}}. Consulte a documentação do S.O. para conhecer os limites com relação ao número de volumes que podem ser montados.

## O limite de IOPS alocado é aplicado por instância ou por volume?
O IOPS é cumprido no nível de volume. Em outras palavras, dois hosts conectados a um volume
com 6.000 IOPS compartilham essas 6.000 IOPS.

## Medindo IOPS
O IOPS é medido com base em um perfil de carregamento de blocos de 16 KB com 50 por cento de leitura e 50 por cento de gravações aleatórias. As cargas de trabalho que diferirem desse perfil poderão enfrentar desempenho inferior.

## O que acontece quando um tamanho de bloco menor é usado para medir o desempenho?
O IOPS máximo ainda poderá ser obtido quando você usar tamanhos de bloco menores. No entanto, o rendimento torna-se menor. Por exemplo, um volume com 6.000 IOPS teria o rendimento a seguir em vários tamanhos de bloco:

- 16 KB * 6.000 IOPS == ~93,75 MB/s 
- 8 KB * 6.000 IOPS == ~46,88 MB/s
- 4 KB * 6.000 IOPS == ~23,44 MB/s

## O volume precisa estar pré-aquecido para atingir o rendimento esperado?
Não há necessidade de pré-aquecimento. É possível observar o rendimento especificado imediatamente ao provisionar o volume.

## É possível obter mais rendimento usando uma conexão Ethernet mais rápida?
Os limites de rendimento são configurados de acordo com o nível de volume/LUN, portanto, o uso de uma conexão Ethernet mais rápida não aumenta esse limite configurado. No entanto, com uma conexão Ethernet mais lenta, sua
largura da banda pode ser um gargalo potencial.

## Os firewalls/grupos de segurança afetam o desempenho?
É melhor executar o tráfego de armazenamento em uma VLAN, que efetua bypass do firewall. A execução do tráfego de armazenamento por meio de firewalls de software aumenta a latência e afeta negativamente o desempenho do armazenamento.

## Qual latência pode ser esperada do {{site.data.keyword.blockstorageshort}}?   
A latência de destino dentro do armazenamento é <1 ms. O armazenamento é conectado a instâncias de cálculo em uma rede compartilhada, portanto, a latência exata de desempenho depende do tráfego de rede durante a operação.

## Por que o {{site.data.keyword.blockstorageshort}} com uma camada de 10 IOPS/GB do Endurance pode ser pedido em alguns data centers e em outros não?
A camada de 10 IOPS/GB do Endurance tipo {{site.data.keyword.blockstorageshort}} está disponível somente em data centers selecionados e novos data centers estão sendo incluídos gradualmente. É possível localizar uma lista completa de
data centers submetidos a upgrade e de recursos disponíveis
[aqui](new-ibm-block-and-file-storage-location-and-features.html).

## Como podemos dizer quais LUNs/volumes do {{site.data.keyword.blockstorageshort}} são criptografados?
Ao examinar sua lista de {{site.data.keyword.blockstorageshort}} no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, é possível ver um ícone de bloqueio à direita do nome de LUN/volume para os LUNs/volumes criptografados.

## Como sabemos quando estamos provisionando o {{site.data.keyword.blockstorageshort}} em um data center submetido a upgrade?
Ao pedir o {{site.data.keyword.blockstorageshort}}, todos os data centers submetidos a upgrade são denotados com um asterisco (`*`) no formulário de pedido e uma indicação de que você está prestes a provisionar armazenamento com criptografia. Quando o armazenamento é provisionado, é possível ver um ícone na lista de armazenamento mostrando esse armazenamento como criptografado. Todos os volumes criptografados e os LUNs são provisionados somente em data centers submetidos a upgrade. É possível localizar uma lista completa de
data centers submetidos a upgrade e de recursos disponíveis
[aqui](new-ibm-block-and-file-storage-location-and-features.html).

## Se possuímos um {{site.data.keyword.blockstorageshort}} não criptografado em um data center que tenha sido submetido a upgrade recentemente, podemos criptografar esse {{site.data.keyword.blockstorageshort}}?
O {{site.data.keyword.blockstorageshort}} que é provisionado antes do upgrade do data center não pode ser criptografado. 
O novo {{site.data.keyword.blockstorageshort}} provisionado em data centers submetidos a upgrade é criptografado automaticamente. Não há configuração de criptografia para escolher, é automático. 
Os dados em armazenamento não criptografado em um data center submetido a upgrade podem ser criptografados
criando um novo LUN de bloco e, em seguida, copiando os dados para o novo LUN criptografado com migração baseada em host. Clique [aqui](migrate-block-storage-encrypted-block-storage.html) para obter instruções.

## O {{site.data.keyword.blockstorageshort}} suporta a Reserva Persistente SCSI-3
para implementar o fence de E/S para Db2 pureScale?
Sim, o {{site.data.keyword.blockstorageshort}} suporta as reservas persistentes SCSI-2 e SCSI-3.

## O que acontece com os dados quando os LUNs do {{site.data.keyword.blockstorageshort}} são excluídos?
O {{site.data.keyword.blockstoragefull}} apresenta volumes de Bloco aos clientes em armazenamento físico cujos dados são apagados antes da reutilização. Os clientes com necessidades especiais de conformidade, como as Diretrizes para sanitização de mídias NIST 800-88, devem executar o procedimento de sanitização de dados antes de excluir seu armazenamento.

## O que acontece com as unidades que são desatribuídas do centro de dados de nuvem?
Quando as unidades são desatribuídas, a IBM as destrói antes de elas serem descartadas. As unidades se tornam inutilizáveis. Quaisquer dados gravados nessas unidades se tornam inacessíveis.
