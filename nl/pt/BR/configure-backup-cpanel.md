---

copyright:
  years: 2014, 2018
lastupdated: "2018-06-26"

---
{:new_window: target="_blank"}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.blockstorageshort}} para backup com o cPanel

Use este artigo para configurar seus backups em cPanel para ser armazenado no {{site.data.keyword.blockstoragefull}}. Supõe-se que o SSH raiz ou
sudo e o acesso do WebHost Manager (WHM) total estejam disponíveis. Estas instruções se baseiam em um host do **CentOS 7**.

**Nota**: é possível localizar a documentação do cPanel para **Configurando o diretório de backup** [aqui](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   **Nota**: por padrão, o sistema cPanel salva os arquivos de backup localmente, no
diretório `/backup`. Para os propósitos deste documento, considera-se que `/backup` existe e contém backups e que `/backup2` é usado como o novo ponto de montagem.
   
3. Configure o seu {{site.data.keyword.blockstorageshort}} conforme descrito em [Conectando aos LUNs de iSCSI do MPIO no Linux](accessing_block_storage_linux.html). Certifique-se de montá-lo em `/backup2` e configurá-lo em `/etc/fstab` para ativar a montagem no início.

4. **Opcional**: copie os backups existentes para o novo armazenamento. É possível usar  ` rsync `.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Nota**: esse comando compacta e transmite seus dados, enquanto preserva o máximo possível, exceto para links físicos. Ele fornece informações sobre quais arquivos estão sendo transferidos e um breve resumo no final.
    
5. Efetue login no WebHost Manager e acesse a configuração de backup clicando em **Página inicial** > **Backup** > **Configuração de backup**.

6. Edite a configuração para salvar os backups no novo ponto de montagem. 
    - Mude o diretório de backup padrão inserindo o caminho absoluto no novo local no lugar do
diretório /backup/. 
    - Selecione **Ativar para montar uma unidade de backup**. Essa configuração faz com que o processo de configuração de backup verifique o arquivo `/etc/fstab` quanto a uma montagem de backup (`/backup2`). <br /> 
    **Nota**: se existir uma montagem com o mesmo nome que o diretório temporário, o processo de configuração de backup montará a unidade e fará backup das informações na unidade. Depois que o processo de backup é concluído, ele desmonta a unidade. 

7. Aplique as mudanças clicando em **Salvar configuração**.

8. **Opcional**: como determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele da conta.

