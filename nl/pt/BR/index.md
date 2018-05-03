---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Sobre {{site.data.keyword.blockstorageshort}}

O {{site.data.keyword.blockstoragefull}} é um armazenamento iSCSI persistente de alto
desempenho que é provisionado e gerenciado, independentemente das instâncias de cálculo. Os LUNs
do {{site.data.keyword.blockstorageshort}} baseados em
iSCSI são conectados a dispositivos autorizados por meio de
conexões de E/S de caminhos múltiplos (MPIO) redundantes.

O {{site.data.keyword.blockstorageshort}} oferece os melhores níveis de durabilidade e
de disponibilidade com um conjunto de recursos incomparável e é construído usando padrões de mercado e
melhores práticas, projetado para proteger a integridade dos dados e manter a disponibilidade nos eventos de
manutenção e de falhas não planejadas enquanto fornece uma linha de base de desempenho consistente.

## Recursos principais

Aproveite os recursos do {{site.data.keyword.blockstorageshort}} a seguir:

- **Linha de base de desempenho consistente**
   - Fornecido pela alocação de IOPS de nível de protocolo para volumes individuais
- **Altamente durável e resiliente**
   - Protege a integridade dos dados e mantém a disponibilidade nos eventos de manutenção e
de falhas não planejadas sem a necessidade de criar e de gerenciar matriz redundante de nível de sistema
operacional de matrizes de disco independente (RAID)
- **Criptografia de dados em repouso**
([Disponível em data centers de
seleção](new-ibm-block-and-file-storage-location-and-features.html)).
   - Criptografia gerenciada por provedor para dados em repouso sem nenhum custo adicional
- **Armazenamento totalmente suportado para flash**
([Disponível em data centers
selecionados](new-ibm-block-and-file-storage-location-and-features.html)).
   - Todo o armazenamento flash para volumes provisionados com Resistência ou Desempenho a 2 IOPS/GB
