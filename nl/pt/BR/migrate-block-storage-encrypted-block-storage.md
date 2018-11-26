---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Fazendo upgrade do {{site.data.keyword.blockstorageshort}} existente para o {{site.data.keyword.blockstorageshort}} aprimorado

O {{site.data.keyword.blockstoragefull}} aprimorado está agora disponível nos data centers selecionados. Para ver a lista de data centers submetidos a upgrade e recursos disponíveis, como taxas de IOPS ajustáveis e volumes expansíveis, clique [aqui](new-ibm-block-and-file-storage-location-and-features.html). Para obter mais informações sobre o armazenamento criptografado gerenciado pelo provedor, consulte [{{site.data.keyword.blockstorageshort}}Criptografia em repouso](block-file-storage-encryption-rest.html).

O caminho de migração preferencial é conectar-se aos dois LUNs simultaneamente e transferir dados diretamente de um LUN para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia.

Supõe-se que seu LUN não criptografado já esteja conectado ao seu host. Se não, siga as instruções que se ajustam melhor ao seu sistema operacional para realizar essa tarefa:

- [Conectando-se a LUNs iSCSI de MPIO no Linux](accessing_block_storage_linux.html)
- [Conectando-se a LUNs do iSCSI de MPIO no CloudLinux](configure-iscsi-cloudlinux.html)
- [Conectando-se às LUNs iSCSI de MPIO no Microsoft Windows](accessing-block-storage-windows.html)

## Criando novo  {{site.data.keyword.blockstorageshort}}

Ao fazer um pedido com a API, especifique o pacote "Armazenamento como um serviço" para assegurar-se de que esteja obtendo os recursos atualizados com seu novo armazenamento.
{:important}

As instruções a seguir são para pedir um LUN aprimorado por meio do {{site.data.keyword.slportal}}. Seu novo LUN deve ser do mesmo tamanho ou maior que o volume original para facilitar a migração.

### Pedindo um LUN do Endurance

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. No canto superior direito, clique em **Pedir o {{site.data.keyword.blockstorageshort}}**.
3. Selecione **Endurance** na lista **Selecionar tipo de armazenamento**.
4. Selecione seu **Local** de implementação (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o volume anterior.
5. Selecione sua opção de faturamento. É possível escolher entre faturamento por hora e mensal.
6. Selecione a camada de IOPS.
7. Clique em **Selecionar tamanho de armazenamento** e selecione seu tamanho de armazenamento na lista.
8. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Esse espaço complementa o seu espaço utilizável.
   Para obter mais informações sobre as considerações e as recomendações de espaço de captura instantânea, consulte
[Pedindo capturas instantâneas](ordering-snapshots.html).
   {:tip}
9. Escolha seu **Tipo de S.O.** na lista.
10. Clique em **Continuar**. Serão exibidos encargos mensais e rateados
com uma chance final para revisar os detalhes do pedido.
11. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principais** e clique em **Fazer pedido**.

### Solicitando um LUN de Desempenho

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. À direita, clique em **Pedir {{site.data.keyword.blockstorageshort}} **.
3. Selecione **Performance** na lista **Selecionar tipo de armazenamento**.
4. Clique em **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o host ou hosts pedidos anteriormente.
5. Selecione sua opção de faturamento. É possível escolher entre faturamento por hora e mensal.
6. Selecione o  ** Tamanho de armazenamento ** apropriado.
7. Insira o número de IOPS no campo **Especifique as IOPS**.
8. Clique em **Continuar**. Serão exibidos os encargos mensais e rateados
com uma chance final para revisar os detalhes do pedido. Clique em **Anterior** se você
desejar mudar seu pedido.
9. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços principal** e clique em **Fazer pedido**.

O armazenamento é provisionado em menos de um minuto e é visível na página {{site.data.keyword.blockstorageshort}} do {{site.data.keyword.slportal}}.



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
  - Se você tiver backups, conteúdo estático e coisas que não devem mudar durante a cópia, não haverá grandes preocupações.
  - Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.blockstorageshort}}, certifique-se de que os dados não sejam alterados durante a cópia para evitar distorção de dados. Se você tiver alguma preocupação com a largura de banda, faça a migração durante os horários fora de pico. Se precisar de assistência com essas considerações, abra um chamado de suporte.

3. Copie os dados em.
   - **Microsoft Windows** - Para copiar dados do LUN original do {{site.data.keyword.blockstorageshort}} para seu novo LUN, formate o novo armazenamento e copie os arquivos usando o Windows Explorer.
   - **Linux** - É possível usar `rsync` para copiar os dados. Este é um exemplo:
   ```
   [root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
   ```

   É uma boa ideia usar o comando anterior com a sinalização `--dry-run` uma vez para certificar-se de que os caminhos sejam alinhados corretamente. Se esse processo for interrompido, será possível excluir o último arquivo de destino que estava sendo copiado para certificar-se de que ele seja copiado para o novo local do início.<br/>
   Quando esse comando for concluído sem a sinalização `--dry-run`, seus dados serão copiados para o novo LUN do {{site.data.keyword.blockstorageshort}}. Execute o comando novamente para certificar-se de que nada foi perdido. Também é possível revisar manualmente ambos os locais para procurar qualquer coisa que possa estar ausente.<br/>
   Quando a migração estiver concluída, será possível mover a produção para o novo LUN. Em seguida, será possível separar e excluir o LUN original da sua configuração. A exclusão também remove qualquer captura instantânea ou réplica no site de destino que tenha sido associada ao LUN original.
