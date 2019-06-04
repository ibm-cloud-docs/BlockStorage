---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, accessible Primary volume, duplicate of a replica volume, Disaster Recovery, volume duplication, replication, failover, failback

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Recuperação de desastre: failover com um volume primário acessível
{: #dr-accessible}

Se uma falha catastrófica ou desastre ocorrer no site primário e o armazenamento primário ainda estiver acessível, os clientes poderão executar as ações a seguir para acessar rapidamente seus dados no site secundário.

Antes de iniciar o failover, certifique-se de que toda a autorização de host esteja estabelecida.

Os hosts e volumes autorizados devem estar no mesmo data center. Por exemplo, não é possível ter um volume de réplica em Londres e o host em Amsterdã. Ambos devem estar em Londres ou ambos devem estar em Amsterdã.
{:note}

1. Efetue login [no console do {{site.data.keyword.cloud}} ](https://{DomainName}/catalog){: external} e clique no ícone de **menu** na parte superior esquerda. Selecione **Infraestrutura clássica**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.
2. Clique em seu volume de origem ou de destino na página **{{site.data.keyword.blockstorageshort}}**.
3. Clique em **Réplica**.
4. Role para baixo para o quadro **Autorizar hosts** e clique em **Autorizar hosts** à direita.
5. Destaque o host que deve ser autorizado para replicações. Para selecionar múltiplos hosts, use a tecla CTRL e clique nos hosts aplicáveis.
6. Clique em **Enviar**. Se você não tiver hosts disponíveis, receberá uma solicitação para adquirir recursos de cálculo no mesmo data center.


## Iniciando um failover de um volume para sua réplica

Se um evento de falha for iminente, será possível iniciar um **failover** para seu volume alvo ou de destino. O volume de destino torna-se ativo. A última captura instantânea replicada com êxito é ativada e o volume é disponibilizado para montagem. Todos os dados que foram gravados no volume de origem desde que o ciclo de replicação anterior foi perdido. Quando um failover é iniciado, o relacionamento de replicação é invertido. O volume de destino torna-se o volume de origem e o volume de origem antigo torna-se o destino, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failovers são iniciados em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

**Antes de continuar com essas etapas, desconecte o volume. A falha ao fazer isso resultará em distorção e perda de dados.**

1. Clique em seu LUN ativo (“origem”).
2. Clique em **Réplica** e clique em **Ações**.
3. Selecione  ** Failover **.

   Espere uma mensagem na página indicando que o failover está em andamento. Além disso, um ícone aparece ao lado do seu volume no **{{site.data.keyword.blockstorageshort}}**, o que indica que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída. Durante o processo de failover, as ações relacionadas à configuração são somente leitura. Não é possível editar qualquer planejamento de captura instantânea nem mudar o espaço de captura instantânea. O evento é registrado no histórico de replicação.<br/> Quando seu volume de destino estiver ativo, você obterá outra mensagem. O nome do LUN do volume de origem original é atualizado para terminar em "REP" e seu Status se torna Inativo.
   {:note}
4. Clique em **Visualizar todos ({{site.data.keyword.blockstorageshort}})**.
5. Clique em seu LUN ativo (anteriormente seu volume de destino).
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) para obter instruções.


## Iniciando um failback de um volume para sua réplica

Quando seu volume de origem original é reparado, é possível iniciar um Failback controlado para ele. Em um failback controlado,

- O volume de origem em ação é colocado off-line,
- Uma captura instantânea é obtida,
- O ciclo de replicação está concluído,
- A captura instantânea de dados apenas obtida é ativada,
- E o volume de origem torna-se ativo para montagem.

Quando um Failback é iniciado, o relacionamento de replicação é invertido novamente. Seu volume de origem é restaurado como seu volume de origem e seu volume de destino é o volume de destino novamente, conforme indicado pelo **Nome do LUN** seguido por **REP**.

Os failbacks são iniciados em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}.

1. Clique em seu LUN ativo ("target").
2. Na parte superior direita, clique em **Réplica** e clique em **Ações**.
3. Selecione **Failback**.

   Espere uma mensagem na página mostrando que o failover está em andamento. Além disso, um ícone aparece ao lado do seu volume no **{{site.data.keyword.blockstorageshort}}**, o que indica que uma transação ativa está ocorrendo. Passar o mouse sobre o ícone produz uma janela que mostra a transação. O ícone desaparece quando a transação está concluída. Durante o processo de Failback, as ações relacionadas à configuração são somente leitura. Não é possível editar qualquer planejamento de captura instantânea nem mudar o espaço de captura instantânea. O evento é registrado no histórico de replicação.
   {:note}
4. No canto superior direito, clique no link **Visualizar todo o {{site.data.keyword.blockstorageshort}}**.
5. Clique em seu LUN ativo ("origem").
6. Monte e conecte o seu volume de armazenamento no host. Clique [aqui](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole) para obter instruções.
