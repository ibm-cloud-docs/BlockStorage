---
 
copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"
 
---

{:shortdesc: .shortdesc} 
{:new_window: target="_blank"}

# Trabalhando com a replicação

A replicação usa um dos seus planejamentos de captura instantânea para copiar automaticamente capturas
instantâneas para um volume de destino em um data center remoto. As cópias podem ser recuperadas no site
remoto no evento de dados corrompidos ou em um evento catastrófico.

As réplicas permitem:

- Recuperar-se de falhas do site e de outros desastres rapidamente efetuando failover para o volume de
destino,
- Efetuar failover para um momento específico na cópia de DR.

Para poder replicar, deve-se criar um planejamento de captura instantânea. Ao efetuar failover, você
“inverte o comutador” do seu volume de armazenamento em seu data center primário para o volume de
destino em seu data center remoto. Por exemplo, seu data center primário está em Londres e seu data center
secundário está em Amsterdã. No caso de um evento de falha, você efetua failover para Amsterdã, conectando-se ao volume agora
primário de uma instância de cálculo em Amsterdã. Depois que seu volume em Londres é reparado, uma captura
instantânea é obtida do volume de Amsterdã para efetuar fallback para Londres e para o volume novamente primário
de uma instância de cálculo em Londres.

 
**Nota:** a menos que seja indicado de outra forma, as etapas são as mesmas para o
{{site.data.keyword.blockstoragefull}} e o Armazenamento de arquivo.

## Como determinar o data center remoto para meu volume de armazenamento replicado?

Os data centers mundiais do {{site.data.keyword.BluSoftlayer_full}} foram emparelhados nas
combinações primárias e remotas.
Consulte a Tabela 1 para obter uma lista completa dos destinos de disponibilidade e de
replicação de data center.
Observe que algumas cidades, como Dallas, San Jose, Washington, D.C. e Amsterdã possuem múltiplos data centers. 


<table cellpadding="1" cellspacing="1">
	<caption>Tabela 1</caption>
	<tbody>
		<tr>
			<td><strong>EUA 1</strong><sup><img src="/images/numberone.png" alt="1" /></sup></td>
			<td><strong>EUA 2</strong></td>
			<td><strong>América do Sul/Latina</strong></td>
			<td><strong>Canadá</strong></td>
			<td><strong>Europa</strong></td>
			<td><strong>Ásia-Pacífico</strong></td>
			<td><strong>Austrália</strong></td>
		</tr>
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
			</td>
			<td>MEX01<br />
				SAO01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br />
			</td>
			<td>
				AMS01<br />
				AMS03<br />
				FRA02<br />
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
                                _____<br />
				CHE01<sup><img src="/images/numberone.png" alt="1" /></sup><br />
				<br />
				<br />
				<br />
			</td>
			<td>
				SYD01<br />
				SYD04<br />
				MEL01<br />
				<br /><br /><br /><br /><br /><br />
			</td>
		</tr>
		<tr>
			<td colspan="100%"><p><sup><img src="/images/numberone.png" alt="1" /></sup>Os data centers nessas regiões
ou especificamente aqueles observados em uma região NÃO têm um armazenamento criptografado.<br /><strong>Nota</strong>:
os datacenters com armazenamento criptografado <strong>não podem</strong> iniciar a replicação com data centers
não criptografados como destinos de réplica.<br />Na região Ásia-Pacífico, o CHE01 pode iniciar a replicação
para data centers com armazenamento criptografado como réplicas (data centers acima da linha).</p>
			</td>
		</tr>
	</tbody>
</table>
 

## Como criar uma replicação inicial?

As replicações trabalham fora de um planejamento de captura instantânea. Deve-se primeiramente ter espaço
de captura instantânea e um planejamento de captura instantânea configurado para o volume de origem para
poder replicar. Você receberá prompts informando o espaço que precisa ser comprado ou
que um planejamento precisa ser configurado, caso você tente configurar a replicação e uma ou outra não está em
vigor. As replicações são gerenciadas no Armazenamento, no {{site.data.keyword.blockstorageshort}} ou
no Armazenamento de arquivo do
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu volume de armazenamento.
2. Clique na guia **Réplica** e clique no link **Comprar uma
replicação**.
Selecione um planejamento de captura instantânea existente que você deseja que suas replicações sigam.
A lista conterá todos os seus planejamentos de captura instantânea ativos. <br />
  **Nota:** é possível selecionar um planejamento, mesmo se você possui uma combinação de
planejamentos por hora, diários e semanais. Todas as capturas instantâneas capturadas desde o ciclo de
replicação anterior serão replicadas, independentemente do planejamento que as originou.<br />
  **Nota:** se você não tiver capturas instantâneas configuradas, será
solicitado a fazer isso para poder solicitar a replicação. Consulte [Trabalhando
com capturas instantâneas](snapshots.html) para obter mais detalhes.
3. Clique na seta suspensa **Local** e selecione o data center que será
o seu site DR.
4. Clique em **Continuar**.
5. Insira em um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo serão padrão.
6. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…**
e clique em **Fazer o pedido**.
 

