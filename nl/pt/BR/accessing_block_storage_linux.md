---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-02"

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando-se a LUNs iSCSI de MPIO no Linux

Essas instruções são para o RHEL6/Centos6. Foram incluídas notas para outros S.O., mas esta documentação **não** abrange todas as distribuições Linux. Se você estiver usando outros sistemas operacionais Linux, consulte a documentação de sua distribuição específica e assegure-se de que os caminhos múltiplos suportem ALUA para prioridade de caminho. 

Por exemplo, é possível localizar instruções do Ubuntu para a Configuração do inicializador iSCSI [aqui](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} e configuração de DM-Multipath [aqui](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window}.

Antes de iniciar, certifique-se de que o host que está acessando o volume do {{site.data.keyword.blockstoragefull}} tenha sido autorizado anteriormente por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

1. Na página de listagem do {{site.data.keyword.blockstorageshort}}, localize o novo volume e clique em **Ações**.
2. Clique em **Autorizar host**.
3. Na lista, selecione os hosts que podem acessar o volume e clique em **Enviar**.

## Montando volumes do {{site.data.keyword.blockstorageshort}}

A seguir estão as etapas necessárias para conectar uma instância de Cálculo do {{site.data.keyword.BluSoftlayer_full}} baseada em Linux a um número de unidade lógica (LUN) de Small Computer System Interface (iSCSI) da internet de Multipath input/output (MPIO).

