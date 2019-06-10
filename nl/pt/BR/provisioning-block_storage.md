---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, ISCSI LUN, secondary storage, SLCLI, API, provisioning

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}


# Pedindo o {{site.data.keyword.blockstorageshort}} por meio do Console
{: #orderingthroughConsole}

É possível fornecer o {{site.data.keyword.blockstorageshort}} e fazer ajustes precisos para atender às suas necessidades de capacidade e de IOPS. Obtenha o máximo de seu armazenamento com duas opções para especificar desempenho.

- É possível escolher entre camadas de IOPs do Endurance que apresentam níveis de desempenho predefinidos para ajustar cargas de trabalho que não têm requisitos de desempenho bem definidos.
- É possível ajustar com precisão seu armazenamento para atender a requisitos de desempenho específicos, determinando o número total de IOPS com o Performance.

## Solicitando  {{site.data.keyword.blockstorageshort}}  com Camadas IOPS predefinidas (Endurance)

1. Efetue login no [catálogo do IBM Cloud](https://{DomainName}/catalog){: external} e clique em **Armazenamento**. Em seguida, selecione **{{site.data.keyword.blockstorageshort}}** e clique em **Criar**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} e clicar em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**. Na parte superior direita, clique em  ** Pedir  {{site.data.keyword.blockstorageshort}} **.

2. Selecione seu **Local** de implementação (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts de cálculo que você tem.
3. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas que o LUN de bloco existiu na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento. Que já vem em primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento que é provisionado nestes [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. Com o faturamento **mensal**, o cálculo para o preço é proporcional desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um LUN de bloco é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).

        O tipo de faturamento mensal é usado por padrão para o armazenamento fornecido em data centers que **não** são atualizados com os recursos aprimorados.
        {:important}
4. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
5. Selecione **Endurance (IOPS em camadas)** na seção **Opções de IOPS de armazenamento**.
6. Selecione a camada de IOPS de que seu aplicativo precisa.
    - **0,25 IOPS por GB** é projetado para cargas de trabalho com baixa intensidade de
E/S. Essas cargas de trabalho geralmente são caracterizadas por ter uma grande porcentagem de dados inativos de cada vez. Aplicativos de exemplo incluem o armazenamento de caixas postais ou compartilhamentos de arquivo de nível departamental.
    - **2 IOPS por GB** é projetado para uso de propósito geral. Os aplicativos de exemplo incluem a hospedagem de bancos de dados pequenos que estão suportando aplicativos da web ou imagens de disco de máquina virtual para um hypervisor.
    - **4 IOPS por GB** é projetado para cargas de trabalho com maior intensidade. Essas cargas de trabalho geralmente são caracterizadas por ter uma alta porcentagem de dados ativos de cada vez. Aplicativos de exemplo incluem bancos de dados transacionais e outros sensíveis a desempenho.
    - **10 IOPS por GB** é projetado para as cargas de trabalho mais exigentes, como aquelas criadas por bancos de dados NoSQL, e para processamento de dados para Analytics. Essa camada está disponível em [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news) para armazenamento provisionado até 4 TB.
7. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço complementa o seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Escolha seu **Tipo de S.O.** na lista.<br/>

   Essa seleção é baseada no sistema operacional no qual seu host está em execução e não pode ser modificada posteriormente. Por exemplo, se o servidor é Ubuntu ou RHEL, selecione Linux. Se o host for um servidor Windows 2012 ou
Windows 2016, selecione a opção Windows 2008+ na lista. Para obter mais informações sobre as várias opções do Windows,
consulte as [Perguntas frequentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
   {:tip}
9. À direita, revise o resumo do pedido e aplique o código promocional, se tiver um.

   Os descontos são aplicados quando o pedido é processado.
   {:note}
10. Após revisar os termos e as condições, marque a caixa **Eu li e concordo com os
Contratos de Prestação de Serviços de Terceiro**.
11. Clique em **Criar**. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o representante de vendas. Leia sobre o aumento dos limites [aqui](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Para o limite de autorizações simultâneas, consulte as [Perguntas mais frequentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}

## Solicitando  {{site.data.keyword.blockstorageshort}}  com IOPS Customizado (Desempenho)

1. Efetue login no [catálogo do IBM Cloud](https://{DomainName}/catalog){: external} e clique em **Armazenamento**. Em seguida, selecione {{site.data.keyword.blockstorageshort}} e clique em **Criar**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external} e clicar em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**. Na parte superior direita, clique em  ** Pedir  {{site.data.keyword.blockstorageshort}} **.
2. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts de cálculo que você tem.
3. Faturamento. Se você selecionou um data center com recursos melhorados (marcados com um asterisco), é possível escolher entre Faturamento por hora ou mensal.
     1. Com o faturamento **por hora**, o número de horas que o LUN de bloco existiu na conta é calculado no momento em que o LUN é excluído ou no término do ciclo de faturamento. Que já vem em primeiro. O faturamento por hora é uma boa opção para armazenamento usado por alguns dias ou menos de um mês completo. O faturamento por hora está disponível somente para armazenamento que é provisionado nestes [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).
     2. Com o faturamento **mensal**, o cálculo para o preço é proporcional desde a data de criação até o término do ciclo de faturamento e faturado imediatamente. Não há reembolso se um LUN de bloco é excluído antes do término do ciclo de faturamento. O faturamento mensal é uma boa opção para armazenamento usado em cargas de trabalho de produção que usam dados que precisam ser armazenados e acessados por longos períodos de tempo (um mês ou mais).

        O tipo de faturamento mensal é usado por padrão para o armazenamento fornecido em data centers que **não** são atualizados com os recursos aprimorados.
        {:note}
4. Insira seu tamanho de armazenamento no campo **Novo tamanho de armazenamento**.
5. Selecione **Performance (IOPS alocado)** na seção **Opções de IOPS de armazenamento**.
6. Insira o IOPS no campo **Performance (IOPS alocado)**.
7. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço complementa o seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
8. Escolha seu **Tipo de S.O.** na lista.<br/>

   Essa seleção é baseada no sistema operacional no qual seu host está em execução e não pode ser modificada posteriormente. Por exemplo, se o servidor é Ubuntu ou RHEL, selecione Linux. Se o host for um servidor Windows 2012 ou
Windows 2016, selecione a opção Windows 2008+ na lista. Para obter mais informações sobre as várias opções do Windows,
consulte as [Perguntas frequentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
   {:tip}
9. À direita, revise o resumo do pedido e aplique o código promocional, se tiver um.

   Os descontos são aplicados quando o pedido é processado.
   {:note}
10. Após revisar os termos e as condições, marque a caixa **Eu li e concordo com os
Contratos de Prestação de Serviços de Terceiro**.
11. Clique em **Criar**. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Por padrão, é possível provisionar um total combinado de 250
volumes do {{site.data.keyword.blockstorageshort}}. Para aumentar o número de seus volumes, entre em contato com o representante de vendas. Leia sobre o aumento dos limites [aqui](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits).<br/><br/>Para o limite de autorizações simultâneas, consulte as [Perguntas mais frequentes](/docs/infrastructure/BlockStorage?topic=block-storage-faqs).
{:important}

## Conectando seu novo armazenamento
{: #mountingnewLUN}

Quando sua solicitação de fornecimento estiver concluída, autorize seus hosts a acessar o novo armazenamento e configurar sua conexão. Dependendo do sistema operacional do seu host, siga o link apropriado.
- [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [Montando
um LUN no armazenamento compartilhado do XenServer](/docs/infrastructure/virtualization?topic=Virtualization-setting-up-and-mounting-an-iscsi-node-in-xenserver-shared-storage)
- [Configurando o Block Storage para backup com cPanel](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Configurando o Block Storage para backup com Plesk](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## Considerações sobre recuperação de desastre

Para evitar a perda de dados e assegurar a continuidade de negócios, considere replicar os servidores e armazená-los em outro data center. A replicação mantém seus dados em sincronização em duas localizações diferentes com base em seu planejamento de captura instantânea. Para obter mais informações, consulte [Replicando dados](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).

Se desejar clonar seu volume e utilizá-lo independentemente do volume original, consulte [Criando um volume de bloco duplicado](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume).

## Identificando  {{site.data.keyword.blockstorageshort}}  em sua fatura

Todos os LUNs aparecem em sua fatura como um item de linha. O Endurance aparece como "Serviço de armazenamento do Endurance" e o Performance aparece como "Serviço de armazenamento do Performance". A taxa varia com base em seu nível de armazenamento. É possível expandir o Endurance ou o Performance para ver se ele é {{site.data.keyword.blockstorageshort}}.