## Como editar uma replicação existente?

É possível editar seu planejamento de replicação e mudar seu espaço de replicação na guia
**Primário** ou **Réplica** em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}**, no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

 

## Como editar um planejamento de replicação?

Na realidade, você está mudando um planejamento de captura instantânea porque seu planejamento
de replicação é baseado em um planejamento de captura instantânea existente. Para mudar o planejamento de
réplica, por exemplo, de por hora para semanal, deve-se cancelar o planejamento de replicação e
configurar um novo.

A mudança do planejamento pode ser feita na guia Primário ou Réplica.

1. Clique no menu suspenso **Ações** na guia **Primário** ou **Réplica**.
2. Selecione **Editar planejamento de captura instantânea**.
3. Procure no quadro de Captura instantânea em Planejamento para determinar qual planejamento você
está usando para replicação. Faça as mudanças no planejamento que está sendo usado para replicação; por
exemplo, se o seu planejamento de replicação for **Diário**, será possível mudar
a hora do dia em que a replicação deverá ocorrer.
4. Clique em **Salvar**.
 

## Como mudar o espaço de replicação?

Seu espaço de captura instantânea primário e seu espaço de réplica devem ser os mesmos.
Se você mudar o espaço na guia **Primário** ou **Réplica**, ela
incluirá espaço automaticamente nos seus data centers de origem e de destino. O aumento do espaço de captura instantânea acionará uma atualização de replicação imediata.

Clique no menu suspenso **Ações** na guia **Primário** ou **Réplica**.
Selecione **Incluir mais espaço de captura instantânea**.
Selecione o tamanho de armazenamento na lista e clique em **Continuar**.
Insira em um **Código promocional** se você tiver um e clique em **Recalcular**. Os outros campos na caixa de diálogo serão padrão.
Clique na caixa de seleção Eu li o Contrato de Prestação de Serviços Principal… e clique no botão Fazer
o pedido.
 

## Como ver meus volumes de réplica na lista de volumes?

É possível visualizar seus volumes de replicação na página do
{{site.data.keyword.blockstorageshort}}
em **Armazenamento > {{site.data.keyword.blockstorageshort}}**. O nome do LUN terá o
nome do volume primário seguido por REP. O tipo é Resistência (Desempenho) - Réplica, o endereço de destino é
N/A porque o volume de réplica não está montado no data center de réplica e o status é Inativo.

 

## Como visualizar os detalhes de um volume replicado no data center de réplica?

É possível visualizar os detalhes do volume de réplica na guia **Réplica** em
**Armazenamento**, **{{site.data.keyword.blockstorageshort}}**. 
Outra opção é selecionar o volume de réplica na página do
**{{site.data.keyword.blockstorageshort}}** e clicar na
guia **Réplica**.

 

## Como especificar autorizações de host antes de efetuar failover para o data center secundário?

Os hosts e volumes autorizados devem estar no mesmo data center. Não é possível ter um volume de réplica
em Londres e o host em Amsterdã; ambos devem estar em Londres ou em Amsterdã.

1. Clique em seu volume de origem ou de destino na página
**{{site.data.keyword.blockstorageshort}}**.
2. Clique na guia **Réplica**.
3. Role até o quadro **Autorizar hosts** e clique no link **Autorizar
hosts** no lado direito da tela.
4. Destaque o host que deve ser autorizado para replicações. Para selecionar múltiplos hosts, use a
tecla CTRL e clique nos hosts aplicáveis.
5. Clique em **Enviar**. Se você não tiver hosts, a caixa de diálogo oferecerá a
opção para comprar recursos de cálculo no mesmo data center ou é possível clicar em
**Fechar**.
 

## Como aumentar meu espaço de captura instantânea em meu data center de réplica ao aumentar espaço
em meu data center primário?

Seus tamanhos de volume devem ser os mesmos para seus volumes de armazenamento primários e de réplica, ou
seja, um não pode ser maior que o outro. Ao aumentar o espaço de captura instantânea para o volume
primário, o espaço de réplica é aumentado automaticamente. O aumento do espaço de captura instantânea acionará uma atualização de replicação imediata. 
O aumento de ambos os volumes será mostrado como itens de linha em sua fatura e será rateado conforme
necessário.

Clique [aqui](snapshots.html) para saber como aumentar o espaço de captura instantânea.

 

## Como iniciar um failover de um volume para sua réplica?

No caso de um evento de falha, a ação **Failover** permite iniciar um failover
para seu volume de destino. O volume de destino se torna ativo, a última captura instantânea replicada com
sucesso é ativada e o volume é ativado para montagem. Quaisquer dados gravados no volume de origem desde
o ciclo de replicação anterior serão destruídos. Lembre-se de que quando um failover é iniciado, o
**relacionamento de replicação se inverte**. O volume de destino é agora o volume de
origem e o volume de origem antigo se torna o destino, conforme indicado pelo **Nome do
LUN** seguido por **REP**.

