---

copyright:
  years: 2014, 2017
lastupdated: "2017-10-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Criando um volume de bloco duplicado

O {{site.data.keyword.BluSoftlayer_full}} fornece a capacidade de criar uma duplicata de um
{{site.data.keyword.blockstoragefull}} existente. O volume duplicado herda as opções de capacidade e de desempenho do LUN/volume original por padrão e tem uma cópia dos dados até o momento de uma captura instantânea.   

Como a duplicata é baseada nos dados em uma captura instantânea de um momento, o espaço de captura instantânea é necessário no volume original antes de poder criar uma duplicata.  Para saber mais sobre capturas
instantâneas e como solicitar espaço de captura instantânea, consulte [Documentação
de captura instantânea](snapshots.html).

As duplicatas podem ser criadas por meio dos volumes primário e de réplica. A nova duplicata é criada no mesmo data center que o volume original.  Por exemplo, se você criar uma duplicata por
meio de um volume de réplica, o novo volume será criado no mesmo data center do volume de réplica.    

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento
é provisionado. No entanto, as capturas instantâneas e a replicação não serão permitidas até que a cópia de dados do original para a duplicata esteja concluída. 

Quando a cópia de dados é concluída, a duplicata pode ser gerenciada e usada como um volume completamente independente. 

Esse recurso está disponível somente para armazenamento que é provisionado com criptografia. Clique [aqui](new-ibm-block-and-file-storage-location-and-features.html) para obter a lista de data centers disponíveis. 

Alguns usos comuns para um volume duplicado:
- **Teste de recuperação de desastre**: crie uma duplicata de seu volume de réplica para verificar se os dados estão intactos e poderão ser usados se um desastre ocorrer, sem interromper a replicação. 
- **Cópia de ouro**: use um volume de armazenamento como uma cópia de ouro da qual é
possível criar múltiplas instâncias para vários usos. 
- **Atualização de dados**: crie uma cópia de seus dados de produção a ser montada em
seu ambiente de não produção para teste. 
- **Restaurar da captura instantânea**: restaure dados no volume original com arquivos/data específicos de uma captura instantânea sem sobrescrever o volume original inteiro com a função de restauração de captura instantânea. 
- **Des./teste**: crie até quatro duplicatas simultâneas de um volume por vez para criar dados duplicados para desenvolvimento e teste. 
- **Redimensionamento de armazenamento**: crie um volume com o novo tamanho, a taxa de IOPS ou ambos sem precisar executar uma migração baseada em host de seus dados.  
	

Existem algumas maneiras de criar um volume duplicado por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}: 

## Como criar uma duplicata de um volume específico na lista de armazenamentos

Acesse sua lista de {{site.data.keyword.blockstorageshort}}:

1. No portal do cliente, clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura->Armazenamento->{{site.data.keyword.blockstorageshort}}**. 
2. Selecione um LUN na lista e clique em **Ações** -> **Duplicar LUN
(volume)** 
3. Escolha sua opção de captura instantânea: 
    - Se estiver solicitando um volume não de réplica:
      - Selecione **Criar por meio da nova captura instantânea**, isso criará uma captura instantânea que será usada para a duplicata. Use essa opção se atualmente não houver nenhuma captura instantânea para seu volume ou se você desejar criar uma duplicata nesse momento.
    
      OR 
      - Selecione **Criar por meio da captura instantânea mais recente** - isso criará uma duplicata da captura instantânea mais recente que existe para esse volume. 
    - Se estiver solicitando de um volume de réplica - a única opção para a captura instantânea é usar a
captura instantânea mais recente disponível. 
4. O tipo de armazenamento (Resistência ou Desempenho) e Local permanecerão os mesmos que o volume original.
5. Faturamento por hora ou mensal – é possível escolher provisionar o LUN duplicado com faturamento por hora ou mensal.  O tipo de faturamento para o volume original é selecionado automaticamente. Se você deseja escolher um tipo de faturamento diferente para seu armazenamento duplicado, é possível fazer essa seleção aqui. 
5. É possível especificar o IOPS ou a Camada de IOPS para o novo volume, caso deseje. A designação de IOPS do volume original é configurada por padrão. Desempenho disponível e combinações de tamanho serão exibidos.
    - Se seu volume original for a camada de 0,25 IOPS do Endurance, não será possível fazer uma nova seleção. 
    - Se seu volume original for a camada de 2, 4 ou 10 IOPR do Endurance, será possível se mover em qualquer lugar entre essas camadas para o novo volume. 
6. É possível atualizar o tamanho do novo volume para que seja maior que o original. O tamanho do volume original é configurado por padrão. 
    - **Nota**: o {{site.data.keyword.blockstorageshort}} pode ser redimensionado somente para 10 vezes o tamanho original do volume. 
7. É possível atualizar o espaço de captura instantânea do novo volume para incluir mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original é configurado por padrão. 
8. Clique em **Continuar** para fazer seu pedido. 



## Como criar duplicata de uma captura instantânea específica

Navegue para a sua lista de {{site.data.keyword.blockstorageshort}}:

1. Clique em um **LUN/volume** na lista para visualizar a página de detalhes. (Ele
pode ser um volume de réplica ou não de réplica). 
2. Role para baixo e selecione uma captura instantânea existente na lista na página de detalhes e
clique em **Ações ->Duplicar**.   
3. O tipo de armazenamento (Resistência ou Desempenho) e Local permanecerão os mesmos que o volume original. 
4. As combinações de desempenho e tamanho disponíveis são exibidas. A designação de IOPs do volume original é configurada por padrão. É possível especificar o IOPS ou a Camada de IOPS para o novo volume. 
    - Se seu volume original for a camada de 0,25 IOPS do Endurance, não será possível fazer uma nova seleção. 
    - Se seu volume original for a camada de 2, 4 ou 10 IOPS do Endurance, será possível se mover em qualquer lugar entre essas camadas para o novo volume. 
5. É possível atualizar o tamanho do novo volume para que seja maior que o original. O tamanho do volume original é configurado por padrão. 
    - **Nota**: o {{site.data.keyword.blockstorageshort}} pode ser redimensionado somente para 10 vezes o tamanho original do volume. 
6. É possível atualizar o espaço de captura instantânea do novo volume para incluir mais, menos ou nenhum espaço de captura instantânea. O espaço de captura instantânea do volume original é configurado por padrão. 
7. Clique em **Continuar** para colocar a sua ordem para a duplicata. 


## Como gerenciar seu volume duplicado

Enquanto os dados estiverem sendo copiados do volume original para a duplicata, você verá um status na página de detalhes que indica que a duplicação está em andamento. Durante esse tempo, é possível conectar-se a um host e ler/gravar no volume, mas não é possível criar planejamentos de captura instantânea. Quando o processo de duplicação for concluído, o novo volume será completamente independente e poderá ser gerenciado com capturas instantâneas e replicação normalmente. 
