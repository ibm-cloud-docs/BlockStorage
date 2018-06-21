---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-22"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}

# Trabalhando com replicação

A replicação usa um de seus planejamentos de captura instantânea para copiar automaticamente capturas instantâneas para um volume de destino em um data center remoto. As cópias poderão ser recuperadas no site remoto se ocorrer um evento catastrófico ou se os dados forem corrompidos.

Com réplicas, é possível:

- Recuperar de falhas do site e outros desastres rapidamente efetuando failover para o volume de destino.
- Efetuar failover para um momento específico na cópia de DR.

Antes de replicar, deve-se criar um planejamento de captura instantânea. Ao efetuar failover, você está "invertendo o comutador" do volume de armazenamento em seu data center primário para o volume de destino em seu data center remoto. Por exemplo, seu data center primário é Londres e seu data center secundário é Amsterdã. Se um evento de falha ocorresse, você efetuaria failover para Amsterdã - conectando-se ao volume agora primário de uma instância de cálculo em Amsterdã. Depois que seu volume em Londres é reparado, uma captura instantânea é tomada do volume de Amsterdã para efetuar failback para Londres e para o volume novamente primário de uma instância de cálculo em Londres.


## Como determinar o data center remoto para meu volume de armazenamento replicado?

Os data centers do {{site.data.keyword.BluSoftlayer_full}} foram emparelhados em combinações primárias e remotas em todo o mundo.
Veja a Tabela 1 para a lista completa de disponibilidade de data center e destinos de replicação.

<table style="width: 80.0%;">
	<caption style="text-align: left;"><p>Tabela 1 - Esta tabela mostra a lista completa de data centers com recursos aprimorados em cada região. Cada região é uma coluna separada. Algumas cidades, como Dallas, San Jose, Washington DC, Amsterdã, Frankfurt, Londres e Sydney, têm múltiplos data centers.</p>
		<p>&#42; Os data centers na região EUA 1 NÃO têm armazenamento aprimorado. Os hosts em data centers com recursos de armazenamento aprimorados <strong>não podem</strong> iniciar a replicação com destinos de réplica em data centers dos EUA 1.</p>
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
				SNG01<br />
				SEO01<br />
                                CHE01<br />
				<br />
				<br />
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

## Como criar uma replicação inicial?

As replicações funcionam com base em um planejamento de captura instantânea. Deve-se primeiro ter um espaço de captura instantânea e um planejamento de captura instantânea configurado para o volume de origem antes de poder replicar. Você receberá prompts que permitem saber que espaço precisará ser comprado ou um planejamento configurado se tentar configurar a replicação e um ou outro não estiver adequado. As replicações são gerenciadas em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu volume de armazenamento.
2. Clique em **Réplica** e em **Comprar uma replicação**.
Selecione o planejamento de captura instantânea existente que você deseja que sua replicação siga. A lista contém todos os seus planejamentos de captura instantânea ativa. <br />
  **Nota:** é possível selecionar um planejamento, mesmo se você possui uma combinação de
planejamentos por hora, diários e semanais. Todas as capturas instantâneas capturadas desde o ciclo de replicação anterior serão replicadas independentemente do planejamento que as originou.<br />
  **Nota:** se as Capturas instantâneas não estiverem configuradas, você será solicitado a fazer isso antes de poder pedir a replicação. Consulte [Trabalhando
com capturas instantâneas](snapshots.html) para obter mais detalhes.
3. Clique na seta da lista suspensa **Local** e selecione o data center que será o seu site de DR.
4. Clique em **Continuar**.
5. Insira um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo são concluídos por padrão.
6. Clique na caixa de seleção **Eu li o contrato de prestação de serviços principal…** e clique em **Colocar ordem**.


## Como editar uma replicação existente?

É possível editar seu planejamento de replicação e mudar o espaço de replicação na guia **Primário** ou **Réplica** em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.



## Como editar um planejamento de replicação?

Na realidade, você está mudando um planejamento de captura instantânea porque seu planejamento de replicação é baseado em um planejamento de captura instantânea existente. Para mudar o planejamento de
réplica, por exemplo, de por hora para semanal, deve-se cancelar o planejamento de replicação e
configurar um novo.

A mudança do planejamento pode ser feita na guia Primário ou Réplica.

