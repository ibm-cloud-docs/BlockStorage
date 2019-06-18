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

# Aprenda sobre o {{site.data.keyword.blockstorageshort}}
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
   - Copia capturas instantâneas automaticamente para um data center parceiro do {{site.data.keyword.cloud}}.
- **Conectividade altamente disponível**
   - Usa conexões de rede redundantes para maximizar a disponibilidade
   - O {{site.data.keyword.blockstorageshort}} baseado em iSCSI usa Multipath I/O (MPIO).
- **Acesso simultâneo**
   - Permite que múltiplos hosts acessem simultaneamente volumes de bloco de acesso (até oito) para configurações em cluster.
- **Bancos de dados em cluster**
   - Suporta casos de uso avançados, como bancos de dados em cluster.


## Provisionando
{: #provisioning}

Os LUNs do {{site.data.keyword.blockstorageshort}} podem ser fornecidos de 20 GB a 12 TB com duas opções: <br/>
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

| Tamanho (GB) | Mínimo de IOPS | Máximo de IOPS
|-----|-----|-----|
| 20 | 100 | 1.000 |
| 40 | 100 | 2.000  |
| 80 | 100 | 4.000 |
| 100 | 100 | 6.000 |
| 250 | 100 | 6.000 |
| 500 | 100  | 6.000 ou 10.000 |
| 1.000 | 100 | 6.000 ou 20.000 ![Nota de rodapé](/images/numberone.png) |
| 2.000 | 200 | 6.000 ou 40.000 ![Nota de rodapé](/images/numberone.png) |
| 3.000-7.000 | 300 | 6.000 ou 48.000 ![Nota de rodapé](/images/numberone.png) |
| 8.000 a 9.000 | 500 | 6.000 ou 48.000 ![Nota de rodapé](/images/numberone.png) |
| 10.000 a 12.000 | 1.000 | 6.000 ou 48.000 ![Nota de rodapé](/images/numberone.png) |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparação de tabela" caption-side="top"}
{: summary="Table 1 is showing the possible minimum and maximum IOPS rates based of the volume size. This table has row and column headers. The row headers identify the volume size range. The column headers identify the minimum and maximum IOPS levels. To understand what IOPS rates you can expect from your Storage, navigate to the row and review the two options."}

![Nota de rodapé](/images/numberone.png) *Os limites de IOPS maiores que 6.000 estão disponíveis nos data centers selecionados.*

Os volumes do Performance foram projetados para operar consistentemente próximo ao nível de IOPS provisionado. A consistência facilita dimensionar e escalar ambientes de aplicativos com um nível específico de desempenho. Além disso, é possível otimizar um ambiente construindo um volume com a proporção preço/desempenho ideal.

## faturamento
{: #billing}

É possível selecionar o faturamento por hora ou mensal para um LUN de bloco. O tipo de faturamento selecionado para um LUN aplica-se a seu espaço de captura instantânea e réplicas. Por exemplo, se você provisionar um LUN com o faturamento por hora, quaisquer taxas de capturas instantâneas ou de réplica serão faturadas por hora. Se você provisionar um LUN com faturamento mensal, quaisquer taxas de capturas instantâneas ou de réplicas serão faturadas mensalmente.

 * Com **faturamento por hora**, o número de horas de existência do LUN de bloco na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento, o que vier primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível para o armazenamento provisionado somente em [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations).

 * Com o **faturamento mensal**, o cálculo do preço é rateado a partir da data
de criação até o término do ciclo de faturamento e é cobrado imediatamente. Se um LUN for excluído antes do término do ciclo de faturamento, não haverá reembolso. O faturamento mensal é uma boa opção para o armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).

### Endurance
{: #pricing-comparison-endurance}

| Opções de precificação para camadas IOPS predefinidas | 0,25 IOPS | 2 IOPS/GB | 4 IOPS/GB | 10 IOPS/GB |
|-----|-----|-----|-----|-----|
| Preço Mensal | US$ 0,06/GB | US$ 0,15/GB | $0,20/GB | $0,58/GB |
| Preço por hora | US$ 0,0001/GB | US$ 0,0002/GB | $0,0003/GB | $0,0009/GB |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparação de tabela" caption-side="top"}
{: summary="Table 2 is showing the prices for Endurance Storage for each tier with monthly and hourly billing options. This table has row and column headers. The row headers identify the billing options. The column headers identify the IOPS level that is chosen for the service. To understand what your price is located in the table, navigate to the column and review the two different billing options for that IOPS tier."}

### Desempenho
{: #pricing-comparison-performance}

| Opções de precificação para IOPS customizado | Cálculo de precificação |
|-----|-----|
| Preço Mensal | $0,10/GB + $0,07/IOP |
| Preço por hora | US$ 0,0001/GB + US$ 0,0002/IOP |
{: row-headers}
{: class="comparison-table"}
{: caption="Comparação de tabela" caption-side="top"}
{: summary="Table 3 is showing the prices for Performance Storage with monthly and hourly billing. This table has row and column headers. The row headers identify the billing options. To see what your cost for Storage is, navigate to the row of the billing option you are interested in."}
