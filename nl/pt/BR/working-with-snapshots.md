---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Gerenciando capturas instantâneas

## Como criar um planejamento de captura instantânea?

Os planejamentos de captura instantânea permitem que você decida com que frequência e quando deseja
criar uma referência em um momento de seu volume de armazenamento. É possível ter um máximo de 50 capturas
instantâneas por volume de armazenamento. Os planejamentos são gerenciados por meio da
guia **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}**
do
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

Para poder configurar seu planejamento inicial, deve-se primeiramente comprar um espaço de captura
instantânea, caso você não tenha comprado durante o fornecimento inicial do volume de armazenamento.

### Como incluir um planejamento de captura instantânea?

Os planejamentos de capturas instantâneas podem ser configurados em intervalos, como por hora,
diários e semanais, cada um com um ciclo de retenção diferente. Há um máximo de 50 capturas
instantâneas planejadas, podendo ser uma combinação de planejamentos por hora, diários e
semanais e capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento, clique na caixa suspensa **Ações** e
clique em **Planejar captura instantânea**.
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
3. Clique em **Salvar** e crie outro planejamento com uma frequência diferente. 
Observe que você receberá uma mensagem de aviso e não será possível salvar se o número total de capturas
instantâneas planejadas for maior que 50.

Uma lista das capturas instantâneas é exibida conforme elas são obtidas na seção Capturas instantâneas
da página Detalhes.

## Como obter uma captura instantânea manual?

Capturas instantâneas manuais podem ser obtidas em vários pontos durante um upgrade ou
manutenção do aplicativo. Elas também permitem obter capturas instantâneas em múltiplas máquinas que tenham sido
desativadas temporariamente no nível do aplicativo.

Há um máximo de 50 capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento.
2. Clique na caixa suspensa Ações.
3. Clique em **Executar captura instantânea manual**.
A captura instantânea será executada e exibida na seção Capturas instantâneas da página Detalhes. O
planejamento será Manual.

## Como ver uma lista de capturas instantâneas com espaço consumido e funções de gerenciamento?

Uma lista de capturas instantâneas retidas e do espaço consumido pode ser vista na
página **Detalhes** (Armazenamento, {{site.data.keyword.blockstorageshort}}). 
As funções de gerenciamento (edição de planejamentos e inclusão de mais espaço) são realizadas na página
Detalhes usando o menu suspenso **Ações** ou os links nas várias seções na
página.

## Como visualizar uma lista de capturas instantâneas retidas?

As capturas instantâneas retidas são baseadas no número que você inseriu no campo Manter o último
ao configurar seus planejamentos. É possível visualizar as capturas instantâneas que foram obtidas na seção
Captura instantânea. As capturas instantâneas são listadas por planejamento.

## Como ver a quantia de espaço de captura instantânea que foi utilizada?

O gráfico de pizza na parte superior da página Detalhes exibe a quantia de espaço
utilizada e a quantia de espaço restante. Você receberá notificações quando começar a atingir os limites de
espaço, 75%, 90% e 95%. 
## Como mudar a quantia de espaço de captura instantânea para Meu volume?

Poderá ser necessário incluir espaço de captura instantânea em um volume que não tenha tido anteriormente
ou que possa requerer espaço de captura instantânea adicional. É possível incluir de 5 GB a 4.000 GB,
dependendo suas necessidades. 

**Nota**: o espaço de captura instantânea pode ser somente aumentado, não reduzido. 
É possível selecionar uma quantia menor de espaço até determinar a quantia de espaço que você realmente
precisa. Lembre-se de que as capturas instantâneas automatizadas e manuais compartilham o mesmo espaço.

O espaço de captura instantânea é mudado por meio de **Armazenamento,
{{site.data.keyword.blockstorageshort}}**.

1. Clique em seus volumes de armazenamento, clique na caixa suspensa **Ações**
e clique em **Incluir mais espaço de captura instantânea**.
2. Selecione em um intervalo de tamanhos no prompt. Os tamanhos geralmente variam de 0 até o tamanho
de seu volume.
3. Clique em **Continuar** para provisionar o espaço adicional.
4. Insira qualquer Código promocional que tenha e clique em **Recalcular**. Os Encargos para essa ordem e Revisão de ordem terão valores padrão.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…** e clique em **Colocar a ordem**. 
Seu espaço de captura instantânea adicional será provisionado em alguns minutos.

## Como receber notificações quando estou próximo de meu limite de espaço de captura instantânea e
quando as capturas instantâneas são excluídas?

