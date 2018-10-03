---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-17"

---
{:new_window: target="_blank"}

# Dados de Replicação

A replicação usa um de seus planejamentos de captura instantânea para copiar automaticamente capturas instantâneas para um volume de destino em um data center remoto. As cópias poderão ser recuperadas no site remoto se ocorrer um evento catastrófico ou se os dados forem corrompidos.

Com réplicas, é possível

- Recuperar de falhas do site e outros desastres rapidamente efetuando failover para o volume de destino.
- Efetuar failover para um momento específico na cópia de DR.

Antes de replicar, deve-se criar um planejamento de captura instantânea. Ao efetuar failover, você está "invertendo o comutador" do volume de armazenamento em seu data center primário para o volume de destino em seu data center remoto. Por exemplo, seu data center primário é Londres e seu data center secundário é Amsterdã. Se um evento de falha ocorresse, você efetuaria failover para Amsterdã - conectando-se ao volume agora primário de uma instância de cálculo em Amsterdã. Depois que seu volume em Londres é reparado, uma captura instantânea é tomada do volume de Amsterdã para efetuar failback para Londres e para o volume novamente primário de uma instância de cálculo em Londres.


## Determinando o data center remoto para meu volume de armazenamento replicado

Os data centers do {{site.data.keyword.BluSoftlayer_full}} são emparelhados em combinações primárias e remotas no mundo todo.
Veja a Tabela 1 para a lista completa de disponibilidade de data center e destinos de replicação.

<table>
	<caption style="text-align: left;"><p>Tabela 1 - Esta tabela mostra a lista completa de data centers com recursos aprimorados em cada região. Cada região é uma coluna separada. Algumas cidades, como Dallas, San Jose, Washington DC, Amsterdã, Frankfurt, Londres e Sydney, têm múltiplos data centers.</p>
		<p>&#42; Os data centers na região EUA 1 NÃO têm armazenamento aprimorado. Os hosts em data centers com recursos de armazenamento aprimorados <strong>não podem</strong> iniciar a replicação com destinos de réplica nos data centers dos EUA 1.</p>
</caption>
	<thead>
		<tr>
			<th>EUA 1 &#42;</th>
			<th>EUA 2</th>
			<th>América Latina</th>
			<th>Canadá</th>
			<th>Europa</th>
			<th>Ásia-Pacífico</th>
			<th>Austrália</t>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>DAL01<br />
				DAL05<br />
				DAL06<br />
				HOU02<br />
				SJC01<br />
				WDC01<br />
				<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>SJC03<br />
			       SJC04<br />
			       WDC04<br />
			       WDC06<br />
			       WDC07<br />
			       DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13<br />
				<br /><br />
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
				FRA04<br />
				FRA05<br />
				LON02<br />
				LON04<br />
				LON06<br />
				OSL01<br />
				PAR01<br />
				MIL01<br />
			</td>
			<td>HKG02<br />
				TOK02<br />
				TOK04<br />
				TOK05<br />
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
		</tr>
	</tbody>
</table>

## Criando a réplica inicial

As replicações funcionam com base em um planejamento de captura instantânea. Deve-se primeiro ter espaço de captura instantânea e um planejamento de captura instantânea para o volume de origem antes de poder replicar. Se você tentar configurar a replicação e uma ou a outra não estiver em vigor, será solicitado que compre mais espaço ou configure um planejamento. As replicações são gerenciadas em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu volume de armazenamento.
2. Clique em **Réplica** e em **Comprar uma replicação**.
3. Selecione o planejamento de captura instantânea existente que você deseja que sua replicação siga. A lista contém todos os seus planejamentos de captura instantânea ativa. <br />
   >**Nota** - Será possível selecionar somente um planejamento, mesmo se você tiver uma combinação de a cada hora, diário e semanal. Todas as capturas instantâneas que foram capturadas desde o ciclo de replicação anterior são replicadas independentemente do planejamento que as originou.<br />Se você não tiver Capturas instantâneas configuradas, será solicitado que faça isso antes de poder pedir replicação. Consulte [Trabalhando
com capturas instantâneas](snapshots.html) para obter mais detalhes.
3. Clique em **Local** e selecione o data center que é seu site de DR.
4. Clique em **Continuar**.
5. Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na janela são preenchidos por padrão.
6. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços principal…** e clique em **Fazer pedido**.


## Editando uma replicação existente

É possível editar seu planejamento de replicação e mudar o espaço de replicação na guia **Primário** ou **Réplica** em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## Editando o Planejamento de Replicação

