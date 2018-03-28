---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-09"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Conectando-se a LUNs iSCSI de MPIO no Linux

Essas instruções são para o RHEL6/Centos6. Se você estiver usando outros sistemas operacionais Linux, consulte a documentação de sua distribuição específica para configuração e assegure-se de que os caminhos múltiplos suportam ALUA para prioridade de caminho.

Antes de iniciar, verifique se o host que está acessando o volume do {{site.data.keyword.blockstoragefull}} foi autorizado por meio do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}:

1. Na pasta de listagem do {{site.data.keyword.blockstorageshort}}, clique nas **Ações** associadas ao volume recém-provisionado
2. Clique em **Autorizar host**.
3. Selecione os hosts desejados na lista e clique em **Enviar**. Isso autoriza que eles acessem o volume.

## Montando volumes do {{site.data.keyword.blockstorageshort}}

A seguir estão as etapas necessárias para conectar uma instância do {{site.data.keyword.BluSoftlayer_full}} Compute baseada em Linux a um número da unidade lógica (LUN) Internet Small Computer System Interface (iSCSI) para E/S de caminhos múltiplos (MPIO).


O exemplo é baseado no **Red Hat Enterprise Linux 6**. As etapas devem ser ajustadas para outras distribuições do Linux de acordo com a documentação do fornecedor do sistema operacional (S.O.). Incluímos notas para outro S.O., mas esta documentação **não** cobre todas as distribuições do Linux. Por exemplo, no caso de Ubuntu, clique [aqui](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html){:new_window:} para obter instruções de configuração do Inicializador iSCSI e clique [aqui](https://help.ubuntu.com/lts/serverguide/multipath-setting-up-dm-multipath.html){:new_window} para obter mais informações sobre a configuração de DM-Multipath.

**Nota:** o IQN do host, o nome do usuário e o endereço de destino referenciados nas instruções podem ser obtidos na tela **Detalhes do {{site.data.keyword.blockstorageshort}}** no [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.

**Nota:** é recomendável executar o tráfego de armazenamento em uma VLAN que ignora o firewall como uma boa prática. Executar o tráfego de armazenamento através de firewalls de software aumentará a latência e afetará adversamente o desempenho de armazenamento.

1. Instale os utilitários iSCSI e de caminhos múltiplos no seu host:
   - RHEL/CentOS:

   ```
   yum install iscsi-initiator-utils device-mapper-multipath
   ```
   {: pre}

   - Ubuntu/Debian:

   ```
   sudo apt-get update
   sudo apt-get install multipath-tools
   ```
   {: pre}

2. Crie ou edite o arquivo de configuração de caminhos múltiplos.
   - Edite **/etc/multipath.conf** com a configuração mínima fornecida nos seguintes comandos. <br /><br /> **Nota:** lembre-se de que para RHEL7/CentOS7, `multipath.conf` pode ficar em branco, já que o S.O. possui configurações integradas. O Ubuntu não usa multipath.conf, já que ele é integrado em ferramentas de caminhos múltiplos.

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

3. Carregue o módulo de caminhos múltiplos, inicie os serviços de caminhos múltiplos e configure o módulo para que seja ativado na inicialização.
   - RHEL 6:
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

   - CentOS 7:
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

   - Ubuntu:
     ```
     service multipath-tools start
     ```
     {: pre}

   - Para outras distribuições, consulte a documentação do fornecedor do S.O.

4. Verifique se os caminhos múltiplos estão funcionando.
   - RHEL 6:
     ```
     multipath -l
     ```
     {: pre}

     Se ele retorna em branco neste momento, ele está funcionando.

   - CentOS 7:
     ```
     multipath -ll
     ```
     {: pre}

     O RHEL 7/CentOS 7 pode retornar Nenhum dispositivo fc_host, que pode ser ignorado.

5. Atualize o arquivo **/etc/iscsi/initiatorname.iscsi** com o IQN do {{site.data.keyword.slportal}}. Digite o valor em minúsculas.
   ```
   InitiatorName=<value-from-the-Portal>
   ```
   {: pre}
6. Edite as configurações do CHAP no **/etc/iscsi/iscsid.conf** usando o nome de usuário e a senha do {{site.data.keyword.slportal}}. Use maiúsculas para nomes do CHAP.
   ```
    node.session.auth.authmethod = CHAP
    node.session.auth.username = <Username-value-from-Portal>
    node.session.auth.password = <Password-value-from-Portal>
    discovery.sendtargets.auth.authmethod = CHAP
    discovery.sendtargets.auth.username = <Username-value-from-Portal>
    discovery.sendtargets.auth.password = <Password-value-from-Portal>
   ```
   {: codeblock}

   **Nota:** deixe as outras configurações do CHAP comentadas. O armazenamento do {{site.data.keyword.BluSoftlayer_full}} usa somente autenticação unilateral.

7. Configure o iSCSI para que seja ativado na inicialização e inicie-o neste momento.
   - RHEL 6:

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

   - CentOS 7:

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

8. Descubra o dispositivo usando o endereço IP de destino obtido do {{site.data.keyword.slportal}}.

     a. Execute a descoberta com relação à matriz iSCSI:
     ```
     iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
     ```
     {: pre}

     b. Configure o host para efetuar login automaticamente na matriz do iSCSI:
     ```
     iscsiadm -m node -L automatic
     ```
     {: pre}

9. Verifique se o host efetuou login na matriz iSCSI e se manteve suas sessões.
   ```
   iscsiadm -m session
   ```
   {: pre}

   ```
   multipath -l
   ```
   {: pre}

   Isto deve relatar os caminhos neste momento.

10. Verifique se o dispositivo está conectado.  Por padrão, o dispositivo se conectará a /dev/mapper/mpathX, em que X é o ID gerado do dispositivo conectado.
    ```
    fdisk -l | grep /dev/mapper
    ```
    {: pre}
  Algo semelhante ao seguinte deverá ser relatado,
    ```
    Disk /dev/mapper/3600a0980383030523424457a4a695266: 73.0 GB, 73023881216 byte
    ```
  O volume agora está montado e acessível no host.

## Crie um sistema de arquivos (opcional)

A seguir estão as etapas para criar um sistema de arquivos sobre o volume recém-montado. Um sistema de arquivos é necessário para que a maioria dos aplicativos use o volume. Use **fdisk** para unidades que sejam menores que 2 TB e **parted** para um disco maior que 2 TB.

### fdisk

1. Obtenha o nome do disco.
   ```
   fdisk -l | grep /dev/mapper
   ```
   {: pre}
   O nome do disco retornado deve ser semelhante a /dev/mapper/XXX.

2. Crie uma partição no disco usando fdisk.

   ```
   fdisk /dev/mapper/XXX
   ```
   {: pre}

   O XXX representa o nome do disco retornado na Etapa 1. <br />

   **Nota**: role ainda mais para baixo até os códigos de comandos listados na tabela de comando Fdisk nesta seção.

3. Crie um sistema de arquivos na nova partição.

   ```
   fdisk –l /dev/mapper/XXX
   ```
   {: pre}

   - A nova partição deverá ser listada com o disco, semelhante a XXXlp1, seguido pelo tamanho, Tipo (83) e Linux.
   - Anote o nome da partição, pois você precisará dele na próxima etapa. (O XXXlp1 representa o nome da partição).
   - Crie o sistema de arquivos:

     ```
     mkfs.ext3 /dev/mapper/XXXlp1
     ```
     {: pre}

4. Crie um ponto de montagem para o sistema de arquivos e monte-o.
   - crie um nome de partição PerfDisk ou no local em que você desejar montar o sistema de arquivos:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Usando o nome de partição monte o armazenamento:
     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifique se você vê o seu novo sistema de arquivos listado:
     ```
     df -h
     ```
     {: pre}

5. Inclua o novo sistema de arquivos no arquivo de sistema **/etc/fstab** para ativar a montagem automática na inicialização.
   - Anexe a linha a seguir na parte inferior de **/etc/fstab** (usando o nome de partição da Etapa 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults,_netdev    0    1
     ```
     {: pre}

#### Tabela de comandos Fdisk
<table border="0" cellpadding="0" cellspacing="0">
 <tbody>
	<tr>
		<td style="width:40%;"><div>Comando</div></td>
		<td style="width:60%;">Resultado</td>
	</tr>
	<tr>
		<td><li><code>Command: n</code></li>	</td>
		<td>Cria uma nova partição.</td>
	</tr>
	<tr>
		<td><li><code>Command action: p</code></li></td>
		<td>Torna a partição a primária.</td>
	</tr>
	<tr>
		<td><li><code>Partition number (1-4): 1</code></li></td>
		<td>Torna-se a partição 1 no disco.</td>
	</tr>
	<tr>
		<td><li><code>First cylinder (1-8877): 1 (default)</code></li></td>
		<td>Inicia com o cilindro 1.</td>
	</tr>
	<tr>
		<td><li><code>Last cylinder, +cylinders or +size {K, M, G}: 8877 (default)</code></li></td>
		<td>Pressione Enter para acessar o último cilindro.</td>
	</tr>
	<tr>
		<td><li>*<code>Command: t</code></li></td>
		<td>Configura o tipo de partição.</td>
	</tr>
	<tr>
		<td><li><code>Select partition 1.</code></li></td>
		<td>Seleciona a partição 1 para ser configurada como um tipo específico.</td>
	</tr>
	<tr>
		<td><li>*<code>Hex code: 83</code></li></td>
		<td>Seleciona Linux como o Tipo (83 é o código hexadecimal para Linux).</td>
	 </tr>
	<tr>
		<td><li>*<code>Command: w</code></li></td>
		<td>Grava as informações da nova partição no disco.</td>
	</tr>
 </tbody>
</table>

  (`*`) Digite m para Ajuda.

  (`**`) Digite L para listar os códigos hexadecimais

### parted

Em muitas distribuições Linux, **parted** vem pré-instalado. Se ele não estiver incluído em sua distribuição, ele poderá ser instalado com:
- Debian/Ubuntu
  ```
  sudo apt-get install parted  
  ```
  {: pre}

- RHEL/CentOS:
  ```
  yum install parted  
  ```
  {: pre}


Para criar um sistema de arquivos com **parted**, siga estas etapas:

1. Execute o parted.

   ```
   parted
   ```
   {: pre}

2. Crie uma partição no disco.
   1. A menos que seja especificado de outra forma, parted usará sua unidade primária, que na maioria dos casos é **/dev/sda**. Alterne para o disco que você deseja particionar usando o comando **select**. Substitua **XXX** pelo seu novo nome do dispositivo.

      ```
      (parted) select /dev/mapper/XXX
      ```
      {: pre}

   2. Execute **print** para confirmar que eles estão no disco certo.

      ```
      (parted) print
      ```
      {: pre}

   3. Crie uma nova tabela de partição GPT

      ```
      (parted) mklabel gpt
      ```
      {: pre}

   4. O parted pode ser usado para criar partições de disco primárias e lógicas e as etapas envolvidas são as mesmas. Para criar uma nova partição, parted usa **mkpart**. É possível fornecer a ele parâmetros adicionais, como **primary** ou **logical**, dependendo do tipo de partição que você deseja criar.
   <br /> **Nota**: as unidades listadas são padronizadas para megabytes (MB), então, para criar uma partição 10 GB, é necessário iniciar em 1 e terminar em 10000. Também é possível mudar as unidades de dimensionamento para terabytes inserindo `(parted) unit TB`, se desejar.

      ```
      (parted) mkpart
      ```
      {: pre}

   5. Saia do parted com **quit**.

      ```
      (parted) quit
      ```
      {: pre}

3. Crie um sistema de arquivos na nova partição.

   ```
   mkfs.ext3 /dev/mapper/XXXlp1
   ```
   {: pre}

   **Nota**: é importante selecionar o disco e a partição certos ao executar o comando acima. Verifique o resultado por imprimir a tabela de partição. Na coluna de sistema de arquivos, deverá ser exibido ext3.

4. Crie um ponto de montagem para o sistema de arquivos e monte-o.
   - crie um nome de partição PerfDisk ou no local em que você desejar montar o sistema de arquivos:

     ```
     mkdir /PerfDisk
     ```
     {: pre}

   - Usando o nome de partição monte o armazenamento:

     ```
     mount /dev/mapper/XXXlp1 /PerfDisk
     ```
     {: pre}

   - Verifique se você vê o seu novo sistema de arquivos listado:

     ```
     df -h
     ```
     {: pre}

5. Inclua o novo sistema de arquivos no arquivo de sistema **/etc/fstab** para ativar a montagem automática na inicialização.
   - Anexe a linha a seguir na parte inferior de **/etc/fstab** (usando o nome de partição da Etapa 3). <br />

     ```
     /dev/mapper/XXXlp1    /PerfDisk    ext3    defaults    0    1
     ```
     {: pre}




## Como verificar se o MPIO está configurado corretamente no *NIX OSes

Para verificar se os caminhos múltiplos estão captando os dispositivos, apenas os dispositivos NETAPP devem aparecer e deve haver dois deles.

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

Verifique se os discos estão presentes e deve haver dois discos com o mesmo identificador e uma listagem de /dev/mapper do mesmo tamanho com o mesmo identificador. O dispositivo /dev/mapper é aquele que os caminhos múltiplos configuram:

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

Se ele não for configurado corretamente, ele será semelhante ao seguinte:
```
No multipath output root@server:~# multipath -l root@server:~#
```

Isso mostrará os dispositivos incluídos na lista de bloqueio:
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

O fdisk mostra apenas os dispositivos `sd *` e nenhum `/dev/mapper`

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
