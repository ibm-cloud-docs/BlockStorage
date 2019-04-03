---

copyright:
  years: 2017, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, LUN, volume duplication,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Criando um volume de bloco duplicado
{: #duplicatevolume}

É possível criar uma duplicata de um {{site.data.keyword.blockstoragefull}} existente. O volume duplicado herda as opções de capacidade e desempenho do volume original por padrão e tem uma cópia dos dados até o momento de uma captura instantânea.   

Como a duplicata é baseada nos dados em uma captura instantânea de um momento, o espaço de captura instantânea é necessário no volume original antes de poder criar uma duplicata. Para obter mais informações sobre capturas instantâneas e como pedir espaço de captura instantânea, consulte a [documentação da Captura instantânea](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots).  

As duplicatas podem ser criadas de ambos os volumes, o **primário** e o de **réplica**. A nova duplicata é criada no mesmo data center que o volume original. Se você criar uma duplicata de um volume de réplica, o novo volume será criado no mesmo data center que o volume de réplica.

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento
é provisionado. No entanto, capturas instantâneas e replicação não são permitidas até que a cópia de dados do original para a duplicata seja concluída.

Quando a cópia de dados for concluída, a duplicata poderá ser gerenciada e usada como um volume independente.

Esse recurso está disponível na maioria dos locais. Clique [aqui](/docs/infrastructure/BlockStorage?topic=BlockStorage-news) para obter a lista de data centers disponíveis.

Se você for um usuário da conta Dedicada do {{site.data.keyword.containerlong}}, consulte suas opções para duplicar um volume na [{{site.data.keyword.containerlong_notm}}documentação](/docs/containers?topic=containers-block_storage#block_backup_restore).
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

1. Acesse a sua lista de {{site.data.keyword.blockstorageshort}}.
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


## Criando uma duplicata por meio da CLI do SL
É possível usar o comando a seguir na CLI do SL para criar um volume {{site.data.keyword.blockstorageshort}}
duplicado.

```
# slcli block volume-duplicate --help
Usage: slcli block volume-duplicate [OPTIONS] ORIGIN_VOLUME_ID

Options:
  -o, --origin-snapshot-id INTEGER
                                  ID of an origin volume snapshot to use for
                                  duplcation.
  -c, --duplicate-size INTEGER    Size of duplicate block volume in GB. ***If
                                  no size is specified, the size of the origin
                                  volume will be used.***
                                  Potential Sizes:
                                  [20, 40, 80, 100, 250, 500, 1000, 2000,
                                  4000, 8000, 12000] Minimum: [the size of the
                                  origin volume]
  -i, --duplicate-iops INTEGER    Performance Storage IOPS, between 100 and
                                  6000 in multiples of 100 [only used for
                                  performance volumes] ***If no IOPS value is
                                  specified, the IOPS value of the origin
                                  volume will be used.***
                                  Requirements: [If
                                  IOPS/GB for the origin volume is less than
                                  0.3, IOPS/GB for the duplicate must also be
                                  less than 0.3. If IOPS/GB for the origin
                                  volume is greater than or equal to 0.3,
                                  IOPS/GB for the duplicate must also be
                                  greater than or equal to 0.3.]
  -t, --duplicate-tier [0.25|2|4|10]
                                  Endurance Storage Tier (IOPS per GB) [only
                                  used for endurance volumes] ***If no tier is
                                  specified, the tier of the origin volume
                                  will be used.***
                                  Requirements: [If IOPS/GB
                                  for the origin volume is 0.25, IOPS/GB for
                                  the duplicate must also be 0.25. If IOPS/GB
                                  for the origin volume is greater than 0.25,
                                  IOPS/GB for the duplicate must also be
                                  greater than 0.25.]
  -s, --duplicate-snapshot-size INTEGER
                                  The size of snapshot space to order for the
                                  duplicate. ***If no snapshot space size is
                                  specified, the snapshot space size of the
                                  origin block volume will be used.***
                                  Input
                                  "0" for this parameter to order a duplicate
                                  volume with no snapshot space.
  --billing [hourly|monthly]      Optional parameter for Billing rate (default
                                  to monthly)
  -h, --help                      Show this message and exit.
```
{:codeblock}

## Gerenciando seu volume duplicado

Enquanto os dados estão sendo copiados do volume original para a duplicata, é possível ver um status na página de detalhes mostrando que a duplicação está em andamento. Durante esse tempo, é possível conectar-se a um host e ler e gravar no volume, mas não é possível criar planejamentos de captura instantânea. Quando o processo de duplicação é concluído, o novo volume fica independente do original e pode ser gerenciado com capturas instantâneas e replicação normalmente.
