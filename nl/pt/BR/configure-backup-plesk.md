---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-12"

keywords: Block storage, Plesk, backups, mountpoint, ISCSI

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurando o {{site.data.keyword.blockstorageshort}} para backup com o Plesk
{: #PleskBackups}

É possível usar as instruções a seguir para configurar o {{site.data.keyword.blockstoragefull}}
para seus backups no Plesk. Supõe-se que o SSH
raiz ou sudo e o acesso total ao Plesk no nível de administrador estejam disponíveis. Essas instruções baseiam-se em um host do CentOS7.

Para obter mais informações, consulte a [documentação do Plesk para backup e restauração](https://docs.plesk.com/en-US/12.5/administrator-guide/backing-up-and-restoration.59256/){: external}.
{:tip}

1. Conecte-se ao host por meio de SSH.
2. Assegure-se de que um destino de ponto de montagem exista.

   O Plesk tem duas opções para armazenar os backups. Uma opção é o armazenamento interno do Plesk (armazenamento de backup no servidor Plesk). A outra opção é um armazenamento FTP externo (armazenamento de backup em algum servidor externo na web ou em sua rede local). Em geral nas caixas do Plesk, os backups internos são armazenados em
`/var/lib/psa/dumps` e usam `/tmp` como um diretório temporário. Nesse exemplo, o diretório temporário é mantido local, mas o diretório dumps é movido para o destino {{site.data.keyword.blockstorageshort}} (`/backup/psa/dumps`). Nenhuma credencial de usuário FTP é necessária.
   {:note}   
3. Configure seu {{site.data.keyword.blockstorageshort}} conforme descrito em [Conectando-se
a LUNs iSCSI no Linux](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux#mountingLinux). Monte o {{site.data.keyword.blockstorageshort}} em /backup e configure /etc/fstab para ativar a montagem no início.
4. **Opcional**: copie os backups existentes para o novo armazenamento. É possível usar  ` rsync `.
   ```
   rsync -avz /var/lib/psa/dumps /backup/psa/dumps
   ```
   {: pre}

    Esse comando compacta e transmite os dados preservando-os o máximo possível, exceto para links físicos. Ele fornece informações sobre quais arquivos estão sendo transferidos e um breve resumo no final.
    {:tip}    
5. Edite `/etc/psa/psa.conf` para apontar o valor de `DUMP_D` para
o novo destino.
    - Ele aparece como:  ` DUMP_D /backup/psa/dumps `.
6. **Opcional**: como determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele da conta.