1. Clique em **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Editar planejamento de captura instantânea**.
3. Consulte o quadro **Captura instantânea** em **Planejamento** para determinar qual planejamento você está usando para replicação. Faça as mudanças no planejamento usado para replicação. Por exemplo, se o seu planejamento de replicação for **Diário**, será possível mudar o horário do dia em que a replicação deverá ocorrer.
4. Clique em **Salvar**.


## Como mudar o espaço de replicação?

Seu espaço de captura instantânea primário e seu espaço de réplica devem ser iguais. Se você mudar o espaço na guia **Primário** ou **Réplica**, ele incluirá espaço automaticamente nos data centers de origem e de destino. Esteja ciente de que aumentar o espaço de captura instantânea acionará uma atualização de replicação imediata.

1. Clique em **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Incluir mais espaço de captura instantânea**.
3. Selecione o tamanho de armazenamento na lista e clique em **Continuar**.
4. Insira em um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo são concluídos por padrão.
5. Clique na caixa de seleção **Eu li o contrato de prestação de serviços principal…** e clique em **Colocar ordem**.


## Como ver meus volumes de réplica na Lista de volumes?

É possível visualizar seus volumes de replicação na página do {{site.data.keyword.blockstorageshort}} em **Armazenamento > {{site.data.keyword.blockstorageshort}}**. O **Nome do LUN** mostra o nome do volume primário seguido por REP. O **Tipo** é Endurance ou Performance - Réplica. O **Endereço de destino** é N/A porque o volume da réplica não está montado no data center da réplica e o **Status** mostra Inativo.



## Como visualizar detalhes de um volume replicado no data center de réplica?

É possível visualizar os detalhes do volume de réplica na guia **Réplica** em
**Armazenamento**, **{{site.data.keyword.blockstorageshort}}**. Outra opção é selecionar o volume de réplica na página **{{site.data.keyword.blockstorageshort}}** e clicar na guia **Réplica**.



## Como especificar autorizações de host antes de efetuar failover para o data center secundário?

Os hosts e volumes autorizados devem estar no mesmo data center. Não é possível ter um volume de réplica em Londres e o host em Amsterdã; ambos devem estar em Londres ou em Amsterdã.

1. Clique em seu volume de origem ou de destino na página **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Réplica**.
3. Role para baixo para o quadro **Autorizar hosts** e clique em **Autorizar hosts** à direita.
4. Destaque o host que deve ser autorizado para replicações. Para selecionar múltiplos hosts, use a tecla CTRL e clique nos hosts aplicáveis.
5. Clique em **Enviar**. Se você não tiver hosts, a caixa de diálogo oferecerá a opção de comprar recursos de cálculo no mesmo data center.


## Como aumentar o espaço de captura instantânea em meu data center de réplica quando aumento o espaço em meu data center primário?

Os tamanhos de volume devem ser os mesmos para os volumes de armazenamento primário e de réplica. Um não pode ser maior que o outro. Ao aumentar o espaço de captura instantânea para o volume
primário, o espaço de réplica é aumentado automaticamente. Esteja ciente de que aumentar o espaço de captura instantânea acionará uma atualização de replicação imediata. O aumento de ambos os volumes será mostrado como itens de linha em sua fatura e será rateado conforme
necessário.

Clique [aqui](snapshots.html) para saber como aumentar o espaço de captura instantânea.



## Como iniciar um failover de um volume para sua réplica?

Se ocorrer um evento de falha, será possível iniciar um **failover** para seu volume de destino ou alvo. O volume de destino torna-se ativo. A última captura instantânea replicada com êxito é ativada e o volume é disponibilizado para montagem. Todos os dados gravados no volume de origem desde o ciclo de replicação anterior serão perdidos. Esteja ciente de que, quando um failover for iniciado, o relacionamento de replicação será invertido. O volume de destino torna-se o volume de origem e o volume de origem antigo torna-se o destino, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failovers são iniciados em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Antes de continuar com essas etapas, recomenda-se desconectar o volume. A falha em fazer isso terminará com distorção e perda de dados.**

