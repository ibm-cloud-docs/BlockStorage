---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Fazendo upgrade do {{site.data.keyword.blockstorageshort}} existente para o {{site.data.keyword.blockstorageshort}} aprimorado

O {{site.data.keyword.blockstoragefull}} aprimorado está agora disponível nos data centers selecionados. Para ver a lista de data centers submetidos a upgrade e recursos disponíveis, como taxas de IOPS ajustáveis e volumes expansíveis, clique [aqui](new-ibm-block-and-file-storage-location-and-features.html). Para obter mais informações sobre armazenamento criptografado gerenciado por provedor, leia o artigo [Criptografia em repouso do {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html).

O caminho de migração preferencial é conectar-se aos dois LUNs simultaneamente e transferir dados diretamente de um LUN para outro. Os detalhes dependerão de seu sistema operacional e se os dados são esperados mudar durante a operação de cópia. 

Supõe-se que seu LUN não criptografado já esteja conectado ao seu host. Se não, siga as instruções que se ajustam melhor ao seu sistema operacional para realizar essa tarefa:

- [Acessando o {{site.data.keyword.blockstorageshort}} no Linux](accessing_block_storage_linux.html)
- [Acessando o {{site.data.keyword.blockstorageshort}} no Windows](accessing-block-storage-windows.html)

 
## Criar novo {{site.data.keyword.blockstorageshort}}

**IMPORTANTE**: ao fazer um pedido com a API, especifique o pacote "Armazenamento como um Serviço" para assegurar que você esteja recebendo os recursos atualizados com seu novo armazenamento.

As instruções a seguir são para pedir um LUN aprimorado por meio da IU. Seu novo LUN deve ser do mesmo tamanho ou maior que o volume original para facilitar a migração.

### Pedir um LUN do Endurance

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. No canto superior direito, clique em **Pedir o {{site.data.keyword.blockstorageshort}}**.
3. Selecione **Endurance** na lista **Selecionar tipo de armazenamento**.
4. Selecione seu **Local** de implementação (data center).
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que o volume anterior.
5. Selecione sua opção de faturamento. É possível escolher entre faturamento por hora e mensal.
6. Selecione a camada de IOPS.
7. Clique em *Selecionar tamanho de armazenamento** e selecione seu tamanho de armazenamento na lista.
8. Clique em **Especificar tamanho do espaço de captura instantânea** e selecione o tamanho da captura instantânea na lista. Isso é além de seu espaço utilizável. Para obter considerações e recomendações sobre espaço de captura instantânea, leia [Pedindo capturas instantâneas](ordering-snapshots.html).
9. Escolha seu **Tipo de S.O.** na lista.
10. Clique em **Continuar**. Serão exibidos encargos mensais e rateados
com uma chance final para revisar os detalhes do pedido.
11. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique em **Fazer pedido**.

### Pedir um LUN de desempenho

1. No [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}, clique em **Armazenamento**, **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura > Armazenamento > {{site.data.keyword.blockstorageshort}}**.
2. No canto superior direito, clique em **Pedir o {{site.data.keyword.blockstorageshort}}**.
3. Selecione **Desempenho** na lista suspensa **Selecionar tipo de
armazenamento**.
4. Clique na lista suspensa **Local** e selecione seu data center.
   - Assegure-se de que o novo Armazenamento seja incluído no mesmo local que os hosts pedidos anteriormente.
5. Selecione sua opção de faturamento. É possível escolher entre faturamento por hora e mensal.
6. Selecione o botão de opções ao lado do **Tamanho de armazenamento** apropriado.
7. Insira o número de IOPS no campo **Especifique as IOPS**.
8. Clique em **Continuar**. Serão exibidos os encargos mensais e rateados
com uma chance final para revisar os detalhes do pedido. Clique em **Anterior** se você
desejar mudar seu pedido.
9. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal** e clique no botão **Fazer pedido.


O armazenamento será provisionado em menos de um minuto e estará visível na página {{site.data.keyword.blockstorageshort}} do {{site.data.keyword.slportal}}.


 
## Conectar o novo {{site.data.keyword.blockstorageshort}} ao host

Hosts "autorizados" são hosts que receberam direitos de acesso a um volume. Sem autorização do host, você não será capaz de acessar ou usar o armazenamento de seu sistema. A autorização de um host para acesso ao seu volume gera o nome do usuário, a senha e o nome qualificado de iSCSI (IQN), que são necessários para montar a conexão iSCSI de multipath I/O (MPIO).

1. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** e clique em seu Nome do LUN.

2. Role para **Hosts autorizados**.

3. À direita, clique em **Autorizar host**. Selecione os hosts que podem acessar o volume.

 
## Capturas instantâneas e replicação

Você tem capturas instantâneas e replicação estabelecidas para o seu LUN original? Se sim, será necessário configurar a replicação, o espaço de captura instantânea e criar planejamentos de captura instantânea para o novo LUN com as mesmas configurações que o volume original. 

Observe que, se o seu data center de destino de replicação não tiver sido submetido a upgrade para criptografia, você não será capaz de estabelecer a replicação para o novo volume até que o data center seja submetido a upgrade.

 
## Migre seus dados

É necessário que você esteja conectado aos seus LUNs original e novo do {{site.data.keyword.blockstorageshort}}. 
- Se você precisar de assistência com a conexão dos dois LUNs ao seu host, abra um chamado de suporte.

### Considerações de dados

Neste ponto, é necessário considerar o tipo de dados que você possui no seu LUN original do {{site.data.keyword.blockstorageshort}} e a melhor maneira de copiá-los para seu novo LUN. Se você possui backups, conteúdo estático e itens que não deverão mudar durante a cópia, não há maiores considerações.

Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.blockstorageshort}}, certifique-se de que os dados não sejam alterados durante a cópia para evitar distorção de dados. Se você tiver preocupações com relação à largura da banda, será necessário executar a migração durante os horários fora de pico. Se precisar de assistência com essas considerações, abra um chamado de suporte.
 
### Microsoft Windows

Para copiar dados de seu LUN original do {{site.data.keyword.blockstorageshort}} para o seu novo LUN, formate o novo armazenamento e copie os arquivos usando o Windows Explorer.

 
### Linux

Você pode considerar o uso de 'rsync' para copiar sobre os dados. Abaixo está um exemplo de comando:

```
[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/new/block/storage
```

É recomendado usar o comando acima com a sinalização `--dry-run` uma vez para garantir que os caminhos sejam alinhados corretamente. Se esse processo for interrompido, você talvez deseje excluir o último arquivo de destino que estava sendo copiado para assegurar que ele seja copiado do início no novo local.

Quando esse comando é concluído sem a sinalização `--dry-run`, seus dados devem ser copiados para o novo LUN do {{site.data.keyword.blockstorageshort}}. Role para cima e execute o comando novamente para ter certeza de que nada foi perdido. Também é possível revisar localmente ambos os locais para procurar por algo que possa estar ausente.

Quando a migração for concluída, você será capaz de mover a produção para o novo LUN. Em seguida, será possível separar e excluir o LUN original da sua configuração. Observe que a exclusão também remove qualquer captura instantânea ou réplica no site de destino que estava associada ao LUN original.