**Nota:** o IQN do host, o nome do usuário, a senha e o endereço de destino referenciados nas instruções podem ser obtidos da tela **Detalhes do {{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Nota:** é melhor executar o tráfego de armazenamento em uma VLAN, que efetua bypass do firewall. A execução do tráfego de armazenamento por meio de firewalls de software aumenta a latência e afeta negativamente o desempenho do armazenamento.

1. Instale os utilitários iSCSI e de caminhos múltiplos em seu host.
   - RHEL/CentOS

   ```
   yum install iscsi-initiator-utils device-mapper-multipath
   ```
   {: pre}

   - Ubuntu / Debian

   ```
   sudo apt-get update
   sudo apt-get install multipath-tools
   ```
   {: pre}

2. Crie ou edite o arquivo de configuração de caminhos múltiplos.
   - Edite **/etc/multipath.conf** com a configuração mínima fornecida nos comandos a seguir. <br /><br /> **Nota:** para RHEL7/CentOS7, `multipath.conf` pode ficar em branco, pois o S.O. tem configurações integradas. O Ubuntu não usa multipath.conf, pois é integrado a ferramentas de caminhos múltiplos.

   ```
   defaults {
   user_friendly_names no
   max_fds max
   flush_on_last_del yes
   queue_without_daemon no
   dev_loss_tmo infinity
   fast_io_fail_tmo 5
   }
   # All data under blacklist must be specific to your system.
   blacklist {
   wwid "SAdaptec*"
   devnode "^hd[a-z]"
   devnode "^(ram|raw|loop|fd|md|dm-|sr|scd|st)[0-9]*"
   devnode "^cciss.*"  
   }
   devices {
   device {
   vendor "NETAPP"
   product "LUN"
   path_grouping_policy group_by_prio
   features "3 queue_if_no_path pg_init_retries 50"
   prio "alua"
   path_checker tur
   failback immediate
   path_selector "round-robin 0"
   hardware_handler "1 alua"
   rr_weight uniform
   rr_min_io 128
   }
   }
   ```
   {: codeblock}

3. Carregue o módulo de caminhos múltiplos, inicie os serviços de caminhos múltiplos e configure-o para ser iniciado na inicialização.
   - RHEL 6
     ```
     modprobe dm-multipath
     ```
     {: pre}

     ```
     service multipathd start
     ```
     {: pre}

     ```
     chkconfig multipathd on
     ```
     {: pre}

   - CentOS 7
     ```
     modprobe dm-multipath
     ```
     {: pre}

     ```
     systemctl start multipathd
     ```
     {: pre}

     ```
     systemctl enable multipathd
     ```
     {: pre}

   - Ubuntu
     ```
     service multipath-tools start
     ```
     {: pre}

   - Para outras distribuições, consulte a documentação do fornecedor do S.O.

4. Verifique se os caminhos múltiplos estão funcionando.
   - RHEL 6
     ```
     multipath -l
     ```
     {: pre}

     Se ele retornar em branco, ele está funcionando.

   - CentOS 7
     ```
     multipath -ll
     ```
     {: pre}

     O RHEL 7/CentOS 7 pode retornar Nenhum dispositivo fc_host, que pode ser ignorado.

5. Atualize o arquivo `/etc/iscsi/initiatorname.iscsi` com o IQN do {{site.data.keyword.slportal}}. Insira o valor como lowercase.
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. Edite as configurações de CHAP em `/etc/iscsi/iscsid.conf` usando o nome de usuário e a senha do {{site.data.keyword.slportal}}. Use maiúscula para nomes de CHAP.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   **Nota:** deixe as outras configurações do CHAP comentadas. O armazenamento do {{site.data.keyword.BluSoftlayer_full}} usa somente autenticação unilateral. Não ativar o CHAP Mútuo

7. Configure iSCSI para ser iniciado na inicialização e inicie-o agora.
   - RHEL 6

      ```
      chkconfig iscsi on
      ```
      {: pre}
      ```
      chkconfig iscsid on
      ```
      {: pre}

      ```
      service iscsi start
      ```
      {: pre}

      ```
      service iscsid start
      ```
      {: pre}

   - CentOS 7

      ```
      systemctl enable iscsi
      ```
      {: pre}

      ```
      systemctl enable iscsid
      ```
      {: pre}

      ```
      systemctl start iscsi
      ```
      {: pre}

      ```
      systemctl start iscsid
      ```
      {: pre}

   - Outras distribuições: consulte a documentação do fornecedor do S.O.

8. Descubra o dispositivo usando o endereço IP de destino que foi obtido do {{site.data.keyword.slportal}}.

     A. Execute a descoberta com relação à matriz iSCSI.
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     B. Configure o host para efetuar login automaticamente na matriz iSCSI.
     ```
     iscsiadm -m node -L automatic
     ```
     {: pre}

9. Verifique se o host efetuou login na matriz iSCSI e manteve suas sessões.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Esse comando relata os caminhos.

10. Verifique se o dispositivo está conectado. Por padrão, o dispositivo se conecta a `/dev/mapper/mpathX`, em que X é o ID gerado do dispositivo conectado.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Esse comando relata algo semelhante ao exemplo a seguir.
    ```
    Disco /dev/mapper/3600a09803830 30 52323424457a4a695266: 73.0 GB, 73023881216 bytes
    ```
  O volume agora está montado e acessível no host.

## Criando um Sistema de Arquivos (opcional)

Siga estas etapas para criar um sistema de arquivos na parte superior do volume recém-montado. Um sistema de arquivos é necessário para que a maioria dos aplicativos use o volume. Use `fdisk` para unidades que sejam menores que 2 TB e `parted` para um disco maior que 2 TB.

### Usando o  ` fdisk `

1. Obtenha o nome do disco.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   O nome do disco retornado é semelhante a `/dev/mapper/XXX`.

2. Crie uma partição no disco.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   O XXX representa o nome do disco retornado na Etapa 1. <br />

   **Nota**: role mais para baixo para obter os códigos de comandos listados na tabela de comandos `fdisk`.

3. Crie um sistema de arquivos na nova partição.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - A nova partição é listada com o disco, semelhante a `XXXlp1`, seguido pelo tamanho, Tipo (83) e Linux.
   - Anote o nome da partição, ele será necessário na próxima etapa. (O XXXlp1 representa o nome da partição).
   - Crie o sistema de arquivos:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Crie um ponto de montagem para o sistema de arquivos e monte-o.
   - Crie um nome de partição `PerfDisk` ou o local em que você deseja montar o sistema de arquivos.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Monte o armazenamento com o nome de partição.
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifique se você vê seu novo sistema de arquivos listado.
     ```
     df -h
     ```
     {: pre}

5. Inclua o novo sistema de arquivos no arquivo `/etc/fstab` do sistema para ativar a montagem automática na inicialização.
   - Anexe a linha a seguir ao término de `/etc/fstab` (com o nome da partição da Etapa 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### A tabela de comandos  ` fdisk `

<table border="0" cellpadding="0" cellspacing="0">
	<caption>A tabela de comandos <code>fdisk</code> contém comandos à esquerda e os resultados esperados à direita.</caption>
    <thead>
	<tr>
		<th style="width:40%;">Comando</th>
		<th style="width:60%;">Resultado</th>
	</tr>
    </thead>
    <tbody>
	<tr>
		<td><code>Command: n</code></td>
		<td>Cria uma nova partição. &#42;</td>
	</tr>
	<tr>
		<td><code>Command action: p</code></td>
		<td>Torna a partição a primária.</td>
	</tr>
	<tr>
		<td><code>Partition number (1-4): 1</code></td>
		<td>Torna-se a partição 1 no disco.</td>
	</tr>
	<tr>
		<td><code>First cylinder (1-8877): 1 (default)</code></td>
		<td>Inicia com o cilindro 1.</td>
	</tr>
	<tr>
		<td><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></td>
		<td>Pressione Enter para ir para o último cilindro.</td>
	</tr>
	<tr>
		<td><code>Command: t</code></td>
		<td>Configura o tipo de partição. &#42;</td>
	</tr>
	<tr>
		<td><code>Select partition 1.</code></td>
		<td>Seleciona a partição 1 para ser configurada como um tipo específico.</td>
	</tr>
	<tr>
		<td><code>Hex code: 83</code></td>
		<td>Seleciona Linux como o Tipo (83 é o código hexadecimal para Linux).&#42;&#42;</td>
	 </tr>
	<tr>
		<td><code>Command: w</code></td>
		<td>Grava as informações da nova partição no disco. &#42;</td>
	</tr>
   </tbody>
</table>

  (`*`) Digite m para Ajuda.

  (`**`) Digite L para listar os códigos hexadecimais

### Usando  ` parted `

Em muitas distribuições Linux, `parted` vem pré-instalado. Se não estiver incluído em sua distribuição, será possível instalá-lo com:
- Debian/Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL/CentOS
  ```
  yum install parted  
  ```
  {: pre}


Para criar um sistema de arquivos com `parted`, siga estas etapas.

1. Execute  ` parted `.

   ```
   parted
   ```
   {: pre}

2. Crie uma partição no disco.
   1. A menos que seja especificado de outra forma, `parted` usa sua unidade primária, que é `/dev/sda` na maioria dos casos. Alterne para o disco que você deseja particionar usando o comando **select**. Substitua **XXX** pelo seu novo nome do dispositivo.

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. Execute `print` para confirmar que você está no disco correto.

      ```
      (parted) print
      ```
      {: pre}

   3. Crie uma tabela de partição GPT.

      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. `Parted` pode ser usado para criar partições de disco primárias e lógicas; as etapas envolvidas são as mesmas. Para criar uma partição, `parted` usa `mkpart`. É possível fornecer outros parâmetros a ele, como **primary** ou **logical**, dependendo do tipo de partição que você deseja criar.
   <br /> **Nota**: as unidades listadas são padronizadas em megabytes (MB). Para criar uma partição de 10 GB você inicia em 1 e termina em 10.000. Também é possível mudar as unidades de dimensionamento para terabytes inserindo `(parted) unit TB`, se desejar.

      ```
      (parted) mkpart
      ```
      {: pre}

   5. Saia  ` parted `  com  ` quit `.

      ```
      (parted) quit
      ```
      {: pre}

3. Crie um sistema de arquivos na nova partição.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **Nota**: é importante selecionar o disco e a partição corretos ao executar esse comando!
   Verifique o resultado por imprimir a tabela de partição. Na coluna do sistema de arquivos, é possível ver ext3.

4. Crie um ponto de montagem para o sistema de arquivos e monte-o.
   - Crie um nome de partição `PerfDisk` ou o local em que você deseja montar o sistema de arquivos.

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Monte o armazenamento com o nome de partição.

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifique se você vê seu novo sistema de arquivos listado.

     ```
     df -h
     ```
     {: pre}

5. Inclua o novo sistema de arquivos no arquivo `/etc/fstab` do sistema para ativar a montagem automática na inicialização.
   - Anexe a linha a seguir ao término de `/etc/fstab` (usando o nome da partição da Etapa 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## Verificando se o MPIO está configurado corretamente nos S.Os. `*NIX`

Para verificar se os caminhos múltiplos estão detectando os dispositivos, liste os dispositivos. Se ele estiver configurado corretamente, somente dois dispositivos NETAPP aparecerão.

```
# multipath -l
```
{: pre}

```
root@server:~# multipath -l
3600a09803830304f3124457a45757067 dm-1 NETAPP,LUN C-Mode size=20G features='1 queue_if_no_path' hwhandler='0' wp=rw
|-+- policy='round-robin 0' prio=-1 status=active`
6:0:0:101 sdd 8:48 active undef running `-+- policy='round-robin 0' prio=-1 status=enabled`
7:0:0:101 sde 8:64 active undef running
```

Verifique se os discos estão presentes. Confirme se há dois discos com o mesmo identificador e uma listagem `/dev/mapper` do mesmo tamanho com o mesmo identificador. O dispositivo `/dev/mapper` é aquele que os caminhos múltiplos configuram:

```
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/mapper/3600a09803830304f3124457a45757066: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```

Se não estiver configurado corretamente, ele será semelhante a este exemplo.
```
No multipath output root@server:~# multipath -l root@server:~#
```

Esse comando mostra os dispositivos incluídos na lista de bloqueio.
```
# multipath -l -v 3 | grep sd <date and time>
```
{: pre}

```
root@server:~# multipath -l -v 3 | grep sd Feb 17 19:55:02
| sda: device node name blacklisted Feb 17 19:55:02
| sdb: device node name blacklisted Feb 17 19:55:02
| sdc: device node name blacklisted Feb 17 19:55:02
| sdd: device node name blacklisted Feb 17 19:55:02
| sde: device node name blacklisted Feb 17 19:55:02
```

`fdisk` mostra somente os dispositivos `sd*` e nenhum `/dev/mapper`.

```
# fdisk -l | grep Disk
```
{: pre}

```
root@server:~# fdisk -l | grep Disk
Disk /dev/sda: 500.1 GB, 500107862016 bytes Disk identifier: 0x0009170d
Disk /dev/sdc: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
Disk /dev/sdb: 21.5 GB, 21474836480 bytes Disk identifier: 0x2b5072d1
```
