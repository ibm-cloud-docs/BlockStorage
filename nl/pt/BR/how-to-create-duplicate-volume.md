---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Criando um volume de bloco duplicado

É possível criar uma duplicata de um {{site.data.keyword.blockstoragefull}} existente. O volume duplicado herda as opções de capacidade e desempenho do volume original por padrão e tem uma cópia dos dados até o momento de uma captura instantânea.   

Como a duplicata é baseada nos dados em uma captura instantânea de um momento, o espaço de captura instantânea é necessário no volume original antes de poder criar uma duplicata. Para obter mais informações sobre capturas instantâneas e como pedir espaço de captura instantânea, consulte a [documentação da Captura instantânea](snapshots.html).  

As duplicatas podem ser criadas de ambos os volumes, o **primário** e o de **réplica**. A nova duplicata é criada no mesmo data center que o volume original. Se você criar uma duplicata de um volume de réplica, o novo volume será criado no mesmo data center que o volume de réplica.

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento
é provisionado. No entanto, capturas instantâneas e replicação não são permitidas até que a cópia de dados do original para a duplicata seja concluída.

Quando a cópia de dados é concluída, a duplicata pode ser gerenciada e usada como um volume completamente independente.

Esse recurso está disponível na maioria dos locais. Clique [aqui](new-ibm-block-and-file-storage-location-and-features.html) para obter a lista de data centers disponíveis.

Se você for um usuário da conta dedicado do {{site.data.keyword.containerlong}}, consulte as opções para duplicar um volume na documentação do [ {{site.data.keyword.containerlong_notm}}](/docs/containers/cs_storage_file.html#backup_restore).
{:tip}

Alguns usos comuns para um volume duplicado:
- **Teste de recuperação de desastre**. Crie uma duplicata de seu volume de réplica para verificar se os dados estão intactos e podem ser usados se ocorrer um desastre, sem interromper a replicação.
- **Cópia de ouro**. Use um volume de armazenamento como uma cópia de ouro da qual é possível criar múltiplas instâncias para vários usos.
- **Atualizações de dados**. Crie uma cópia de seus dados de produção para montar em seu ambiente de não produção para teste.
- **Restaurar da captura instantânea**. Restaure dados no volume original com arquivos e data específicos por meio de uma captura instantânea sem sobrescrever o volume original inteiro com a função de restauração de captura instantânea.
- **Desenvolvimento e teste (dev/test)**. Crie até quatro duplicatas simultâneas de um volume por vez para criar dados duplicados para desenvolvimento e teste.
- **Redimensionamento de armazenamento**. Crie um volume com novo tamanho, taxa de IOPS, ou ambos, sem a necessidade de mover seus dados.  

É possível criar um volume duplicado por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window} de algumas maneiras.


## Criando uma duplicata de um volume específico na Lista de armazenamento

1. Vá para a sua lista de  {{site.data.keyword.blockstorageshort}}
    - No portal do cliente, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU
    - Por meio do console do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione um volume na lista e clique em **Ações** > **Duplicar LUN (volume)**
3. Escolha sua opção de captura instantânea:
    - Se você pedir por meio de um volume que **não é de réplica**,
      - Selecione **Criar de uma nova captura instantânea** - essa ação cria uma captura instantânea a ser usada para a duplicata. Use essa opção se o seu volume não tiver capturas instantâneas atuais ou se você desejar criar uma duplicata logo em seguida.<br/>
      - Selecione **Criar da captura instantânea mais recente** - essa ação cria uma duplicata da captura instantânea mais recente existente para esse volume.
    - Se você pedir usando um volume de **réplica**, a única opção para a captura instantânea será usar a captura instantânea mais recente disponível.
4. O Tipo de armazenamento e o Local permanecem iguais aos do volume original.
5. Faturamento por hora ou mensal – é possível escolher provisionar o LUN duplicado com faturamento por hora ou mensal. O tipo de faturamento para o volume original é selecionado automaticamente. Se você deseja escolher um tipo de faturamento diferente para seu armazenamento duplicado, é possível fazer essa seleção aqui.
5. É possível especificar o IOPS ou a Camada de IOPS para o novo volume, caso deseje. A designação de IOPS do volume original é configurada por padrão. As combinações de desempenho e tamanho disponíveis são exibidas.
    - Se o seu volume original for a camada 0,25 IOPS Endurance, não será possível fazer uma nova seleção.
    - Se seu volume original for a camada de 2, 4 ou 10 IOPR do Endurance, será possível se mover em qualquer lugar entre essas camadas para o novo volume.
6. É possível atualizar o tamanho do novo volume para que seja maior que o do original. O tamanho do volume original é configurado por padrão.

   O {{site.data.keyword.blockstorageshort}} pode ser redimensionado para 10 vezes o tamanho original do
volume.
   {:tip}
7. É possível atualizar o espaço de captura instantânea do novo volume para incluir mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original é configurado por padrão.
8. Clique em **Continuar** para fazer seu pedido.



## Criando uma duplicata de uma Captura instantânea específica

1. Vá para a sua lista de  {{site.data.keyword.blockstorageshort}}
2. Clique em um LUN na lista para visualizar a página de detalhes. (Ele
pode ser um volume de réplica ou não de réplica).
3. Role para baixo e selecione uma captura instantânea existente na lista na página de detalhes e clique em **Ações** > **Duplicar**.   
4. O Tipo de armazenamento (Endurance ou Performance) e o Local permanecem iguais aos do volume original.
5. As combinações de desempenho e tamanho disponíveis são exibidas. A designação de IOPs do volume original é configurada por padrão. É possível especificar o IOPS ou a Camada de IOPS para o novo volume.
    - Se o seu volume original for a camada 0,25 IOPS Endurance, não será possível fazer uma nova seleção.
    - Se seu volume original for a camada de 2, 4 ou 10 IOPS do Endurance, será possível se mover em qualquer lugar entre essas camadas para o novo volume.
6. É possível atualizar o tamanho do novo volume para que seja maior que o original. O tamanho do volume original é configurado por padrão.

   O {{site.data.keyword.blockstorageshort}} pode ser redimensionado para 10 vezes o tamanho original do
volume.
   {:tip}
7. É possível atualizar o espaço de captura instantânea do novo volume para incluir mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original é configurado por padrão.
8. Clique em **Continuar** para colocar a sua ordem para a duplicata.


## Gerenciando seu volume duplicado

Enquanto os dados estão sendo copiados do volume original para a duplicata, é possível ver um status na página de detalhes mostrando que a duplicação está em andamento. Durante esse tempo, é possível conectar-se a um host e ler/gravar no volume, mas não é possível criar planejamentos de captura instantânea. Quando o processo de duplicação é concluído, o novo volume fica independente do original e pode ser gerenciado com capturas instantâneas e replicação normalmente.
