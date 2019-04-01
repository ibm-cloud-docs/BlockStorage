---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-14"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Sobre {{site.data.keyword.blockstorageshort}}
{: #About}

O {{site.data.keyword.blockstoragefull}} é um armazenamento iSCSI persistente e de alto desempenho provisionado e gerenciado independentemente de instâncias de cálculo. Os LUNs do {{site.data.keyword.blockstorageshort}} baseados em iSCSI são conectados a dispositivos autorizados por meio de conexões Multi-path I/O (MPIO).

O {{site.data.keyword.blockstorageshort}} traz os melhores níveis de durabilidade e disponibilidade com um conjunto de recursos incomparável. Ele é construído usando padrões de mercado e melhores práticas. O {{site.data.keyword.blockstorageshort}} foi projetado para proteger a integridade dos dados e manter a disponibilidade por meio de eventos de manutenção e falhas não planejadas, além de fornecer uma linha de base de desempenho consistente.

## Recursos principais
{: #corefeatures}

Aproveite os recursos do {{site.data.keyword.blockstorageshort}} a seguir:

- **Linha de base de desempenho consistente**
   - Fornecido por meio da alocação de IOPS de nível de protocolo para volumes individuais.
- **Altamente durável e resiliente**
   - Protege a integridade dos dados e mantém a disponibilidade por meio dos eventos de manutenção e de falhas não planejadas sem a necessidade de criar e gerenciar matrizes Redundant Array of Independent Disks (RAID) no nível do sistema operacional.
- **Criptografia de dados em repouso** ([Disponível em data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Criptografia gerenciada por provedor para dados em repouso sem custo adicional.
- **Todo armazenamento suportado em flash** ([Disponível em data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Armazenamento totalmente em flash para volumes provisionados com o Endurance ou o Performance em níveis de 2 IOPS/GB ou mais altos.
- **Capturas instantâneas** ([Disponíveis em data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Obtém capturas instantâneas de dados de um momento de modo ininterrupto.
- **Replicação** ([Disponível em data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Copia capturas instantâneas automaticamente para um data center parceiro do {{site.data.keyword.BluSoftlayer_full}}.
- **Conectividade altamente disponível**
   - Usa conexões de rede redundantes para maximizar a disponibilidade
   - O {{site.data.keyword.blockstorageshort}} baseado em iSCSI usa Multipath I/O (MPIO).
- **Acesso simultâneo**
   - Permite que múltiplos hosts acessem simultaneamente volumes de bloco de acesso (até oito) para configurações em cluster.
- **Bancos de dados em cluster**
   - Suporta casos de uso avançados, como bancos de dados em cluster.

## Faturamento
{: #billing}

É possível selecionar o faturamento por hora ou mensal para um LUN de bloco. O tipo de faturamento selecionado para um LUN aplica-se a seu espaço de captura instantânea e réplicas. Por exemplo, se você provisionar um LUN com o faturamento por hora, quaisquer taxas de capturas instantâneas ou de réplica serão faturadas por hora. Se você provisionar um LUN com faturamento mensal, quaisquer taxas de capturas instantâneas ou de réplicas serão faturadas mensalmente.

Com **faturamento por hora**, o número de horas de existência do LUN de bloco na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento, o que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível para o armazenamento provisionado somente em [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

Com o **faturamento mensal**, o cálculo do preço é rateado a partir da data
de criação até o término do ciclo de faturamento e é cobrado imediatamente. Se um LUN for excluído antes do término do ciclo de faturamento, não haverá reembolso. O faturamento mensal é uma boa opção para o armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).

**Performance**
<table>
  <caption>A Tabela 1 está mostrando os preços para o armazenamento do Performance com faturamento mensal e por hora.</caption>
  <tr>
   <th>Preço Mensal</th>
   <td>$0,10/GB + $0,07/IOP</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>US$ 0,0001/GB + US$ 0,0002/IOP</td>
  </tr>
</table>

** Endurance **
<table>
  <caption>A Tabela 2 está mostrando os preços para o armazenamento do Endurance para cada camada com opções de faturamento mensais e por hora.</caption>
  <tr>
   <th>Camada de IOPS</th>
   <th>0,25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>Preço Mensal</th>
   <td>US$ 0,06/GB</td>
   <td>US$ 0,15/GB</td>
   <td>$0,20/GB</td>
   <td>$0,58/GB</td>
  </tr>
  <tr>
   <th>Preço por hora</th>
   <td>US$ 0,0001/GB</td>
   <td>US$ 0,0002/GB</td>
   <td>$0,0003/GB</td>
   <td>$0,0009/GB</td>
  </tr>
</table>



## Fornecimento
{: #provisioning}

Os LUNs do {{site.data.keyword.blockstorageshort}} podem ser provisionados de 20 GB a 12 TB com duas opções: <br/>
- Provisiona camadas do **Endurance** que apresentam níveis de desempenho predefinidos e outros recursos, como capturas instantâneas e replicação.
- Construa um ambiente de **Desempenho** poderoso com operações de
entrada/saída por segundo (IOPS) alocadas.

### Fornecimento com Camadas de Endurance
{: #provendurance}

O {{site.data.keyword.blockstorageshort}} Endurance está disponível em quatro camadas de desempenho do IOPS para suportar necessidades de aplicativo variadas. <br />

- **0,25 IOPS por GB** é projetado para cargas de trabalho com baixa intensidade de
E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos a qualquer momento. Aplicativos de exemplo incluem o armazenamento de caixas postais ou compartilhamentos de arquivo de nível departamental.

- **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco da VM para um hypervisor.

- **4 IOPS por GB** é projetado para cargas de trabalho com maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos a qualquer momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis a desempenho.

- **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível para o armazenamento provisionado até 4 TB em [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations) apenas.

Até 48.000 IOPS estão disponíveis com um volume do Endurance de 12 TB.

A escolha da camada correta do Endurance é essencial para sua carga de trabalho. É igualmente importante usar o tamanho de bloco, a velocidade de conexão Ethernet e o número de hosts corretos necessários para alcançar o máximo desempenho. Se alguma dessas partes não se alinhar, poderá haver um impacto significativo no rendimento resultante.


### Provisionando com Desempenho
{: #provperformance}

Performance é uma classe do {{site.data.keyword.blockstorageshort}} projetada para suportar aplicativos de alta E/S com requisitos de desempenho entendidos que não se ajustam bem em uma camada do Endurance. Um desempenho previsível é atingido por meio da alocação
de IOPS de nível de protocolo para volumes individuais. Várias taxas de IOPS (100 - 48.000) podem ser provisionadas com tamanhos de armazenamento que variam de 20 GB a 12 TB.

O Performance para o {{site.data.keyword.blockstorageshort}} é acessado e montado por meio de uma conexão Small Computer System Interface (iSCSI) da internet de Multipath I/O (MPIO). O {{site.data.keyword.blockstorageshort}} é usado geralmente quando o volume é acessado por um único servidor. Múltiplos volumes podem ser montados em um host e divididos juntos para atingir volumes
e contagens de IOPS maiores. Os volumes do Performance podem ser pedidos de acordo com os tamanhos e as taxas de IOPS na Tabela 3 para os sistemas operacionais Linux, XEN e Windows.


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>A Tabela 3 está mostrando combinações de tamanho e de IOPS para armazenamento do Performance.<br/><sup><img src="/images/numberone.png" alt="Nota de rodapé" /></sup> Os limites de IOPS maiores que 6.000 estão disponíveis nos data centers selecionados.</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
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
            <td>6.000 ou 10.000 <sup><img src="/images/numberone.png" alt="Nota de rodapé" /></sup></td>
          </tr>
          <tr>
            <td>1.000</td>
            <td>100</td>
            <td>6.000 ou 20.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
          <tr>
            <td>2.000</td>
            <td>200</td>
            <td>6.000 ou 40.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
          <tr>
            <td>3.000-7.000</td>
            <td>300</td>
            <td>6.000 ou 48.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
          <tr>
            <td>8.000 a 9.000</td>
            <td>500</td>
            <td>6.000 ou 48.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
          <tr>
            <td>10.000 a 12.000</td>
            <td>1.000</td>
            <td>6.000 ou 48.000 <sup> <img src="/images/numberone.png" alt="Footnote" /> </sup></td>
          </tr>
</table>


Os volumes do Performance foram projetados para operar consistentemente próximo ao nível de IOPS provisionado. A consistência facilita dimensionar e escalar ambientes de aplicativos com um nível específico de desempenho. Além disso, é possível otimizar um ambiente construindo um volume com a proporção preço/desempenho ideal.
