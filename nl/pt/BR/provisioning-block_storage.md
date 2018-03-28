---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Solicitando o {{site.data.keyword.blockstorageshort}}

Há dois tipos diferentes de armazenamento que podem ser provisionados com base em suas necessidades e
preferências. As duas opções de volumes do {{site.data.keyword.blockstorageshort}} são: 

- **Resistência**: camadas de Resistência de provisão com níveis
de desempenho predefinidos e recursos, como capturas instantâneas e replicação. 
- **Desempenho**: construa um ambiente Desempenho altamente poderoso no qual é
possível alocar as operações de entrada/saída por segundo (IOPS) desejadas.

## Como solicitar Resistência para o {{site.data.keyword.blockstorageshort}}

1. No
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window},
clique em **Armazenamento** >
**{{site.data.keyword.blockstorageshort}}** ou, no
Catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura >
Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. No canto superior direito, clique no link **Solicitar o
{{site.data.keyword.blockstorageshort}}**.
3. Selecione **Resistência** na lista suspensa Selecionar tipo de armazenamento.
4. Clique na lista suspensa e selecione seu **Local** de implementação (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts solicitados anteriormente.
   - Se você selecionar um data center com recursos melhorados (indicados com um * na lista suspensa), terá a opção de escolher entre Faturamento por hora ou mensal. 
     1. Com o faturamento **por hora**, o cálculo do número de horas que o LUN de bloco existia na conta é executado no momento em que o LUN é excluído ou no término do ciclo de faturamento, aquele que vier primeiro.  O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível apenas para armazenamento provisionado nesses [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é proporcional desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não haverá reembolso se um LUN de bloco for excluído antes do término do ciclo de faturamento.  Faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (mês ou mais).
     **NOTA**: o tipo de faturamento mensal é usado por padrão para armazenamento provisionado em data centers que **não** são atualizados com recursos melhorados.
5. Selecione o botão de opções ao lado da camada de IOPS necessária para seus aplicativos.
    - *0,25 IOPS por GB* é projetado para cargas de trabalho com baixa intensidade de E/S. Essas cargas de trabalho são tipicamente caracterizadas por terem uma alta porcentagem de dados inativos em um determinado momento. Aplicativos de exemplo incluem o armazenamento de caixas postais ou compartilhamentos de arquivo de nível departamental.
    - *2 IOPS por GB* é projetado para fins de uso geral. Aplicativos de exemplo incluem a hospedagem de pequenos bancos de dados que auxiliam os aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - *4 IOPS por GB* é projetado para cargas de trabalho com maior intensidade. Essas cargas de trabalho são tipicamente caracterizadas por terem uma alta porcentagem de dados ativos em um determinado momento. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis a desempenho.
    - *10 IOPS por GB* é projetado para as cargas de trabalho mais exigentes, como aquelas
criadas por bancos de dados NoSQL e processamento de dados para análise de dados.  Esta camada está disponível
em [selecionar data centers](new-ibm-block-and-file-storage-location-and-features.html) para
armazenamento provisionado de até 4 TB de tamanho.
6. Clique na lista suspensa **Selecionar tamanho de armazenamento** e selecione seu
tamanho de armazenamento.
7. Clique na lista suspensa **Especificar tamanho de espaço de captura instantânea**
e selecione o tamanho da captura instantânea (além de seu espaço utilizável). Para obter
considerações e recomendações sobre espaço de captura instantânea, leia
[Solicitando capturas instantâneas](ordering-snapshots.html).
8. Escolha seu **Tipo de S.O.** na lista suspensa.
9. Clique na lista suspensa e selecione o local de sua implementação (data center).
10. Clique em **Continuar**. Serão exibidos encargos mensais e rateados
com uma chance final para revisar os detalhes do pedido.
11. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal**
e clique no botão **Fazer pedido**.
12. A sua nova alocação de armazenamento deve estar disponível em alguns minutos.

**Nota**: por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).

Para o limite sobre autorizações simultâneas veja as nossas [FAQs](BlockStorageFAQ.html)
 
## Como solicitar o Desempenho para armazenamento de bloco

1. No
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window},
clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** ou, no Catálogo do
{{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento >
{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Solicitar o {{site.data.keyword.blockstorageshort}}** no
canto superior direito da tela.
3. Selecione **Desempenho** na lista suspensa **Selecionar tipo de
armazenamento**.
4. Clique na lista suspensa **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts solicitados anteriormente.
   - Se você selecionar um data center com recursos melhorados (indicados com um * na lista suspensa), terá a opção de escolher entre Faturamento por hora ou mensal. 
     1. Com o faturamento **por hora**, o cálculo do número de horas que o LUN de bloco existia na conta é executado no momento em que o LUN é excluído ou no término do ciclo de faturamento, aquele que vier primeiro.  O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível apenas para armazenamento provisionado nesses [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é proporcional desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não haverá reembolso se um LUN de bloco for excluído antes do término do ciclo de faturamento.  Faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (mês ou mais).
     **NOTA**: o tipo de faturamento mensal é usado por padrão para armazenamento provisionado em data centers que **não** são atualizados com recursos melhorados.
5. Selecione o botão de opções ao lado do **Tamanho de armazenamento** apropriado.
6. Insira o número de IOPS no campo **Especifique as IOPS**.
7. Clique em **Continuar**. Serão exibidos os encargos mensais e rateados
com uma chance final para revisar os detalhes do pedido. Clique em **Anterior** se você
desejar mudar seu pedido.
8. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços
Principal** e clique no botão **Fazer o pedido**.
9. A sua nova alocação de armazenamento deve estar disponível em alguns minutos.

**Nota**: por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o seu representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).

Para o limite sobre autorizações simultâneas veja as nossas [FAQs](BlockStorageFAQ.html)

## Como identificar o {{site.data.keyword.blockstorageshort}} na minha fatura

Todos os LUNs aparecerão em sua fatura como um item de linha; a Resistência aparecerá como
“Serviço de Armazenamento de Resistência” e o Desempenho aparecerá como “Serviço de Armazenamento de
Desempenho”. A taxa varia com base em seu nível de armazenamento. Em seguida, é possível
expandir na Resistência ou no Desempenho para ver que ele é {{site.data.keyword.blockstorageshort}}.
