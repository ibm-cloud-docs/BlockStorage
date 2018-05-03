---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-24"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Perguntas mais frequentes do {{site.data.keyword.blockstorageshort}}

## Quantas instâncias podem compartilhar o uso de um
volume fornecido do {{site.data.keyword.blockstorageshort}}?
O limite padrão para o número de autorizações por volume de bloco é 8. Entre em contato com seu
representante de vendas para aumentar o limite.

## Durante o fornecimento do {{site.data.keyword.blockstorageshort}} de Desempenho ou
Resistência, as IOPS alocadas são aplicadas por instância ou por volume?
As IOPS são aplicadas no nível de volume. Em outras palavras, dois hosts conectados a um volume
com 6.000 IOPS compartilham essas 6.000 IOPS.

## Como as IOPS são medidas?
As IOPS são medidas com base em um perfil de carregamento de blocos de 16 KB com 50% de leitura
e 50% de gravações aleatórias. As cargas de trabalho que diferem deste perfil podem ter um desempenho inferior.

## O que acontece se eu usar um tamanho de bloco menor ao medir o desempenho?
O máximo de IOPS ainda pode ser obtido ao usar tamanhos de blocos menores, porém o rendimento é menor. Por
exemplo, um volume com 6.000 IOPS teria o rendimento a seguir em vários tamanhos de bloco:

- 16 KB * 6.000 IOPS == ~93,75 MB/s 
-  8 KB * 6.000 IOPS == ~46,88 MB/s
-  4 KB * 6.000 IOPS == ~23,44 MB/s

## O volume precisa estar pré-aquecido para atingir o rendimento esperado?
Não há necessidade de pré-aquecimento. Você observará o rendimento especificado imediatamente após o
fornecimento do volume.

## Por que eu posso provisionar o {{site.data.keyword.blockstorageshort}} com uma camada de resistência de IOPS 10 em alguns data centers e não em outros?
A camada de 10 IOPS/GB do tipo de armazenamento Resistência
do {{site.data.keyword.blockstorageshort}}
está disponível apenas em data centers de seleção, com novos data centers sendo incluídos em breve.  É possível localizar uma lista completa de data centers atualizados e de recursos disponíveis aqui.

## Como saber quais dos meus LUNs/Volumes do {{site.data.keyword.blockstorageshort}}
estão criptografados?
Ao visualizar sua lista de {{site.data.keyword.blockstorageshort}} no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window},
é exibido um ícone de bloqueio à direita do nome do LUN/volume para aqueles que estão criptografados.

## Como eu sei se estou provisionando o {{site.data.keyword.blockstorageshort}} em um data
center submetido a upgrade?
Ao provisionar o {{site.data.keyword.blockstorageshort}}, todos os data centers submetidos a
upgrade são indicados com um asterisco (`*`) no formulário de pedido e com uma indicação de
que você provisionará o armazenamento com criptografia. Quando o armazenamento é provisionado, é exibido um ícone na
lista de armazenamentos que mostra esse volume ou volumes como criptografados. Todos os volumes criptografados
e os volumes são provisionados somente em data centers submetidos a upgrade. É possível localizar uma lista completa de data centers atualizados e de recursos disponíveis aqui.

## Por que eu posso provisionar o {{site.data.keyword.blockstorageshort}} com uma camada de resistência de IOPS 10 em alguns data centers e não em outros?
A camada de 10 IOPS/GB do tipo Resistência está disponível apenas em data centers
de seleção, com novos data centers sendo incluídos em breve.  É possível localizar uma lista completa de
data centers submetidos a upgrade e de recursos disponíveis
[aqui](new-ibm-block-and-file-storage-location-and-features.html).

## Se eu tiver provisionado um {{site.data.keyword.blockstorageshort}} não criptografado em um
data center que foi submetido a upgrade para criptografia, posso criptografar meu
{{site.data.keyword.blockstorageshort}}?
O {{site.data.keyword.blockstorageshort}} que é fornecido antes de um upgrade do data center não
pode ser criptografado. 
O novo {{site.data.keyword.blockstorageshort}} provisionado em data centers submetidos a upgrade é
criptografado automaticamente; não há opção de criptografia para escolha, pois ela é automática. 
Os dados em armazenamento não criptografado em um data center submetido a upgrade podem ser criptografados
criando um novo LUN de bloco e, em seguida, copiando os dados para o novo LUN criptografado com migração baseada em host. Consulte este [artigo](migrate-block-storage-encrypted-block-storage) para obter instruções
sobre como executar a migração.

## Quantos volumes posso provisionar?
Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}.  Entre em contato com seu representante de vendas para aumentar seus volumes.

## Terei mais rendimento se eu usar uma conexão Ethernet mais rápida?
Os limites de rendimento são configurados em um nível por volume/LUN, portanto, o uso de uma conexão
Ethernet mais rápida não aumentará esse limite configurado. No entanto, com uma conexão Ethernet mais lenta, sua
largura da banda pode ser um gargalo potencial.

## Os firewalls/grupos de segurança impactam o desempenho?
Recomendamos a execução do tráfego de armazenamento em uma vlan que ignora o firewall como uma melhor
prática. Executar o tráfego de armazenamento através de firewalls de software aumentará a latência e afetará adversamente o desempenho de armazenamento.

## Qual latência de desempenho posso esperar do meu {{site.data.keyword.blockstorageshort}}?   

A latência de destino dentro do armazenamento é < 1 ms. Como o armazenamento é conectado às
instâncias de cálculo em uma rede compartilhada, a latência de desempenho exata dependerá do tráfego de
rede dentro de um intervalo de tempo especificado.

## O {{site.data.keyword.blockstorageshort}} suporta a Reserva Persistente SCSI-3
para implementar o fence de E/S para Db2 pureScale?
Sim, o {{site.data.keyword.blockstorageshort}} suporta as reservas persistentes SCSI-2 e SCSI-3.

## O que acontece com meus dados quando os LUNs do {{site.data.keyword.blockstorageshort}}
são excluídos?

Quando o armazenamento é excluído, quaisquer ponteiros para os dados naquele volume são removidos, de
modo que os dados se tornam completamente inacessíveis. Quando o armazenamento físico é reprovisionado para
outra conta, um novo conjunto de ponteiros é designado. Não há meios para que a nova conta acesse
quaisquer dados que possam ter estado no armazenamento físico e todo o novo conjunto de ponteiros é
exibido como 0. Quando novos dados são gravados no volume/LUN, quaisquer dados inacessíveis que ainda
existam são sobrescritos.

## O que acontece com as unidades que são desatribuídas do centro de dados de nuvem?

Quando as unidades são desatribuídas, a IBM as destrói antes de descartá-las, tornando-as inutilizáveis. Quaisquer dados gravados nessas unidades se tornam inacessíveis.
