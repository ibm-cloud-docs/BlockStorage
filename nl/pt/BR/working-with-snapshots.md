---

copyright:
  years: 2014, 2018
lastupdated: "2018-11-30"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Gerenciando capturas instantâneas

## Criando um planejamento de captura instantânea

Você decide com que frequência e quando deseja criar uma referência de momento de seu volume de armazenamento com planejamentos de Captura instantânea. É possível ter um máximo de 50 capturas
instantâneas por volume de armazenamento. Os planejamentos são gerenciados por meio da guia **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** do [{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.

Para poder configurar seu planejamento inicial, deve-se primeiramente comprar um espaço de captura instantânea, caso você não tenha comprado durante o fornecimento inicial do volume de armazenamento.
{:important}

### Incluindo um planejamento de captura instantânea

Os planejamentos de capturas instantâneas podem ser configurados em intervalos, como por hora,
diários e semanais, cada um com um ciclo de retenção diferente. Há um limite máximo de 50 capturas instantâneas
planejadas, que pode ser uma combinação de planejamentos por hora, por dia e por semana, e capturas instantâneas
manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento, clique em **Ações** e clique em **Planejar captura instantânea**.
2. Na janela Nova captura instantânea de planejamento, há três frequências de captura instantânea diferentes para seleção. Use qualquer combinação das três frequências para criar um planejamento de captura
instantânea abrangente.
   - Hora em Hora
      - Especifique o minuto de cada hora em que uma captura instantânea deve ser tirada. O padrão é o minuto atual.
      - Especifique o número de capturas instantâneas por hora a serem retidas antes que a mais antiga seja descartada.
   - Diário
      - Especifique a hora e o minuto em que uma captura instantânea deve ser tirada. O padrão é a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas por hora a serem retidas antes que a mais antiga seja descartada.
   - Semanal
      - Especifique o dia da semana, a hora e o minuto em que uma captura instantânea deve ser tirada. O padrão é o dia, a hora e o minuto atuais.
      - Selecione o número de capturas instantâneas semanais a serem retidas antes que a mais antiga seja descartada.
3. Clique em **Salvar** e crie outro planejamento com uma frequência diferente. Se o número total de capturas instantâneas planejadas for superior a 50, você receberá uma mensagem de aviso e não poderá salvar.

A lista de capturas instantâneas é exibida conforme elas são tomadas na seção **Capturas instantâneas** da página **Detalhe**.

## Obtendo uma Captura Instantânea

Capturas instantâneas manuais podem ser obtidas em vários pontos durante um upgrade ou
manutenção do aplicativo. Também é possível tirar capturas instantâneas em múltiplos servidores que tenham sido desativados temporariamente no nível do aplicativo.

Há um limite máximo de 50 capturas instantâneas manuais por volume de armazenamento.

1. Clique em seu volume de armazenamento.
2. Clique em ** Ações**.
3. Clique em **Executar captura instantânea manual**.
A captura instantânea é tomada e exibida na seção **Capturas instantâneas** da página **Detalhe**. Seu planejamento aparece Manual.

## Listando todas as capturas instantâneas com informações de espaço usado e funções de gerenciamento

Uma lista de capturas instantâneas retidas e o espaço que é usado podem ser vistos na página
de **Detalhes**.  As funções de gerenciamento (editando planejamentos e incluindo mais espaço) são conduzidas na página Detalhe usando o menu **Ações** ou links nas várias seções na página.

## Visualizando a lista de Capturas instantâneas retidas

As capturas instantâneas retidas baseiam-se no número inserido no campo **Manter o último** ao configurar seus planejamentos. É possível visualizar as capturas instantâneas que foram tiradas na seção **Captura instantânea**. As capturas instantâneas são listadas por planejamento.

## Visualizando a quantia de espaço de Captura instantânea usado

O gráfico de pizza na parte superior da página **Detalhes** exibe quanto espaço é usado e quanto espaço ainda resta. Você recebe notificações ao atingir limites de espaço - 75 por cento, 90 por cento e 95 por cento.

## Mudando a quantia de espaço de Captura instantânea para um volume

Talvez seja necessário incluir espaço de captura instantânea em um volume que não tinha nenhum anteriormente ou que pode requerer espaço de captura instantânea extra. É possível incluir de 5 a 4.000 GB, dependendo de suas necessidades.

O espaço de captura instantânea somente pode ser aumentado. Ele não pode ser reduzido. Será possível selecionar uma quantia menor de espaço até você determinar quanto espaço realmente é necessário. Lembre-se, capturas instantâneas automatizadas e manuais compartilham o espaço.
{:note}

O espaço de captura instantânea é mudado por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique em seus volumes de armazenamento, clique em **Ações** e clique em **Incluir mais espaço de captura instantânea**.
2. Selecione em um intervalo de tamanhos no prompt. Os tamanhos geralmente variam de 0 até o tamanho
de seu volume.
3. Clique em **Continuar**.
4. Insira qualquer Código promocional que você tiver e clique em **Recalcular**. Os campos Encargos para este pedido e Revisão do pedido são concluídos por padrão.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principais…**
e clique em **Fazer pedido**. Seu espaço de captura instantânea adicional é provisionado em alguns minutos.

## Recebendo notificações quando o limite de espaço de captura instantânea é atingido e as capturas instantâneas são excluídas

As notificações são enviadas por meio dos chamados de suporte para o Usuário principal em sua conta quando você atinge três limites de espaço diferentes, 75 por cento, 90 por cento e 95 por cento.

- **Capacidade de 75 por cento**: um aviso é enviado de que o uso de espaço de captura instantânea excedeu 75 por cento. Se você atender ao aviso e incluir espaço manualmente ou excluir capturas instantâneas retidas e desnecessárias, a ação será anotada e o chamado será encerrado. Caso não faça nada, deve-se reconhecer manualmente o chamado e, em seguida, ele será encerrado.
- **Capacidade de 90 por cento**: um segundo aviso é enviado quando o uso de espaço de captura instantânea excede 90 por cento. Semelhante a atingir a capacidade de 75 por cento, se você tomar as ações necessárias para diminuir o espaço usado, a ação será anotada e o chamado será encerrado. Caso não faça nada, deve-se reconhecer manualmente o chamado e, em seguida, ele será encerrado.
- **Capacidade de 95 por cento**: um aviso final é enviado. Se nenhuma ação for executada para que o uso de espaço fique abaixo do limite, uma notificação será gerada e a exclusão automática ocorrerá para que futuras capturas instantâneas possam ser criadas. As capturas instantâneas planejadas são excluídas, iniciando com a mais antiga, até o uso cair abaixo de 95 por cento. As capturas instantâneas continuarão sendo excluídas sempre que o uso de tempo exceder 95 por cento, até cair abaixo do limite. Se o espaço for aumentado manualmente ou as capturas instantâneas forem excluídas, o aviso será reconfigurado e emitido novamente se o limite for mais uma vez excedido. Se nenhuma ação for executada, essa notificação será o único aviso que você receberá.

## Excluindo um planejamento de captura instantânea

Os planejamentos de captura instantânea podem ser cancelados por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique no planejamento a ser excluído no quadro **Planejamentos de captura instantânea** na página **Detalhes**.
2. Clique na caixa de seleção ao lado do planejamento a ser excluído e clique em **Salvar**.<br />

Se você estiver usando o recurso de replicação, certifique-se de que o planejamento que está sendo excluído não
seja o planejamento usado pela replicação. Para obter mais informações sobre a exclusão de um planejamento de
replicação, consulte [Replicando dados](replication.html).
{:important}

## Excluindo uma captura instantânea

As capturas instantâneas que não são mais necessárias podem ser removidas manualmente para liberar
espaço para capturas instantâneas futuras. A exclusão é feita por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique em seu volume de armazenamento e role para a seção **Captura instantânea** para ver a lista de capturas instantâneas existentes.
2. Clique em **Ações** ao lado de uma captura instantânea específica e clique em **Excluir** para excluir a captura instantânea. Essa exclusão não afetará nenhuma captura instantânea futura ou passada no mesmo planejamento, pois não há nenhuma dependência entre as capturas instantâneas.

As capturas instantâneas manuais não excluídas da maneira descrita anteriormente são excluídas automaticamente quando você atinge limitações de espaço (a mais antiga primeiro).

## Restaurando o volume de armazenamento para um momento específico usando uma captura instantânea

Talvez seja necessário retornar o seu volume de armazenamento para um momento específico devido a um erro do usuário ou a uma distorção de dados.

1. Desmonte e separe seu volume de armazenamento do host.
   - [Conectando-se a LUNs iSCSI de MPIO no Linux](accessing_block_storage_linux.html#un-mounting-block-storage-volumes)
   - [Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows](accessing-block-storage-windows.html#unmounting-block-storage-volumes)
2. Clique em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** no
[{{site.data.keyword.slportal}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://control.softlayer.com/){:new_window}.
3. Role para baixo e clique no seu volume a ser restaurado. A seção **Capturas instantâneas** da página **Detalhes** exibe a lista de todas as capturas instantâneas salvas juntamente com seu tamanho e data de criação.
4. Clique em **Ações** próximo à captura instantânea a ser usada e clique em **Restaurar**. <br/>

   A conclusão da restauração resulta na perda dos dados que foram criados ou modificados depois que a captura
instantânea foi obtida. Essa perda de dados ocorre porque seu volume de armazenamento retorna para o mesmo estado em que estava no momento da captura instantânea.
   {:note}
5. Clique em  ** Sim **  para iniciar a restauração. Espere uma mensagem na parte superior da página indicando que o volume está sendo restaurado usando a captura instantânea selecionada. Além disso, aparece um ícone próximo ao seu volume no {{site.data.keyword.blockstorageshort}} indicando que uma transação ativa está em andamento. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída.
6. Monte e reconecte seu volume de armazenamento ao host.
   - [Conectando-se a LUNs iSCSI de MPIO no Linux](accessing_block_storage_linux.html)
   - [Conectando-se a LUNs do iSCSI de MPIO no CloudLinux](configure-iscsi-cloudlinux.html)
   - [Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows](accessing-block-storage-windows.html)

A restauração de um volume resulta na exclusão de todas as capturas instantâneas que foram obtidas após a
captura instantânea que foi usada para a restauração.
{:important}
