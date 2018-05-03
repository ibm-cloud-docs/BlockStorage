---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Criando um volume de bloco duplicado

O {{site.data.keyword.BluSoftlayer_full}} fornece a capacidade de criar uma duplicata de um
{{site.data.keyword.blockstoragefull}} existente. O volume duplicado herdará as opções de capacidade e
de desempenho do LUN/volume original por padrão e terá uma cópia dos dados até o momento de uma captura
instantânea.   

Como a duplicata é baseada nos dados em uma captura instantânea de momento, um espaço de captura
instantânea é necessário no volume original para poder criar uma duplicata.  Para saber mais sobre capturas
instantâneas e como solicitar espaço de captura instantânea, consulte [Documentação
de captura instantânea](snapshots.html).

Duplicatas podem ser criadas por meio de volumes primários e de réplica, em que as novas
duplicatas são criadas no mesmo data center do volume original.  Por exemplo, se você criar uma duplicata por
meio de um volume de réplica, o novo volume será criado no mesmo data center do volume de réplica.    

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento
é provisionado.  As capturas instantâneas e a replicação não serão permitidas até que a cópia de dados do
original para a duplicata seja concluída. 

Quando a cópia de dados é concluída, a duplicata pode ser gerenciada e usada como um volume
completamente independente do original. 

Esse recurso está disponível somente para armazenamento que é provisionado com criptografia. Clique
[aqui](new-ibm-block-and-file-storage-location-and-features.html) para obter uma lista de
data centers disponíveis. 

Alguns usos comuns para um volume duplicado:
- **Teste de recuperação de desastre**: crie
uma duplicata de seu volume de réplica para verificar se os dados estão intactos e se podem ser usados no
evento de um desastre, sem interrupção da replicação. 
- **Cópia de ouro**: use um volume de armazenamento como uma cópia de ouro da qual é
possível criar múltiplas instâncias para vários usos. 
- **Atualização de dados**: crie uma cópia de seus dados de produção a ser montada em
seu ambiente de não produção para teste. 
- **Restaurar por meio da captura instantânea**: restaure dados no volume original
com arquivos/data específicos por meio de uma captura instantânea sem sobrescrever o volume original
inteiro com uma função de restauração de captura instantânea. 
- **Desenvolvimento/teste**: crie até 4 duplicatas simultâneas de um volume por
vez para criar volumes com dados duplicados para desenvolvimento e teste. 
- **Redimensionamento de armazenamento**: crie um novo volume com o
novo tamanho e/ou IOPS sem precisar executar uma migração baseada em host de seus dados.  
	

Existem algumas maneiras de criar um volume duplicado por meio do
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}: 

## Como criar uma duplicata de um volume específico na lista de armazenamentos

Navegue para a sua lista de {{site.data.keyword.blockstorageshort}}:

No portal do cliente, clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** ou, no
Catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura->
Armazenamento->{{site.data.keyword.blockstorageshort}}**. 


1. Selecione um LUN na lista e clique em **Ações** -> **Duplicar LUN
(volume)** 
2. Escolha sua opção de captura instantânea: 
    - Se estiver solicitando um volume não de réplica:
      - Selecione **Criar de uma nova captura instantânea** - isso criará uma nova
captura instantânea que será usada para a duplicata. Use esta opção quando atualmente não há nenhuma captura
instantânea para o volume ou se você deseja criar uma duplicata nesse momento.
    
      OR 
      - Selecione **Criar da captura instantânea mais recente** - isso
criará uma duplicata de qualquer captura instantânea mais recente que existe para este volume. 
    - Se estiver solicitando de um volume de réplica - a única opção para a captura instantânea é usar a
captura instantânea mais recente disponível. 
3. O tipo de armazenamento (Resistência ou Desempenho) e Local permanecerão os mesmos que o volume original.
4. Faturamento por hora ou mensal – é possível escolher provisionar o novo LUN duplicado com
faturamento por hora ou mensal.  O tipo de faturamento para o volume original é selecionado automaticamente,
mas se você deseja escolher um tipo de faturamento diferente para seu novo armazenamento duplicado, é
possível fazer essa seleção aqui. 
5. Se desejar, é possível especificar IOPS ou Camada de IOPS para o novo volume. A designação de IOPs do volume original é configurada por padrão. 
    - Se o seu volume original for a Camada de resistência de IOPS 0,25, você não poderá fazer uma nova seleção. 
    - Se o seu volume original for a Camada de resistência de IOPS 2, 4 ou 10, será possível se mover em qualquer lugar entre essas camadas para o novo volume. 
    - Desempenho disponível e combinações de tamanho serão exibidos. 
6. Se desejar, é possível atualizar o tamanho do novo volume para que seja maior que o original.  O tamanho do volume original é configurado por padrão. 
    - **Nota**: o {{site.data.keyword.blockstorageshort}} só pode ser redimensionado para 10 vezes o tamanho original do volume. 
7. Se desejar, é possível atualizar o espaço de captura instantânea para o novo volume para incluir mais,
menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original será configurado por padrão. 
8. Clique em **Continuar** para colocar a sua ordem para a duplicata. 



## Como criar uma duplicata de uma captura instantânea específica

Navegue para a sua lista de {{site.data.keyword.blockstorageshort}}:

1. Clique em um **LUN/volume** na lista para visualizar a página de detalhes. (Ele
pode ser um volume de réplica ou não de réplica). 
2. Role para baixo e selecione uma captura instantânea existente na lista na página de detalhes e
clique em **Ações ->Duplicar**.   
3. O tipo de armazenamento (Resistência ou Desempenho) e Local permanecerão os mesmos que o volume original. 
4. Se desejar, é possível especificar IOPS ou Camada de IOPS para o novo volume. A designação de IOPs do volume original é configurada por padrão. 
    - Se o seu volume original for a Camada de resistência de IOPS 0,25, você não poderá fazer uma nova seleção. 
    - Se o seu volume original for a Camada de resistência de IOPS 2, 4 ou 10, será possível se mover em qualquer lugar entre essas camadas para o novo volume. 
    - Desempenho disponível e combinações de tamanho serão exibidos. 
5. Se desejar, é possível atualizar o tamanho do novo volume para que seja maior que o original.  O tamanho do volume original é configurado por padrão. 
    - **Nota**: o {{site.data.keyword.blockstorageshort}} só pode ser redimensionado para 10 vezes o tamanho original do volume. 
6. Se desejar, é possível atualizar o espaço de captura instantânea para o novo volume para incluir
mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original será configurado por padrão. 
7. Clique em **Continuar** para colocar a sua ordem para a duplicata. 


## Como gerenciar seu volume duplicado

Enquanto os dados são copiados do volume original para a duplicata, é exibido um status na
página de detalhes indicando que a duplicação está em andamento. Durante esse tempo, é possível conectar-se a um
host e executar leitura/gravação no volume, mas não é possível criar planejamentos de captura instantânea. Quando o
processo de duplicação for concluído, o novo volume será completamente independente do volume original e
poderá ser gerenciado com capturas instantâneas e replicação, etc., normalmente. 
