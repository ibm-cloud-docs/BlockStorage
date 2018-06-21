---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gerenciando capturas instantâneas

## Como criar um planejamento de captura instantânea?

Você decide com que frequência e quando deseja criar uma referência de momento de seu volume de armazenamento com planejamentos de captura instantânea. É possível ter um máximo de 50 capturas
instantâneas por volume de armazenamento. Os planejamentos são gerenciados por meio de **Armazenamento**, guia **{{site.data.keyword.blockstorageshort}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Para poder configurar seu planejamento inicial, deve-se primeiramente comprar um espaço de captura instantânea, caso você não tenha comprado durante o fornecimento inicial do volume de armazenamento.

### Como incluir um planejamento de captura instantânea

Os planejamentos de capturas instantâneas podem ser configurados em intervalos, como por hora,
diários e semanais, cada um com um ciclo de retenção diferente. Há um máximo de 50 capturas
instantâneas planejadas, podendo ser uma combinação de planejamentos por hora, diários e
semanais e capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento, clique em **Ações** e clique em **Planejar captura instantânea**.
2. No diálogo Novo planejamento de captura instantânea, há três frequências de captura
instantânea diferentes para seleção. Use qualquer combinação das três frequências para criar um planejamento de captura
instantânea abrangente.
   - Hora em Hora
      - Especifique o minuto de cada hora em que uma captura instantânea deve ser executada; o padrão é
o minuto atual.
      - Especifique o número de capturas instantâneas por hora a serem retidas antes de
descartar a mais antiga.
   - Diário
      - Especifique a hora e o minuto em que uma captura instantânea deve ser executada; o padrão é a
hora e o minuto atuais.
      - Selecione o número de capturas instantâneas por hora a serem retidas antes de
descartar a mais antiga.
   - Semanal
      - Especifique o dia da semana, a hora e o minuto em que uma captura instantânea deve ser executada;
o padrão é o dia, a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas semanais a serem retidas antes de descartar a
mais antiga.
3. Clique em **Salvar** e crie outro planejamento com uma frequência diferente. Se o número total de capturas instantâneas planejadas for mais de 50, você receberá uma mensagem de aviso e não conseguirá salvar.

A lista de capturas instantâneas é exibida conforme elas são tomadas na seção **Capturas instantâneas** da página **Detalhe**.

## Como tomar a captura instantânea manual?

Capturas instantâneas manuais podem ser obtidas em vários pontos durante um upgrade ou
manutenção do aplicativo. Também é possível tomar capturas instantâneas em múltiplas máquinas que tenham sido desativadas temporariamente no nível do aplicativo.

Há um máximo de 50 capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento.
2. Clique em ** Ações**.
3. Clique em **Executar captura instantânea manual**.
A captura instantânea é tomada e exibida na seção **Capturas instantâneas** da página **Detalhe**. O
planejamento será Manual.

## Como ver uma lista de capturas instantâneas com espaço usado e funções de gerenciamento?

Uma lista de capturas instantâneas retidas e do espaço consumido pode ser vista na
página **Detalhes** (Armazenamento, {{site.data.keyword.blockstorageshort}}). As funções de gerenciamento (edição de planejamentos e inclusão de mais espaço) são realizadas na página Detalhe usando o menu **Ações** ou os links nas várias seções na página.

## Como visualizar uma lista de capturas instantâneas retidas?

As capturas instantâneas retidas são baseadas no número que você inseriu no campo **Manter o último** ao configurar seus planejamentos. É possível visualizar as capturas instantâneas que foram tomadas sob a seção **Captura instantânea**. As capturas instantâneas são listadas por planejamento.

## Como ver quanto espaço de captura instantânea foi usado?

O gráfico de pizza na parte superior da página **Detalhes** exibe quanto espaço foi usado e quanto espaço resta. Você receberá notificações quando atingir os limites de espaço de 75 por cento, 90 por cento e 95 por cento.

## Como mudar a quantia de espaço de captura instantânea para meu volume?

Você pode precisar incluir espaço de captura instantânea em um volume que não tinha nenhum anteriormente ou pode requerer espaço de captura instantânea extra. É possível incluir 5 GB - 4.000 GB, dependendo de suas necessidades. 

**Nota**: o espaço de captura instantânea pode ser somente aumentado, não reduzido. É possível selecionar uma quantia menor de espaço até determinar a quantia de espaço que você realmente
precisa. Lembre-se de que as capturas instantâneas automatizadas e manuais compartilham o espaço.

O espaço de captura instantânea é mudado por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique em seus volumes de armazenamento, clique em **Ações** e clique em **Incluir mais espaço de captura instantânea**.
2. Selecione em um intervalo de tamanhos no prompt. Os tamanhos geralmente variam de 0 até o tamanho
de seu volume.
3. Clique em **Continuar**.
4. Insira qualquer Código promocional que tenha e clique em **Recalcular**. Os campos Encargos para este pedido e Revisão do pedido são concluídos por padrão.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…**
e clique em **Fazer o pedido**. Seu espaço de captura instantânea adicional será provisionado em alguns minutos.

## Como receber notificações quando estou próximo de meu limite de espaço de captura instantânea e capturas instantâneas são excluídas?

As notificações são enviadas por meio dos chamados de suporte para o Usuário principal em sua conta quando você atinge três limites de espaço diferentes, 75 por cento, 90 por cento e 95 por cento.

- **Capacidade de 75 por cento**: um aviso é enviado que a utilização de espaço de captura instantânea excedeu 75 por cento. Se você seguir o aviso e incluir espaço manualmente ou excluir capturas instantâneas retidas e desnecessárias, a ação será anotada e o chamado será encerrado. Se você não fizer nada, deverá confirmar manualmente o chamado e, em seguida, ele será encerrado.
- **Capacidade de 90 por cento**: um segundo aviso será enviado quando a utilização de espaço de captura instantânea tiver excedido 90 por cento. Assim como ao atingir a capacidade de 75 por cento, se você tomar as ações necessárias para reduzir o espaço usado, a ação será anotada e o chamado será encerrado. Se você não fizer nada, deverá confirmar manualmente o chamado e, em seguida, ele será encerrado.
- **Capacidade de 95 por cento**: um aviso final é enviado. Se nenhuma ação for executada
para trazer seu espaço abaixo do limite, uma notificação será gerada e uma exclusão automática ocorrerá para que
capturas instantâneas futuras possam ser criadas. As capturas instantâneas planejadas são excluídas, iniciando com a mais antiga, até o uso cair abaixo de 95 por cento. As capturas instantâneas continuarão a ser excluídas cada vez que a utilização exceder 95 por cento até cair abaixo do limite. Se o espaço for aumentado manualmente ou capturas instantâneas forem excluídas, o aviso será reconfigurado e emitido novamente se o limite for excedido mais uma vez. Se nenhuma ação for executada, este será o único aviso que será recebido.

## Como excluir um planejamento de captura instantânea?

Os planejamentos de captura instantânea podem ser cancelados por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique no planejamento a ser excluído no quadro **Planejamentos de captura instantânea** na página **Detalhes**.
2. Clique na caixa de seleção ao lado do planejamento a ser excluído e clique em **Salvar**.<br />
**Cuidado**: se estiver usando o recurso de replicação, certifique-se de que o planejamento que você está excluindo não é o planejamento que é usado pela replicação. Clique
[aqui](replication.html) para obter mais informações sobre como excluir um
planejamento de replicação.

## Como excluir uma captura instantânea?

As capturas instantâneas que não são mais necessárias podem ser removidas manualmente para liberar
espaço para capturas instantâneas futuras. A exclusão é feita por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique em seu volume de armazenamento e role para a seção **Captura instantânea** para ver a lista de capturas instantâneas existentes.
2. Clique em **Ações** ao lado de uma captura instantânea específica e clique em **Excluir** para excluir a captura instantânea. Isso não afetará nenhuma captura instantânea futura ou passada no mesmo planejamento, já que não há dependência entre as capturas instantâneas.

As capturas instantâneas manuais que não são excluídas da maneira descrita anteriormente são excluídas automaticamente (primeiramente as mais antigas) ao atingir as limitações de espaço.

## Como restaurar meu volume de armazenamento para um momento específico usando uma captura instantânea?

Você pode precisar retornar seu volume de armazenamento para um momento específico devido a um erro do usuário ou distorção de dados.

1. Desmonte e separe seu volume de armazenamento do host.
   - Clique [aqui](accessing_block_storage_linux.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Linux.
   - Clique [aqui](accessing-block-storage-windows.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Microsoft Windows.
2. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Role para baixo e clique no seu volume a ser restaurado. A seção **Capturas instantâneas** da página **Detalhes** exibe a lista de todas as capturas instantâneas salvas juntamente com seu tamanho e data de criação.
4. Clique em **Ações** próximo à captura instantânea a ser usada e clique em **Restaurar**. <br/>
  **Nota**: a execução de uma restauração resulta em uma perda de dados que tiverem sido
criados ou modificados desde quando a captura instantânea que está sendo usada foi obtida. Quando concluída, seu volume de armazenamento será retornado ao mesmo estado em que estava quando a captura instantânea foi tomada. 
5. Clique em **Sim** para iniciar a restauração. Espere uma mensagem na parte superior da página informando que o volume foi restaurado usando a captura instantânea selecionada. Além disso, um ícone aparece ao lado do seu volume no {{site.data.keyword.blockstorageshort}} indicando que uma transação ativa está em andamento. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparece quando a transação está concluída.
6. Monte e reconecte seu volume de armazenamento ao host.
   - Clique [aqui](accessing_block_storage_linux.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Linux.
   - Clique [aqui](accessing-block-storage-windows.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Microsoft Windows.
   
**Nota**: a restauração de um volume resultará na exclusão de todas as capturas instantâneas anteriores à captura instantânea restaurada.
