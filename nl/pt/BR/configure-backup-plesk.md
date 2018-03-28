---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.blockstorageshort}} para backup com o Plesk

Neste artigo, o objetivo é fornecer instruções para configurar o
{{site.data.keyword.blockstoragefull}} para seus backups no Plesk. Supõe-se que o SSH
raiz ou sudo e o acesso total ao Plesk no nível de administrador estejam disponíveis. Esse exemplo é
baseado em um host CentOS7.

**Nota**: é possível localizar a documentação do Plesk para backup e restauração
[aqui](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Deve haver um destino de ponto de montagem. <br />
   **Nota**: o Plesk tem duas opções para armazenamento de backups, uma é o
armazenamento do Plesk interno, que é um backup de armazenamento localizado em seu servidor Plesk e a outra é um
armazenamento FTP externo, que é um backup de armazenamento localizado em um servidor externo na web ou em sua
rede local. Em geral nas caixas do Plesk, os backups internos são armazenados em
`/var/lib/psa/dumps` e usam `/tmp` como um diretório temporário. Em nosso
exemplo, o diretório temporário local é mantido, mas o diretório de dump é movido para o destino STaaS
(`/backup/psa/dumps`). Nenhuma credencial de usuário FTP é necessária.
   
3. Configure o seu {{site.data.keyword.blockstorageshort}} conforme descrito em [Conectando aos LUNs de iSCSI do MPIO no Linux](accessing_block_storage_linux.html). 
Monte o {{site.data.keyword.blockstorageshort}} em `/backup` e configure
`/etc/fstab` para ativar a montagem na inicialização.

4. **Opcional**: copie os backups existentes para o novo armazenamento. Use `rsync` por exemplo:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Nota**: esse comando transmite os seus dados compactados enquanto preserva tanto quanto possível (exceto hardlinks) e fornece informações sobre quais arquivos estão sendo transferidos, mais um breve resumo no final.
    
5. Edite `/etc/psa/psa.conf` para apontar o valor de `DUMP_D` para
o novo destino. 
    -  Deve aparecer como: `DUMP_D /backup/psa/dumps`. 

6. **Opcional**: como determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele da conta.


