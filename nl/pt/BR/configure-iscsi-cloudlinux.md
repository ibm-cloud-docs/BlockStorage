---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: IBM Block Storage, MPIO, iSCSI, LUN, mount secondary storage, mount storage in CloudLinux

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Conectando-se a LUNs iSCSI no CloudLinux
{: #mountingCloudLinux}

Siga estas instruções para instalar seu LUN do iSCSI com caminhos múltiplos no CloudLinux Server liberação 6.10.

Antes de iniciar, certifique-se de que o host que está acessando o volume {{site.data.keyword.blockstoragefull}} tenha sido autorizado anteriormente por meio do [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
{:tip}

1. Efetue login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
2. Na página de listagem do {{site.data.keyword.blockstorageshort}}, localize o novo volume e clique em **Ações**.
3. Clique em **Autorizar host**.
4. Na lista, selecione os hosts que podem acessar o volume e clique em **Enviar**.
5. Anote o IQN do host, o nome do usuário, a senha e o endereço de destino.

Como alternativa, é possível autorizar o host por meio do SLCLI.
```
# slcli block access-authorize --help Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```
{:codeblock}

É melhor executar o tráfego de armazenamento em uma VLAN, que efetua bypass do firewall. A execução do tráfego de armazenamento por meio de firewalls de software aumenta a latência e afeta negativamente o desempenho do armazenamento.
{:important}

## Montando volumes do {{site.data.keyword.blockstorageshort}}
{: #mountingCloudLin}

1. Instale os utilitários iSCSI e de caminhos múltiplos no host e ative-os.
   ```
   yum install iscsi-initiator-utils
   ```
   {: pre}

   ```
   yum install multipath-tools

   ```
   {: pre}

   ```
   chkconfig multipathd on
   ```
   {: pre}

   ```
   chkconfig iscsid on
   ```
   {: pre}

2. Crie ou edite os arquivos de configuração.
   - Atualize seu '/etc/multipath.conf'. <br/>**Nota** - Todos os dados na lista de bloqueio devem ser específicos de seu sistema.
     ```
     defaults {
        user_friendly_names no
        flush_on_last_del       yes
        queue_without_daemon    no
        dev_loss_tmo            infinity
        fast_io_fail_tmo        5
     }
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

   - Atualize suas configurações de CHAP `/etc/iscsi/iscsid.conf` incluindo o nome de usuário e a senha.

     ```
     iscsid.startup = /etc/rc.d/init.d/iscsid force-start
     node.startup = automatic
     node.leading_login = No
     node.session.auth.authmethod = CHAP
     node.session.auth.username = <USER NAME VALUE FROM PORTAL>
     node.session.auth.password = <PASSWORD VALUE FROM PORTAL>
     discovery.sendtargets.auth.authmethod = CHAP
     discovery.sendtargets.auth.username = <USER NAME VALUE FROM PORTAL>
     discovery.sendtargets.auth.password = <PASSWORD VALUE FROM PORTAL>
     ```
     {: codeblock}

     Use maiúscula para nomes de CHAP. Deixe as outras configurações do CHAP comentadas. O armazenamento do {{site.data.keyword.BluSoftlayer_full}} usa somente autenticação unilateral. Não ative o CHAP Mútuo.
     {:important}


3. Reinicie os serviços  ` iscsi `  e  ` multipathd ` .
   ```
   /etc/init.d/iscsi restart   
   ```
   {: pre}

   ```
   /etc/init.d/multipathd restart   
   ```
   {: pre}

4. Descubra o dispositivo usando o endereço IP de destino que foi obtido do {{site.data.keyword.slportal}}.

     A. Execute a descoberta com relação à matriz iSCSI.
       ```
       iscsiadm -m discovery -t sendtargets -p <ip-value-from-SL-Portal>
       ```
       {: pre}

        Exemplo de saída
       ```
       # iscsiadm -m discovery -t sendtargets -p 161.26.98.105
       161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002
       161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002
       ```

     B. Configure o host para efetuar login automaticamente na matriz iSCSI.
       ```
       iscsiadm -m node -L automatic
       ```
       {: pre}

5. Verifique se o host efetuou login na matriz iSCSI e manteve suas sessões.
   ```
   iscsiadm -m session
   ```
   {: pre}

   Exemplo de saída
   ```
   tcp: [1] 161.26.98.105:3260,1026 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   tcp: [2] 161.26.98.108:3260,1029 iqn.1992-08.com.netapp:stfdal1002 (non-flash)
   ```


6. Verifique se o dispositivo está conectado.
   ```
   fdisk -l
   ```
   {: pre}

   Exemplo de saída
   ```
   Disk /dev/sda: 999.7 GB, 999653638144 bytes
   255 heads, 63 sectors/track, 121534 cylinders
   Units = cylinders of 16065 * 512 = 8225280 bytes
   Sector size (logical/physical): 512 bytes / 512 bytes
   I/O size (minimum/optimal): 262144 bytes / 262144 bytes
   Disk identifier: 0x00040f06

   Device Boot      Start         End      Blocks   Id  System
   /dev/sda1   *           1          33      262144   83  Linux
   Partition 1 does not end on cylinder boundary.
   /dev/sda2              33         164     1048576   82  Linux swap / Solaris
   Partition 2 does not end on cylinder boundary.
   /dev/sda3             164      121535   974912512   83  Linux

   Disk /dev/sdb: 21.5 GB, 21474836480 bytes
   64 heads, 32 sectors/track, 20480 cylinders
   Units = cylinders of 2048 * 512 = 1048576 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   Disk identifier: 0x00000000

   Disk /dev/sdc: 21.5 GB, 21474836480 bytes
   64 heads, 32 sectors/track, 20480 cylinders
   Units = cylinders of 2048 * 512 = 1048576 bytes
   Sector size (logical/physical): 512 bytes / 4096 bytes
   I/O size (minimum/optimal): 4096 bytes / 65536 bytes
   Disk identifier: 0x00000000
   ```

   O volume agora está montado e acessível no host.

7. Verifique se o MPIO está configurado corretamente listando os dispositivos. Se a configuração estiver correta, apenas dois dispositivos NETAPP serão mostrados.

   ```
   # multipath -l
   ```
   {: pre}

   Exemplo de saída
   ```
   root@server:~# multipath -l
   3600a098038304454515d4b6a5a444e35 dm-0 NETAPP,LUN C-Mode
   size=20G features='3 queue_if_no_path pg_init_retries 50' hwhandler='1 alua' wp=rw
   |-+- policy='round-robin 0' prio=50 status=active
   | `- 1:0:0:1 sdb 8:16 active ready running
   `-+-policy = 'round-robin 0' prio=10 status=enabled
   `- 2:0:0:1 sdc 8:32 active ready running
   ```
