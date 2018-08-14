---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# Solicitando o {{site.data.keyword.blockstorageshort}}

É possível provisionar o Armazenamento do {{site.data.keyword.blockstorageshort}} e ajustá-lo com precisão para atender às suas necessidades de capacidade e IOPS. Obtenha o máximo de seu armazenamento com duas opções para especificar desempenho.

- É possível escolher entre camadas de IOPs do Endurance que apresentam níveis de desempenho predefinidos para ajustar cargas de trabalho que não têm requisitos de desempenho bem definidos. 
- É possível ajustar com precisão seu armazenamento para atender a requisitos de desempenho muito específicos, especificando o número total de IOPS com o Performance.

## Solicitando  {{site.data.keyword.blockstorageshort}}  com Camadas IOPS predefinidas (Endurance)

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. Na parte superior direita, clique em  ** Pedir  {{site.data.keyword.blockstorageshort}} **.
3. Selecione seu **Local** de implementação (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts de cálculo que você tem.
4. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal. 
     1. Com o faturamento **por hora**, o número de horas que o LUN de bloco existiu na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento. Que já vem em primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento que é provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é proporcional desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um LUN de bloco é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).
        >**NOTA** - o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que **não** estão atualizados com recursos melhorados.
5. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
6. Selecione **Endurance (IOPS em camadas)** na seção **Opções de IOPS de armazenamento**.
7. Selecione a camada de IOPS de que seu aplicativo precisa.
    - **0,25 IOPS por GB** é projetado para cargas de trabalho com baixa intensidade de
E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos de cada vez. Aplicativos de exemplo incluem o armazenamento de caixas postais ou compartilhamentos de arquivo de nível departamental.
    - **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** é projetado para cargas de trabalho com maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos de cada vez. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis a desempenho.
    - **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html) para armazenamento provisionado até 4 TB.
8. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço complementa o seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
9. Escolha seu **Tipo de S.O.** na lista.
10. Marque as caixas de seleção de **Termos e condições** e clique em **Fazer pedido**.
11. Sua nova alocação de armazenamento estará disponível em alguns minutos.

>**Nota** - por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).<br/><br/>Para o limite de autorizações simultâneas, veja as [FAQs](BlockStorageFAQ.html).
 
## Solicitando  {{site.data.keyword.blockstorageshort}}  com IOPS Customizado (Desempenho)

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. Na parte superior direita, clique em  ** Pedir  {{site.data.keyword.blockstorageshort}} **.
3. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts de cálculo que você tem.
4. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas que o LUN de bloco existiu na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento. Que já vem em primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento que é provisionado nestes [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 
     2. Com o faturamento **mensal**, o cálculo para o preço é proporcional desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um LUN de bloco é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).
        >**NOTA** - o tipo de faturamento mensal é usado por padrão para o armazenamento provisionado em data centers que **não** estão atualizados com recursos melhorados.
5. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
6. Selecione **Performance (IOPS alocado)** na seção **Opções de IOPS de armazenamento**.
7. Insira o IOPS no campo **Performance (IOPS alocado)**.
8. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço complementa o seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
9. Escolha seu **Tipo de S.O.** na lista.
10. Sua nova alocação de armazenamento estará disponível em alguns minutos.

>**Nota** - por padrão, é possível provisionar um total combinado de 250 volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o representante de vendas. Leia sobre como aumentar os limites [aqui](managing-storage-limits.html).<br/><br/>Para o limite de autorizações simultâneas, veja as [FAQs](BlockStorageFAQ.html).

## Conectando seu novo armazenamento

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional do seu host, siga o link apropriado.
- [Conectando-se a LUNs iSCSI de MPIO no Linux](accessing_block_storage_linux.html)
- [Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows](accessing-block-storage-windows.html)
- [Configurando o Block Storage para backup com o cPanel](configure-backup-cpanel.html)
- [Configurando o Block Storage para backup com o Plesk](configure-backup-plesk.html)

## Identificando  {{site.data.keyword.blockstorageshort}}  em sua fatura

Todos os LUNs aparecem em sua fatura como um item de linha. O Endurance aparece como "Serviço de armazenamento do Endurance" e o Performance aparece como "Serviço de armazenamento do Performance". A taxa varia com base em seu nível de armazenamento. É possível expandir o Endurance ou o Performance para ver se ele é {{site.data.keyword.blockstorageshort}}.
