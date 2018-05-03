---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Migrando o {{site.data.keyword.blockstorageshort}} para o {{site.data.keyword.blockstorageshort}} criptografado

O {{site.data.keyword.blockstoragefull}} criptografado para Resistência ou Desempenho agora é data centers de seleção disponíveis. Abaixo você encontrará informações sobre como migrar seu {{site.data.keyword.blockstorageshort}} de não criptografado para criptografado. Para obter mais informações sobre o armazenamento criptografado gerenciado por provedor, leia o artigo [Criptografia de dados em repouso do {{site.data.keyword.blockstorageshort}}](block-file-storage-encryption-rest.html). Para ver uma lista de data centers submetidos a upgrade e de recursos disponíveis, clique [aqui](new-ibm-block-and-file-storage-location-and-features.html).

O caminho de migração preferencial é conectar-se aos dois LUNs simultaneamente e transferir dados diretamente de um LUN de arquivo para outro. Os detalhes dependerão de seu sistema operacional e se mudanças de dados são esperadas durante a operação de cópia.

Os cenários mais comuns foram descritos para sua conveniência. Supõe-se que seu LUN de arquivo não criptografada já esteja conectado ao seu host. Se não estiver, siga as instruções abaixo que melhor se ajustam ao sistema operacional que está em execução para realizar essa tarefa.

- [Acessando o {{site.data.keyword.blockstorageshort}} no Linux](accessing_block_storage_linux.html)

- [Acessando o {{site.data.keyword.blockstorageshort}} no Windows](accessing-block-storage-windows.html)

 
## Crie um LUN criptografado

Use as etapas a seguir para criar um LUN do mesmo tamanho ou maior que esteja criptografado para facilitar o processo de migração. 
Solicite um LUN de armazenamento de Resistência criptografado

1. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** na página inicial do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** no catálogo do {{site.data.keyword.BluSoftlayer_full}}.

2. Clique no link **Solicitar o {{site.data.keyword.blockstorageshort}}** na página {{site.data.keyword.blockstorageshort}}.

3. Selecione **Resistência**.

4. Selecione o data center no qual o LUN original está localizado. <br/> **Nota**: a criptografia está disponível apenas em data centers de seleção.

5. Selecione a camada de IOPS desejada.

6. Selecione a quantia desejada de espaço de armazenamento em GBs. Para TB, 1 TB equivale a 1.000 GB e 12 TB equivalem a 12.000 GB.

7. Insira a quantidade desejada de espaço de armazenamento em GBs para capturas instantâneas.

8. Selecione o S.O. do VMware na lista suspensa.

9. Envie a ordem.

## Solicite um LUN de armazenamento de Desempenho criptografado

1. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** na página inicial do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window} OU clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** no catálogo do {{site.data.keyword.BluSoftlayer_full}}.

2. Clique em **Solicitar o {{site.data.keyword.blockstorageshort}}**.

3. Selecione **Desempenho**.

4. Selecione o data center no qual o LUN original está localizado. Observe que a criptografia está disponível apenas em data centers com um asterisco (`*`).

5. Selecione a quantia desejada de espaço de armazenamento em GBs. Para TB, 1 TB equivale a 1.000 GB e 12 TB equivalem a 12.000 GB.

6. Insira a quantia desejada de IOPS em intervalos de 100.

7. Selecione o S.O. do VMware na lista suspensa.

8. Envie a ordem.

O armazenamento será provisionado em menos de um minuto e estará visível na página {{site.data.keyword.blockstorageshort}} do {{site.data.keyword.slportal}}.

 
## Conecte-se ao novo volume de host

Hosts “autorizados” são hosts que receberam direitos de acesso a um volume. Sem a autorização do host, não é possível acessar ou usar o armazenamento no seu sistema. A autorização de um host para acesso ao seu volume gera o nome de usuário, a senha e o nome qualificado de iSCSI (IQN), que são necessários para montar a conexão iSCSI de E/S de caminhos múltiplos (MPIO).

1. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** e clique no nome do seu LUN.

2. Role para a seção **Hosts autorizados** da página.

3. Clique no link **Autorizar host** no lado direito da página. Selecione os hosts que podem acessar o volume.

 
## Capturas instantâneas e replicação

Você tem capturas instantâneas e replicação estabelecidas para o seu LUN original? Se sim, será necessário configurar a replicação, o espaço de captura instantânea e criar planejamentos de captura instantânea para o novo LUN criptografado com as mesmas configurações que o volume original. 

Observe que se seu data center de destino de replicação não foi submetido a upgrade para criptografia, não será possível estabelecer a replicação para o volume criptografado até que o data center seja submetido a upgrade.

 
## Migre seus dados

É necessário que você esteja conectado aos seus LUNs do {{site.data.keyword.blockstorageshort}} originais e criptografados. 
- Caso contrário, certifique-se de que você seguiu as etapas acima e referenciadas em outros posts corretamente. Abra um chamado de suporte para obter assistência com a conexão os dois LUNs.

### Considerações de dados

Neste ponto, você deseja considerar o tipo de dados que possui no seu LUN do {{site.data.keyword.blockstorageshort}} original e a melhor maneira de copiá-los para seu LUN criptografado. Se você possui backups, conteúdo estático e itens que não deverão mudar durante a cópia, não há maiores considerações.

Se você estiver executando um banco de dados ou uma máquina virtual em seu {{site.data.keyword.blockstorageshort}}, certifique-se de que os dados no LUN original não sejam mudados durante a cópia para que nenhuma distorção ocorra. Se você tiver preocupações com relação à largura da banda, será necessário executar a migração durante os horários fora de pico. Se você precisa de assistência com estas considerações, não hesite em abrir um chamado de suporte.
 
### Microsoft Windows

Para copiar dados de seu LUN do {{site.data.keyword.blockstorageshort}} original para o seu LUN criptografado, formate o novo armazenamento e copie os arquivos usando o Windows Explorer.

 
### Linux

É possível considerar o uso de rsync para copiar sobre os dados. Abaixo está um exemplo de comando:

``[root@server ~]# rsync -Pavzu /path/to/original/block/storage/* /path/to/encrypted/block/storage
``

É recomendado que você use o comando acima com a sinalização --dry-run uma vez para garantir que os caminhos estejam alinhados corretamente. Se esse processo for interrompido, será possível excluir o último arquivo de destino que estava sendo copiado para assegurar que ele seja copiado do início para o novo local.

Quando esse comando é concluído sem a sinalização --dry-run, seus dados devem ser copiados para o LUN do {{site.data.keyword.blockstorageshort}} criptografado. É necessário rolar para cima e executar o comando novamente para ter certeza de que nada foi perdido. Também é possível revisar localmente ambos os locais para procurar por algo que possa estar ausente.

Quando a migração for concluída, será possível mover a produção para o criptografado LUN e separar e excluir o LUN original da sua configuração. Observe que a exclusão também remove qualquer captura instantânea ou réplica no site de destino que estava associada ao LUN original.