Os failovers são iniciados em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** no
[[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Antes de continuar com esse processo, é recomendável desconectar o volume. A falha ao fazer
isso resulta em distorção e/ou em perda de dados.**

1. Clique em seu LUN ativo (“origem”).
2. Clique na guia **Réplica** e clique no link **Ações** no canto superior direito.
3. Selecione Failover. Você receberá uma mensagem na parte superior da página informando que o failover está em andamento. Além disso, um ícone aparecerá próximo ao seu volume no **{{site.data.keyword.blockstorageshort}}** indicando que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparecerá quando a transação estiver concluída. 
Durante o processo de failover, as ações relacionadas à configuração são somente leitura; não é possível
editar qualquer planejamento de captura instantânea, mudar o espaço de captura instantânea e assim
por diante. O evento é registrado no histórico de replicação.
   Outra mensagem informará quando seu volume de destino está ativo. O Nome do LUN do seu volume de origem
original será seguido por REP e seu status será Inativo.
4. Clique no link de Armazenamento **Visualizar tudo**
(**{{site.data.keyword.blockstorageshort}}**) no canto superior direito.
5. Clique em seu LUN ativo (anteriormente seu volume de destino). Esse volume terá agora um status **Ativo**.
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](provisioning-block_storage.html) para obter instruções.
 

## Como iniciar um fallback de um volume para sua réplica?

Quando o volume de origem original é reparado, a ação **Fallback** permite o
início de um fallback controlado para seu volume de origem original. Em um fallback controlado,

- O volume de origem interino é colocado off-line.
- Uma captura instantânea é obtida;
- O ciclo de replicação é concluído;
- A captura instantânea de dados recém-obtida é ativada e;
- O volume de origem é ativado para montagem.

Lembre-se de que quando um fallback é iniciado, o **relacionamento de replicação se inverte
novamente**. Seu volume de origem é restaurado como seu volume de origem e seu volume de destino
novamente é o volume de destino, conforme indicado pelo **Nome do LUN** seguido por
**REP**.

Os fallbacks são iniciados em **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** no
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Clique em seu LUN de Resistência ativo (“destino”).
2. Clique na guia **Réplica** e clique no link **Ações** no canto superior direito.
3. Selecione **Fallback**.
   Você receberá uma mensagem na parte superior da página informando que o failover está em andamento. Além disso, um ícone aparecerá próximo ao seu volume no **{{site.data.keyword.blockstorageshort}}** indicando que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz um diálogo indicando a transação. O ícone desaparecerá quando a transação estiver concluída. 
Durante o processo de fallback, as ações relacionadas à configuração são somente leitura; não é possível
editar qualquer planejamento de captura instantânea, mudar o espaço de captura instantânea e assim por diante. O evento é registrado no histórico de replicação.
   Outra mensagem informará quando seu volume de origem está ativo. Seu volume de destino terá agora um
status Inativo.
4. Clique no link **Visualizar tudo**
(**{{site.data.keyword.blockstorageshort}}**) no canto superior direito.
5. Clique em seu LUN de Resistência ativo (origem). Esse volume terá agora um status **Ativo**.
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](provisioning-block_storage.html) para obter instruções.
 

## Como ver meu histórico de replicação?

O histórico de replicação é exibido no **Log de auditoria** por meio da
guia **Conta** em **Gerenciar**. Os volumes primários e de réplica
exibirão o histórico de replicação idêntico, que inclui:

- O tipo de replicação (failover ou fallback)
- Quando ele foi iniciado
- Captura instantânea usada para a replicação
- Tamanho da replicação
- Quando é concluído
 

## Como cancelar uma replicação existente?

O cancelamento pode ser feito imediatamente ou na data de aniversário, que faz com que o faturamento
seja finalizado. A replicação pode ser cancelada nas guias **Primário** ou
**Réplica**.

1. Clique no volume na página **{{site.data.keyword.blockstorageshort}}**.
2. Clique no menu suspenso **Ações** na guia **Primário** ou
**Réplica**.
3. Selecione **Cancelar réplica**.
4. Selecione quando cancelar, **Imediatamente** ou **Data
de aniversário** e clique em **Continuar**.
5. Clique na caixa de seleção **Eu reconheço que pode ocorrer perda de
dados devido ao cancelamento** e clique em **Cancelar réplica**.
 

## Como cancelar a replicação quando o volume primário é cancelado?

Quando um volume primário é cancelado, o planejamento de replicação e o volume no data center de réplica
são excluídos. As réplicas são canceladas na página {{site.data.keyword.blockstorageshort}}.

 1. Destaque seu volume na página **{{site.data.keyword.blockstorageshort}}**.
 2. Clique no menu suspenso **Ações** e selecione **Cancelar
{{site.data.keyword.blockstorageshort}}**.
 3. Selecione quando cancelar o volume, **Imediatamente** ou
**Data de aniversário** e clique em **Continuar**.
 4. Clique na caixa de seleção **Eu reconheço que pode ocorrer perda
de dados devido ao cancelamento** e clique em **Cancelar**.