As notificações são enviadas por meio de chamados de suporte do Suporte para o Usuário principal em sua
conta quando você atinge três limites de espaço diferentes, 75%, 90% e 95%. 
- **75% de capacidade**: um aviso é enviado quanto à utilização de espaço de
captura instantânea excede 75%. Se você seguir o aviso e incluir manualmente espaço ou excluir capturas
instantâneas retidas e desnecessárias, a ação será registrada e o chamado será encerrado.
Se você não fizer nada, deverá confirmar manualmente o chamado e, em seguida, ele será encerrado.
- **90% de capacidade**: um segundo aviso é enviado quando a utilização de espaço de
captura instantânea excede 90%. Assim como quando você atinge 75% da capacidade, se executar as ações
necessárias para reduzir o espaço utilizado, a ação será registrada e o chamado será encerrado.
Se você não fizer nada, deverá confirmar manualmente o chamado e, em seguida, ele será encerrado.
- **95% da capacidade**: um aviso final é enviado. Se nenhuma ação for executada
para trazer seu espaço abaixo do limite, uma notificação será gerada e uma exclusão automática ocorrerá para que
capturas instantâneas futuras possam ser criadas. As capturas instantâneas planejadas são excluídas, iniciando
com a mais antiga, até o uso cair abaixo de 95% e a exclusão continua cada vez
que a utilização excede 95% até cair abaixo do limite. Quando o espaço é aumentado manualmente e as capturas
instantâneas são excluídas, o aviso é reconfigurado e emitido novamente se o limite é excedido
mais uma vez. Se nenhuma ação for executada, este será o único aviso que será recebido.

## Como excluir um planejamento de captura instantânea?

Os planejamentos de captura instantânea podem ser cancelados por meio de **Armazenamento,
{{site.data.keyword.blockstorageshort}}**.

1. Clique no planejamento a ser excluído no quadro **Planejamentos de captura
instantânea** na página **Detalhes**.
2. Clique na caixa de seleção ao lado do planejamento a ser excluído e clique em
**Salvar**.<br />
**Cuidado**: se estiver usando o recurso de replicação, certifique-se de que o planejamento
que você está excluindo não seja o planejamento que está sendo usado pela replicação. Clique
[aqui](replication.html) para obter mais informações sobre como excluir um
planejamento de replicação.

## Como excluir uma captura instantânea?

As capturas instantâneas que não são mais necessárias podem ser removidas manualmente para liberar
espaço para capturas instantâneas futuras. A exclusão é feita por meio de **Armazenamento,
{{site.data.keyword.blockstorageshort}}**.

1. Clique em seu volume de armazenamento e role para baixo até a seção Captura instantânea para ver
uma lista de capturas instantâneas existentes.
2. Clique na lista suspensa **Ações** ao lado de uma captura instantânea
específica e clique em **Excluir** para excluir a captura instantânea. Isso não afeta
nenhuma captura instantânea futura ou anterior no mesmo planejamento, já que não há dependência entre as
capturas instantâneas.

As capturas instantâneas manuais que não são excluídas na maneira descrita acima são
excluídas automaticamente (primeiramente a mais antiga) ao atingir as limitações de espaço.

## Como restaurar meu volume de armazenamento para um momento específico usando uma
captura instantânea?

Pode ser necessário retornar seu volume de armazenamento de volta para um momento específico
devido a um erro do usuário ou distorção de dados.

1. Desmonte e separe seu volume de armazenamento do host.
   - Clique [aqui](accessing_block_storage_linux.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Linux.
   - Clique [aqui](accessing-block-storage-windows.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Microsoft Windows.
2. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
3. Role para baixo e clique no seu volume a ser restaurado. A seção **Capturas
instantâneas** da página **Detalhes** exibirá uma lista de todas as
capturas instantâneas salvas juntamente com seu tamanho e data de criação.
4. Clique no botão **Ações** para a captura instantânea a ser usada e clique
em **Restaurar**. <br/>
  **Nota**: a execução de uma restauração resulta em uma perda de dados que tiverem sido
criados ou modificados desde quando a captura instantânea que está sendo usada foi obtida. Uma vez concluída,
seu volume de armazenamento retorna ao mesmo estado em que estava quando a captura instantânea foi
obtida. Um prompt será exibido para notificá-lo sobre isso.
5. Clique em **Sim** para iniciar a restauração. Você receberá uma mensagem na parte
superior da página informando que o volume foi restaurado usando a captura instantânea selecionada. Além
disso, um ícone aparecerá ao lado do seu volume no {{site.data.keyword.blockstorageshort}} indicando
que uma transação ativa está em andamento. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparecerá quando a transação estiver concluída.
6. Monte e reconecte seu volume de armazenamento ao host.
   - Clique [aqui](accessing_block_storage_linux.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Linux.
   - Clique [aqui](accessing-block-storage-windows.html) para
{{site.data.keyword.blockstorageshort}}obter instruções no Microsoft Windows.
   
**Nota**: a restauração de um volume resulta na exclusão de todas as capturas
instantâneas anteriores à captura instantânea restaurada.