O planejamento de replicação baseia-se em um planejamento de captura instantânea existente. Para mudar o planejamento de
réplica, por exemplo, de por hora para semanal, deve-se cancelar o planejamento de replicação e
configurar um novo.

A mudança do planejamento pode ser feita na guia Primário ou Réplica.

1. Clique em **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Editar planejamento de captura instantânea**.
3. Consulte o quadro **Captura instantânea** em **Planejamento** para determinar qual planejamento você está usando para replicação. Altere o planejamento que você deseja. Por exemplo, se o seu planejamento de replicação for **Diário**, será possível mudar o horário do dia em que a replicação deverá ocorrer.
4. Clique em **Salvar**.


## Mudando o espaço de Replicação

Seu espaço de captura instantânea primário e seu espaço de réplica devem ser os mesmos. Se você mudar o espaço na guia **Primário** ou **Réplica**, ele incluirá automaticamente o espaço nos data centers de origem e de destino. Aumentar o espaço de captura instantânea também aciona uma atualização de replicação imediata.

1. Clique em **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Incluir mais espaço de captura instantânea**.
3. Selecione o tamanho de armazenamento na lista e clique em **Continuar**.
4. Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo são concluídos por padrão.
5. Clique na caixa de seleção **Eu li o contrato de prestação de serviços principal…** e clique em **Fazer pedido**.


## Visualizando os volumes de réplica na Lista de volumes

É possível visualizar seus volumes de replicação na página do {{site.data.keyword.blockstorageshort}} em **Armazenamento > {{site.data.keyword.blockstorageshort}}**. O **Nome do LUN** mostra o nome do volume primário seguido por REP. O **Tipo** é Endurance ou Performance - Réplica. O **Endereço de destino** é N/A porque o volume da réplica não está montado no data center da réplica e o **Status** mostra Inativo.


## Visualizando os detalhes de um volume replicado no data center de réplica

É possível visualizar os detalhes do volume de réplica na guia **Réplica** em
**Armazenamento**, **{{site.data.keyword.blockstorageshort}}**. Outra opção é selecionar o volume de réplica na página **{{site.data.keyword.blockstorageshort}}** e clicar na guia **Réplica**.


## Especificando autorizações de host antes que ocorra failover do servidor no data center secundário

Os hosts e volumes autorizados devem estar no mesmo data center. Não é possível ter um volume de réplica em Londres e o host em Amsterdã. Ambos devem estar em Londres ou ambos devem estar em Amsterdã.

1. Clique em seu volume de origem ou de destino na página **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Réplica**.
3. Role para baixo para o quadro **Autorizar hosts** e clique em **Autorizar hosts** à direita.
4. Destaque o host que deve ser autorizado para replicações. Para selecionar múltiplos hosts, use a tecla CTRL e clique nos hosts aplicáveis.
5. Clique em **Enviar**. Se você não tiver hosts, será solicitado que compre recursos de cálculo no mesmo data center.


## Aumentando o espaço de Captura instantânea no data center de réplica quando o espaço de Captura instantânea é aumentado no data center primário

Os tamanhos de volume devem ser os mesmos para os volumes de armazenamento primário e de réplica. Um não pode ser maior que o outro. Ao aumentar o espaço de captura instantânea para o volume
primário, o espaço de réplica é aumentado automaticamente. Aumentar o espaço de captura instantânea aciona uma atualização de replicação imediata. O aumento em ambos os volumes é mostrado como itens de linha em sua fatura e é rateado conforme necessário.

Clique [aqui](snapshots.html) para saber como aumentar o espaço de captura instantânea.


## Iniciando um failover de um volume para sua réplica

Se ocorrer um evento de falha, será possível iniciar um **failover** em seu volume de destino. O volume de destino torna-se ativo. A última captura instantânea replicada com êxito é ativada e o volume é disponibilizado para montagem. Todos os dados que foram gravados no volume de origem desde que o ciclo de replicação anterior foi perdido. Quando um failover é iniciado, o relacionamento de replicação é invertido. O volume de destino torna-se o volume de origem e o volume de origem antigo torna-se o destino, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failovers são iniciados em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Antes de continuar com essas etapas, desconecte o volume. A falha ao fazer isso resultará em distorção e perda de dados.**

