---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.blockstorageshort}} para backup com o Plesk

Este artigo ajudará você a configurar o {{site.data.keyword.blockstoragefull}} para seus backups no Plesk. Supõe-se que o SSH
raiz ou sudo e o acesso total ao Plesk no nível de administrador estejam disponíveis. Nossas instruções se baseiam em um host CentOS7.

**Nota**: é possível localizar a documentação do Plesk para backup e restauração
[aqui](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   **Nota**: o Plesk tem duas opções para armazenar backups. Uma opção é o armazenamento interno do Plesk, que é o armazenamento de backup no servidor Plesk, a outra é um armazenamento externo do FTP, que é um armazenamento de backup em algum servidor externo na web ou na rede local. Geralmente em caixas do Plesk, os backups internos são armazenados em
`/var/lib/psa/dumps` e usam `/tmp` como um diretório temporário. Em nosso exemplo, mantemos o local do diretório temporário, mas movemos o diretório dumps para o destino STaaS (`/backup/psa/dumps`). Nenhuma credencial do usuário FTP é necessária.
   
3. Configure o seu {{site.data.keyword.blockstorageshort}} conforme descrito em [Conectando aos LUNs de iSCSI do MPIO no Linux](accessing_block_storage_linux.html). Monte o {{site.data.keyword.blockstorageshort}} em `/backup` e configure
`/etc/fstab` para ativar a montagem na inicialização.

4. **Opcional**: copie os backups existentes para o novo armazenamento. É possível usar `rsync`:
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}
    
    **Nota**: esse comando transmite seus dados compactados enquanto preserva o máximo possível, exceto para links físicos. Ele fornece informações sobre quais arquivos estão sendo transferidos e um breve resumo no final.
    
5. Edite `/etc/psa/psa.conf` para apontar o valor de `DUMP_D` para
o novo destino. 
    - Ele deve aparecer como: `DUMP_D /backup/psa/dumps`. 

6. **Opcional**: como determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele da conta.