ou superior
- **Capturas instantâneas** (quando provisionadas com Resistência ou Desempenho
nos [data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html).
   - Obtém capturas instantâneas de dados em um momento de modo ininterrupto
- **Replicação** (quando provisionada com Resistência ou Desempenho em
[data centers de seleção](/new-ibm-block-and-file-storage-location-and-features.html).
   - Copia automaticamente capturas instantâneas para um
data center do {{site.data.keyword.BluSoftlayer_full}} parceiro
- **Conectividade altamente disponível**
   - Usa conexões de rede redundantes para maximizar a disponibilidade, o
{{site.data.keyword.blockstorageshort}} baseado em iSCSI usa E/S de caminhos múltiplos (MPIO)
- **Acesso simultâneo**
   - Permite que múltiplos hosts acessem simultaneamente volumes de bloco (até 8) para configurações
em cluster.
- **Bancos de dados em cluster**
   - Suporta casos de uso avançados, como bancos de dados em cluster
     
## Faturamento por hora/mensal

É possível selecionar o faturamento por hora ou mensal para um LUN de bloco. O tipo de
faturamento selecionado para um LUN será aplicado ao seu espaço e às suas réplicas de captura instantânea. Por
exemplo, se você provisionar um LUN com um faturamento por hora, quaisquer taxas de capturas instantâneas ou
de réplica serão cobradas por hora. Se você provisionar um LUN com faturamento mensal, quaisquer taxas
de capturas instantâneas ou de réplicas serão cobradas mensalmente. 

Com o **faturamento por hora**, o cálculo do número de horas em que o LUN de bloco
existiu na conta é feito no momento em que o LUN é excluído ou no término do ciclo de faturamento, que nunca
vem antes.  O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível apenas para armazenamento provisionado em
[data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html). 

Com o **faturamento mensal**, o cálculo do preço é rateado a partir da data
de criação até o término do ciclo de faturamento e é cobrado imediatamente. Não há reembolso quando um LUN é
excluído antes do término do ciclo de faturamento.  O faturamento mensal é uma boa opção para armazenamento
usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos
períodos de tempo (um mês ou mais). 

### Desempenho:
<table>
 <tbody>
  <tr>
   <th>Preço Mensal</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>$0,0001/GB + $0,0002/IOP</td>
  </tr>
  </tbody>
</table>
 
### Resistência:
<table>
 <tbody>
  <tr>
   <th>Camada de IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Preço Mensal</th>
   <td>$0,10/GB</td>
   <td>$0,20/GB</td>
   <td>$0,35/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>0,0002 /GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0005/GB</td>
   <td>$0,0009/GB</td>
  </tr>
  </tbody>
</table>



## Fornecimento

Os LUNs do {{site.data.keyword.blockstorageshort}} podem ser provisionados de 20 GB até 12 TB com
duas opções para fornecimento: <br/>
- Provisione camadas de **Resistência** com níveis de
desempenho e recursos predefinidos, como capturas instantâneas e replicação.
- Construa um ambiente de **Desempenho** poderoso com operações de
entrada/saída por segundo (IOPS) alocadas. 

### Camadas de resistência

A Resistência está disponível em três camadas de desempenho de IOPS para suportar diferentes necessidades
do aplicativo. <br />

- **0,25 IOPS por GB** é projetado para cargas de trabalho com baixa intensidade de
E/S. Essas cargas de trabalho são tipicamente caracterizadas por terem uma alta porcentagem de dados inativos em um determinado momento. Aplicativos de exemplo incluem o armazenamento de caixas postais ou compartilhamentos de arquivo de nível departamental.
- **2 IOPS por GB** é projetado para uso de propósito geral. Aplicativos de exemplo incluem a hospedagem de pequenos bancos de dados que auxiliam os aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
- **4 IOPS por GB** é projetado para cargas de trabalho com maior intensidade. Essas cargas de trabalho são tipicamente caracterizadas por terem uma alta porcentagem de dados ativos em um determinado momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis a desempenho.
- **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como
aquelas criadas por bancos de dados NoSQL e para processamento de dados para análise de dados.  Esta camada está
disponível para um armazenamento provisionado até 4 TB de tamanho nos
[data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html).

Até 48.000 IOPS estão disponíveis com um volume de Resistência de 12 TB.
 

Ao escolher o nível certo do {{site.data.keyword.blockstorageshort}} de Resistência,
é igualmente importante e fundamental utilizar o tamanho de bloco, a velocidade de conexão Ethernet
e o número de hosts necessários para atingir o desempenho máximo. Se alguma dessas partes não se alinhar
entre si, poderá haver um impacto significativo no rendimento resultante.

 
### Performance

O Desempenho é uma classe do {{site.data.keyword.blockstorageshort}} que é projetada para
suportar aplicativos de alta taxa de E/S com requisitos de desempenho bem entendidos que não se
ajustam adequadamente em uma camada de Resistência. Um desempenho previsível é atingido por meio da alocação
de IOPS de nível de protocolo para volumes individuais. Entre 100 e 48.000 IOPS podem ser provisionadas com
tamanhos de armazenamento que variam de 20 GB a 12 TB. 

O Desempenho para {{site.data.keyword.blockstorageshort}} é acessado e montado por meio de uma
conexão Small Computer System Interface (iSCSI) de E/S de caminhos múltiplos (MPIO). O {{site.data.keyword.blockstorageshort}} é geralmente utilizado quando o volume é acessado por uma
única máquina. Múltiplos volumes podem ser montados em um host e divididos juntos para atingir volumes
e contagens de IOPS maiores. Os volumes de desempenho podem ser solicitados de acordo com o tamanho e com as
IOPS na Tabela 1 para sistemas operacionais Linux, XEN, VMware e Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Tamanho (GB)</th>
            <th>Mínimo de IOPS</th>
            <th>Máximo de IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1.000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2.000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4.000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6.000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6.000 ou 10.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 ou 20.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>2.000 a 3.000</td>
            <td>200</td>
            <td>6.000 ou 40.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>4.000 a 7.000</td>
            <td>300</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>8.000 a 9.000</td>
            <td>500</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>10.000 a 12.000</td>
            <td>1.000</td>
            <td>6.000 ou 48.000<sup><img src="/images/numberone.png" alt="nota de rodapé" /></sup></td>
          </tr>
        </tbody>
</table>

O limite de IOPS <sup>![nota de rodapé](/images/numberone.png)</sup>
acima de 6.000 está disponível nos
[data centers de seleção](new-ibm-block-and-file-storage-location-and-features.html).


Os volumes de desempenho são projetados para executar consistentemente próximo ao nível de IOPS
provisionado. A consistência facilita o dimensionamento e o ajuste de escala de ambientes de aplicativos
com um determinado nível de desempenho. Além disso, dado o intervalo de tamanhos de volumes e de contagens de
IOPS, é possível otimizar um ambiente construindo um volume com a razão ideal entre preço e desempenho.

### Dicas para fornecimento de IOPS para o {{site.data.keyword.blockstorageshort}}

As IOPS para Resistência e Desempenho têm com base um tamanho de bloco de 16 KB com 50% de
leitura/gravação e 50% de carga de trabalho aleatória. Um bloco de 16 KB equivale a uma gravação no
volume.

O tamanho de bloco usado pelo seu aplicativo impacta diretamente o desempenho de armazenamento.  Se o
tamanho de bloco usado pelo seu aplicativo for menor que 16 KB, o limite de IOP será percebido antes do
limite de rendimento.  Por outro lado, se o tamanho de bloco usado pelo seu aplicativo for maior que 16 KB, o
limite de rendimento será percebido antes do limite de IOP.

A mudança do tamanho de bloco afetará o desempenho conforme a seguir:

<table cellpadding="1" cellspacing="1" style="width: 99%;">
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <tbody>
          <tr>
            <th>Tamanho de bloco (KB)</th>
            <th>IOPS</th>
            <th>Rendimento (MB/s)</th>
          </tr>
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
            <td>32 (típico para SQLServer)</td>
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

É importante escolher o {{site.data.keyword.blockstorageshort}} certo para sua carga de
trabalho e também saber como evitar gargalos. A velocidade da sua conexão de Ethernet deve ser mais rápida do
que o rendimento máximo esperado de seu volume. Como uma regra geral, você não espera saturar sua conexão
Ethernet além de 70% da largura da banda disponível. Por exemplo, se você tiver 6.000 IOPS e estiver usando um
tamanho de bloco de 16 KB, o volume terá uma capacidade aproximada de 94 MB por segundo. Se você tiver uma
conexão Ethernet de 1 Gbps para o LUN, torna-se um gargalo quando seus servidores tentam utilizar o
rendimento máximo disponível, já que 70% do limite teórico de uma conexão Ethernet de 1 Gbps (125 MB por
segundo) permitiria apenas 88 MB por segundo.


Outro fator a ser considerado é o número de hosts que estão utilizando o volume. Se apenas um único host
estiver acessando o volume, poderá ser difícil perceber o máximo de IOPS disponível, especialmente em
contagens de IOPS extremas (10.000s). Se a sua carga de trabalho requer alto rendimento, é melhor configurar
pelo menos dois ou três servidores para acessar seu volume para evitar gargalo em um único servidor.


Para obter o máximo de IOPS, recursos de rede adequados precisam estar em vigor. Outras considerações
incluem o uso de rede privada fora do lado de armazenamento e de host e os ajustes específicos do aplicativo
(pilha de IP, profundidades da fila e assim por diante).