1. Clique em seu LUN ativo (“origem”).
2. Clique em **Réplica** e clique no link **Ações** no canto superior direito.
3. Selecione  ** Failover **.
   >Espere uma mensagem na parte superior da página indicando que o failover está em andamento. Além disso, um ícone aparece ao lado do seu volume no **{{site.data.keyword.blockstorageshort}}**, o que indica que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída. Durante o processo de failover, as ações relacionadas à configuração são somente leitura. Não é possível editar qualquer planejamento de captura instantânea nem mudar o espaço de captura instantânea. O evento é registrado no histórico de replicação.<br/> Quando seu volume de destino for em tempo real, você obterá outra mensagem. O nome do LUN do volume de origem original é atualizado para terminar em "REP" e seu Status se torna Inativo.
4. Clique em **Visualizar todos ({{site.data.keyword.blockstorageshort}})**.
5. Clique em seu LUN ativo (anteriormente seu volume de destino).
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](provisioning-block_storage.html) para obter instruções.


## Iniciando um failback de um volume para sua réplica

Quando seu volume de origem original é reparado, é possível iniciar um Failback controlado para ele. Em um failback controlado,

- O volume de origem em ação é colocado off-line,
- Uma captura instantânea é obtida,
- O ciclo de replicação está concluído,
- A captura instantânea de dados apenas obtida é ativada,
- E o volume de origem torna-se ativo para montagem.

Quando um Failback é iniciado, o relacionamento de replicação é invertido novamente. Seu volume de origem é restaurado como seu volume de origem e seu volume de destino é o volume de destino novamente, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failbacks são iniciados em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu LUN ativo ("target").
2. Na parte superior direita, clique em **Réplica** e clique em **Ações**.
3. Selecione **Failback**. >Espere uma mensagem na parte superior da página mostrando que o failover está em andamento. Além disso, um ícone aparece ao lado do seu volume no **{{site.data.keyword.blockstorageshort}}**, o que indica que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída. Durante o processo de Failback, as ações relacionadas à configuração são somente leitura. Não é possível editar qualquer planejamento de captura instantânea nem mudar o espaço de captura instantânea. O evento é registrado no histórico de replicação.
4. No canto superior direito, clique no link **Visualizar todo o {{site.data.keyword.blockstorageshort}}**.
5. Clique em seu LUN ativo ("origem").
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](provisioning-block_storage.html) para obter instruções.


## Visualizando o histórico de replicação

O histórico de replicação pode ser visualizado no **Log de auditoria** na guia **Conta** em **Gerenciar**. Os volumes primário e de réplica exibem histórico de replicação idêntico. O histórico inclui:

- Tipo da replicação (failover ou failback)
- Quando ele foi iniciado
- Captura instantânea que foi usada para a replicação
- Tamanho da replicação
- Quando é concluído


## Criando uma duplicata de uma réplica

É possível criar uma duplicata de um {{site.data.keyword.BluSoftlayer_full}} {{site.data.keyword.blockstoragefull}} existente. O volume duplicado herda as opções de capacidade e de desempenho do LUN/volume original por padrão e tem uma cópia dos dados até o momento de uma captura instantânea.

As duplicatas podem ser criadas de ambos os volumes, o primário e o de réplica. A nova duplicata é criada no mesmo data center que o volume original. Se você criar uma duplicata de um volume de réplica, o novo volume será criado no mesmo data center que o volume de réplica.

Os volumes duplicados podem ser acessados por um host para leitura/gravação assim que o armazenamento
é provisionado. No entanto, capturas instantâneas e replicação não são permitidas até que a cópia de dados do original para a duplicata seja concluída.

Para obter mais informações, consulte [Criando um volume de bloco duplicado](how-to-create-duplicate-volume.html)


## Cancelando uma Replicação Existente

É possível cancelar a replicação imediatamente ou na data de aniversário, que faz com que o faturamento termine. A replicação pode ser cancelada nas guias **Primário** ou **Réplica**.

1. Clique no volume na página **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Ações** na guia **Primário** ou **Réplica**.
3. Selecione **Cancelar réplica**.
4. Selecione quando cancelar. Escolha **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
5. Clique em **Eu reconheço que, devido ao cancelamento, a perda de dados pode ocorrer** e clique em **Cancelar réplica**.


## Cancelando a replicação quando o volume primário é cancelado

Quando um volume primário é cancelado, o planejamento de replicação e o volume no data center de réplica são excluídos. As réplicas são canceladas na página {{site.data.keyword.blockstorageshort}}.

 1. Destaque seu volume na página **{{site.data.keyword.blockstorageshort}}**.
 2. Clique em **Ações** e selecione **Cancelar {{site.data.keyword.blockstorageshort}}**.
 3. Selecione quando cancelar. Escolha **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
 4. Clique em **Eu reconheço que, devido ao cancelamento, a perda de dados pode ocorrer** e clique em **Cancelar**.
