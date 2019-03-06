---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}_
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:pre: .pre}

# Gerenciando capturas instantâneas
{: #managingSnapshots}

## Criando um planejamento de captura instantânea

Você decide com que frequência e quando deseja criar uma referência de momento de seu volume de armazenamento com planejamentos de Captura instantânea. É possível ter um máximo de 50 capturas
instantâneas por volume de armazenamento. Os planejamentos são gerenciados por meio da guia **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.

Para poder configurar seu planejamento inicial, deve-se primeiramente comprar um espaço de captura instantânea, caso você não tenha comprado durante o fornecimento inicial do volume de armazenamento. Para obter mais informações, consulte [Pedindo capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).
{:important}

### Incluindo um planejamento de captura instantânea
{: #addingschedule}

Os planejamentos de capturas instantâneas podem ser configurados em intervalos, como por hora,
diários e semanais, cada um com um ciclo de retenção diferente. O limite máximo de capturas instantâneas é 50 por volume de armazenamento, que pode ser uma combinação de planejamentos por hora, diários e semanais e capturas instantâneas manuais.

1. Clique em seu volume de armazenamento, clique em **Ações** e clique em **Planejar captura instantânea**.
2. Na janela Nova captura instantânea de planejamento, é possível selecionar entre três frequências de captura instantânea diferentes. Use qualquer combinação das três frequências para criar um planejamento de captura
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
3. Clique em **Salvar** e crie outro planejamento com uma frequência diferente. Se o número total de capturas instantâneas planejadas for maior que 50, você receberá uma mensagem de aviso e não poderá salvar.

A lista de capturas instantâneas é exibida conforme obtida na seção **Capturas instantâneas** da página **Detalhe**.

Também é possível ver a lista dos planejamentos de captura instantânea por meio da CLI do SL com o comando a seguir.
```
# slcli block snapshot-schedule-list --help
Usage: slcli block snapshot-schedule-list [OPTIONS] VOLUME_ID

Options:
  -h, --help  Show this message and exit.
```
{:codeblock}

## Obtendo uma Captura Instantânea

Capturas instantâneas manuais podem ser obtidas em vários pontos durante um upgrade ou
manutenção do aplicativo. Também é possível tirar capturas instantâneas em múltiplos servidores que tenham sido desativados temporariamente no nível do aplicativo.

O limite máximo de capturas instantâneas por volume de armazenamento é 50.

1. Clique em seu volume de armazenamento.
2. Clique em ** Ações**.
3. Clique em **Executar captura instantânea manual**.
A captura instantânea é tomada e exibida na seção **Capturas instantâneas** da página **Detalhe**. Seu planejamento aparece Manual.

Como alternativa, é possível usar o comando a seguir para criar uma captura instantânea por meio da CLI do SL.
```
# slcli block snapshot-create --help
Usage: slcli block snapshot-create [OPTIONS] VOLUME_ID

Options:
  -n, --notes TEXT  Notes to set on the new snapshot
  -h, --help        Show this message and exit.
```
{:codeblock}

## Listando todas as capturas instantâneas com informações de espaço usado e funções de gerenciamento

Uma lista de capturas instantâneas retidas e o espaço que é usado podem ser vistos na página
de **Detalhes**.  As funções de gerenciamento (editando planejamentos e incluindo mais espaço) são conduzidas na página Detalhe usando o menu **Ações** ou links nas várias seções na página.

## Visualizando a lista de Capturas instantâneas retidas

As capturas instantâneas retidas baseiam-se no número inserido no campo **Manter o último** ao configurar seus planejamentos. É possível visualizar as capturas instantâneas que foram tiradas na seção **Captura instantânea**. As capturas instantâneas são listadas por planejamento.

Como alternativa, é possível usar o comando a seguir na CLI do SL para exibir as capturas instantâneas disponíveis.
```
# slcli block snapshot-list --help
Usage: slcli block snapshot-list [OPTIONS] VOLUME_ID

Options:
  --sortby TEXT   Column to sort by
  --columns TEXT  Columns to display. Options: id, name, created, size_bytes
  -h, --help      Show this message and exit.
```
{:codeblock}

## Visualizando a quantia de espaço de Captura instantânea usado

O gráfico de pizza na página **Detalhes** exibe quanto espaço é usado e quanto espaço resta. Você recebe notificações ao atingir limites de espaço - 75 por cento, 90 por cento e 95 por cento.

## Mudando a quantia de espaço de Captura instantânea para um volume

Talvez seja necessário incluir espaço de captura instantânea em um volume que não tinha nenhum anteriormente ou que pode requerer espaço de captura instantânea extra. É possível incluir de 5 a 4.000 GB, dependendo de suas necessidades.

O espaço de captura instantânea somente pode ser aumentado. Ele não pode ser reduzido. É possível selecionar uma quantidade menor de espaço até que você determine o espaço de que precisa. Lembre-se, capturas instantâneas automatizadas e manuais compartilham o espaço.
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

- Em **Capacidade de 75 por cento**, um aviso é enviado indicando que o uso do espaço de captura instantânea excedeu 75 por cento. Se você atender ao aviso e incluir espaço manualmente ou excluir capturas instantâneas retidas e desnecessárias, a ação será anotada e o chamado será encerrado. Caso não faça nada, deve-se reconhecer manualmente o chamado e, em seguida, ele será encerrado.
- Em **Capacidade de 90 por cento**, um segundo aviso é enviado quando o uso do espaço de captura instantânea excede 90 por cento. Semelhante a atingir a capacidade de 75 por cento, se você tomar as ações necessárias para diminuir o espaço usado, a ação será anotada e o chamado será encerrado. Caso não faça nada, deve-se reconhecer manualmente o chamado e, em seguida, ele será encerrado.
- Em **Capacidade de 95 por cento**, um aviso final é enviado. Se nenhuma ação for tomada para diminuir o uso de espaço para abaixo do limite, uma notificação será gerada e ocorrerá exclusão automática para que as capturas instantâneas futuras possam ser criadas. As capturas instantâneas planejadas são excluídas, iniciando com a mais antiga, até que o uso caia abaixo de 95%. As capturas instantâneas continuarão sendo excluídas sempre que o uso de tempo exceder 95% até cair abaixo do limite. Se o espaço for aumentado manualmente ou as capturas instantâneas forem excluídas, o aviso será reconfigurado e emitido novamente se o limite for mais uma vez excedido. Se nenhuma ação for executada, essa notificação será o único aviso que você receberá.

## Excluindo um planejamento de captura instantânea

Os planejamentos de captura instantânea podem ser cancelados por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique no planejamento a ser excluído no quadro **Planejamentos de captura instantânea** na página **Detalhes**.
2. Clique na caixa de seleção ao lado do planejamento a ser excluído e clique em **Salvar**.<br />

Se você estiver usando o recurso de replicação, certifique-se de que o planejamento que está sendo excluído não
seja o planejamento usado pela replicação. Para obter mais informações sobre a exclusão de um planejamento de
replicação, consulte [Replicando dados](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication).
{:important}

## Excluindo uma captura instantânea

As capturas instantâneas que não são mais necessárias podem ser removidas manualmente para liberar
espaço para capturas instantâneas futuras. A exclusão é feita por meio de **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.

1. Clique em seu volume de armazenamento e role para a seção **Captura instantânea** para ver a lista de capturas instantâneas existentes.
2. Clique em **Ações** ao lado de uma captura instantânea específica e clique em **Excluir** para excluir a captura instantânea. Essa exclusão não afeta as capturas instantâneas futuras ou passadas no mesmo planejamento, pois não há nenhuma dependência entre as capturas instantâneas.

As capturas instantâneas manuais que não são excluídas manualmente no portal, são excluídas automaticamente quando você atinge as limitações de espaço (a mais antiga primeiro).

É possível usar o comando a seguir para excluir um volume por meio da CLI do SL.
```
# slcli block snapshot-delete
Usage: slcli block snapshot-delete [OPTIONS] SNAPSHOT_ID

Options:
  -h, --help  Show this message and exit.
```
{:codeblock}


## Restaurando o volume de armazenamento para um momento específico usando uma captura instantânea

Talvez seja necessário retornar o seu volume de armazenamento para um momento específico devido a um erro do usuário ou a uma distorção de dados.

A restauração de um volume resulta na exclusão de todas as capturas instantâneas que foram obtidas após a
captura instantânea que foi usada para a restauração.
{:important}

1. Desmonte e separe seu volume de armazenamento do host.
   - [Conectando-se a LUNs iSCSI no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#unmounting)
   - [Conectando-se a LUNs iSCSI no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows#unmounting)
2. Clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
3. Role para baixo e clique no seu volume a ser restaurado. A seção **Capturas instantâneas** da página **Detalhes** exibe a lista de todas as capturas instantâneas salvas juntamente com seu tamanho e data de criação.
4. Clique em **Ações** próximo à captura instantânea a ser usada e clique em **Restaurar**. <br/>

   A conclusão da restauração resulta na perda dos dados que foram criados ou modificados depois que a captura
instantânea foi obtida. Essa perda de dados ocorre porque seu volume de armazenamento retorna para o mesmo estado em que estava no momento da captura instantânea.
   {:note}
5. Clique em  ** Sim **  para iniciar a restauração.

   Espere uma mensagem na página indicando que o volume está sendo restaurado usando a captura instantânea selecionada. Além disso, aparece um ícone próximo ao seu volume no {{site.data.keyword.blockstorageshort}} indicando que uma transação ativa está em andamento. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída.
   {:note}
6. Monte e reconecte seu volume de armazenamento ao host.
   - [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
   - [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
   - [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Como alternativa, depois que o volume for removido do host, será possível usar o comando a seguir na CLI do SL para
iniciar uma restauração.
```
# slcli block snapshot-restore --help
Usage: slcli block snapshot-restore [OPTIONS] VOLUME_ID

Options:
  -s, --snapshot-id TEXT  The id of the snapshot which is to be used to restore
                          the block volume
  -h, --help              Show this message and exit.
```
{:codeblock}  

Após a conclusão da restauração, monte e reconecte seu volume de armazenamento para o host.