1. Clique em seu LUN ativo (“origem”).
2. Clique em **Réplica** e clique no link **Ações** no canto superior direito.
3. Selecione Failover.
   Espere uma mensagem na parte superior da página informando que o failover está em andamento. Além disso, um ícone aparece próximo ao seu volume no **{{site.data.keyword.blockstorageshort}}** indicando que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparece quando a transação está concluída. Durante o processo de failover, as ações relacionadas à configuração são somente leitura. Não é possível editar nenhum planejamento de captura instantânea, mudar o espaço de captura instantânea e assim por diante. O evento é registrado no histórico de replicação.
   Outra mensagem informará quando seu volume de destino está ativo. O Nome do LUN do volume de origem original será seguido por REP e seu status será Inativo.
4. Clique em **Visualizar todos ({{site.data.keyword.blockstorageshort}})**.
5. Clique em seu LUN ativo (anteriormente seu volume de destino). Esse volume agora tem um status **Ativo**.
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](provisioning-block_storage.html) para obter instruções.


## Como iniciar um failback de um volume para sua réplica?

Quando o volume de origem original tiver sido reparado, será possível iniciar um failback controlado para seu volume de origem original. Em um failback controlado,

- O volume de origem de atuação é colocado off-line;
- Uma captura instantânea é tomada;
- O ciclo de replicação é concluído;
- A captura instantânea de dados recém-tomada é ativada;
- E o volume de origem torna-se ativo para montagem.

Esteja ciente de que, quando um Failback for iniciado, o relacionamento de replicação será invertido novamente. Seu volume de origem é restaurado como seu volume de origem e seu volume de destino é o volume de destino novamente, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failbacks são iniciados em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu LUN do Endurance ativo ("destino").
2. Clique em **Réplica** e clique em **Ações** no canto superior direito.
3. Selecione **Failback**. Espere uma mensagem na parte superior da página mostrando que o failover está em andamento. Além disso, um ícone aparece próximo ao seu volume no **{{site.data.keyword.blockstorageshort}}** indicando que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma caixa de diálogo que indica a transação. O ícone desaparece quando a transação está concluída. Durante o processo de Failback, as ações relacionadas à configuração são somente leitura. Não é possível editar nenhum planejamento de captura instantânea, mudar o espaço de captura instantânea e assim por diante. O evento é registrado no histórico de replicação. Outra mensagem informa quando o volume de origem está em tempo real. Seu volume de destino tem um status Inativo.
4. No canto superior direito, clique no link **Visualizar todos os {{site.data.keyword.blockstorageshort}}**.
5. Clique no LUN do Endurance ativo (origem). Esse volume tem um status **Ativo** agora.
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](provisioning-block_storage.html) para obter instruções.


## Como ver meu histórico de replicação?

O histórico de replicação é visualizado no **Log de auditoria** na guia **Conta** em **Gerenciar**. Os volumes primário e de réplica exibem um histórico de replicação idêntico, que inclui:

- Tipo da replicação (failover ou failback)
- Quando foi iniciado
- Captura instantânea usada para a replicação
- Tamanho da replicação
- Quando é concluído


## Como cancelar uma replicação existente?

É possível cancelar a replicação imediatamente ou na data de aniversário, que faz com que o faturamento termine. A replicação pode ser cancelada nas guias **Primário** ou **Réplica**.

1. Clique no volume na página **{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Ações** na guia **Primário** ou **Réplica**.
3. Selecione **Cancelar réplica**.
4. Selecione quando cancelar. - **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
5. Clique na caixa de seleção **Eu reconheço que pode ocorrer perda de dados devido ao cancelamento** e clique em **Cancelar réplica**.


## Como cancelar a replicação quando o volume primário é cancelado?

Quando um volume primário é cancelado, o planejamento de replicação e o volume no data center de réplica são excluídos. As réplicas são canceladas na página {{site.data.keyword.blockstorageshort}}.

 1. Destaque seu volume na página **{{site.data.keyword.blockstorageshort}}**.
 2. Clique em **Ações** e selecione **Cancelar {{site.data.keyword.blockstorageshort}}**.
 3. Selecione quando cancelar. - **Imediatamente** ou **Data de aniversário** e clique em **Continuar**.
 4. Clique na caixa de seleção **Eu reconheço que pode ocorrer perda de dados devido ao cancelamento** e clique em **Cancelar**.
