---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Fazendo upgrade do {{site.data.keyword.blockstorageshort}} existente para o {{site.data.keyword.blockstorageshort}} aprimorado
{: #migratestorage}

O {{site.data.keyword.blockstoragefull}} aprimorado está agora disponível nos data centers selecionados. Para ver a lista de data centers submetidos a upgrade e recursos disponíveis, como taxas de IOPS ajustáveis e volumes expansíveis, clique [aqui](/docs/infrastructure/BlockStorage?topic=BlockStorage-news). Para obter mais informações sobre o armazenamento criptografado gerenciado pelo provedor, consulte [{{site.data.keyword.blockstorageshort}}Criptografia em repouso](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

O caminho de migração preferencial é conectar-se aos dois LUNs simultaneamente e transferir dados diretamente de um LUN para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia.

A suposição é que você já tem o LUN não criptografado conectado ao seu host. Se não, siga as instruções que se ajustam melhor ao seu sistema operacional para realizar essa tarefa:

- [Conectando-se a LUNs no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [Conectando-se a LUNs no CloudLinux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Conectando-se a LUNS no Microsoft Windows](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)

Todos os volumes aprimorados do {{site.data.keyword.blockstorageshort}} provisionados nesses data centers têm um ponto de montagem diferente de volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para os dois volumes de armazenamento, é possível visualizar as informações do ponto de montagem na página **Detalhes do volume** no console. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.
{:tip}

## Criando um {{site.data.keyword.blockstorageshort}}

Ao fazer um pedido com a API, especifique o pacote "Armazenamento como um serviço" para assegurar-se de que esteja obtendo os recursos atualizados com seu novo armazenamento.
{:important}

É possível pedir um LUN aprimorado por meio do console do IBM Cloud e do {{site.data.keyword.slportal}}. Seu novo LUN deve ser do mesmo tamanho ou maior que o volume original para facilitar a migração.

- [Pedindo o {{site.data.keyword.blockstorageshort}} com Camadas IOPS predefinidas (Endurance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-pre-defined-iops-tiers-endurance-)
- [Pedindo o {{site.data.keyword.blockstorageshort}} com IOPS customizado (Performance)](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole#ordering-block-storage-with-custom-iops-performance-)

Seu novo armazenamento estará disponível para montagem em alguns minutos. É possível visualizá-lo
na lista de recursos e na lista do {{site.data.keyword.blockstorageshort}}.

## Conectando o novo  {{site.data.keyword.blockstorageshort}}  ao host

Hosts "autorizados" são aqueles que receberam acesso a um volume. Sem a autorização do host, não é possível acessar nem usar o armazenamento de seu sistema. A autorização de um host para acessar seu volume gera o nome do usuário, a senha e o nome qualificado de iSCSI (IQN), que são necessários para montar a conexão iSCSI de Multipath I/O (MPIO).

1. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** e depois em seu Nome do LUN.
2. Role para **Hosts autorizados**.
3. À direita, clique em **Autorizar host**. Selecione os hosts que podem acessar o volume.


## Capturas instantâneas e replicação

Você tem capturas instantâneas e replicação estabelecidas para o seu LUN original? Em caso positivo, será necessário configurar a replicação, o espaço de captura instantânea e criar planejamentos de captura instantânea para o novo LUN com as mesmas configurações que as do volume original.

Se o data center de destino de replicação ainda não tiver sido submetido a upgrade, não será possível estabelecer a replicação do novo volume até que seja feito upgrade desse data center.


## Migrando seus Dados

1. Conecte-se a ambos os LUNs do {{site.data.keyword.blockstorageshort}}, originais e novos.

   Se você precisar de assistência para conectar os dois LUNs ao host, abra um caso de suporte.
   {:tip}

2. Considere qual tipo de dados você tem no LUN original do {{site.data.keyword.blockstorageshort}} e como melhor copiá-lo para seu novo LUN.
  - Se você tem backups, conteúdo estático e coisas que não se espera que sejam mudadas
durante a cópia, não precisa se preocupar.
  - Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.blockstorageshort}}, certifique-se de que os dados não sejam alterados durante a cópia para evitar distorção de dados.
  - Se você tiver alguma preocupação com a largura de banda, faça a migração durante os horários fora de pico.
  - Se você precisar de assistência com essas considerações, abra um caso de suporte.

3. Copie os dados em.
   - Para o **Microsoft Windows**, formate o novo armazenamento e copie os dados do seu LUN original do {{site.data.keyword.blockstorageshort}} para seu novo LUN usando o Windows Explorer.
   - Para o **Linux**, é possível usar `rsync` para copiar sobre os dados.
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   É uma boa ideia usar o comando anterior com a sinalização `--dry-run` uma vez para certificar-se de que os caminhos sejam alinhados corretamente. Se esse processo for interrompido, será possível excluir o último arquivo de destino que estava sendo copiado para certificar-se de que ele seja copiado para o novo local do início.<br/>
   Quando esse comando for concluído sem a sinalização `--dry-run`, seus dados serão copiados para o novo LUN do {{site.data.keyword.blockstorageshort}}. Execute o comando novamente para certificar-se de que nada foi perdido. Também é possível revisar manualmente ambos os locais para procurar qualquer coisa que possa estar ausente.<br/>
   Quando a migração estiver concluída, será possível mover a produção para o novo LUN. Em seguida, será possível separar e excluir o LUN original da sua configuração. A exclusão também remove qualquer captura instantânea ou réplica no site de destino que tenha sido associada ao LUN original.
