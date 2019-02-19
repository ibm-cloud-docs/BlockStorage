---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Configurando o {{site.data.keyword.blockstorageshort}} para backup com o cPanel
{: #cPanelBackups}

É possível usar as instruções a seguir para configurar que seus backups no cPanel sejam
armazenados no {{site.data.keyword.blockstoragefull}}. Supõe-se que o SSH raiz ou
sudo e o acesso do WebHost Manager (WHM) total estejam disponíveis. Estas instruções se baseiam em um host do **CentOS 7**.

Para obter mais informações, consulte [cPanel: configurando o diretório de backup ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.
{:tip}

1. Conecte-se ao host por meio de SSH.

2. Assegure-se de que um destino de ponto de montagem exista. <br />
   Por padrão, o sistema cPanel salva os arquivos de backup localmente no diretório `/backup`. Para os propósitos deste documento, considera-se que `/backup` existe e contém backups e que `/backup2` é usado como o novo ponto de montagem.
   {:note}

3. Configure o seu {{site.data.keyword.blockstorageshort}} conforme descrito em [Conectando aos LUNs de iSCSI do MPIO no Linux](accessing_block_storage_linux.html). Certifique-se de montá-lo em `/backup2` e configurá-lo em `/etc/fstab` para ativar a montagem no início.

4. **Opcional**: copie os backups existentes para o novo armazenamento. É possível usar  ` rsync `.
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}

    Esse comando compacta e transmite os dados preservando-os o máximo possível, exceto para os links
físicos. Ele fornece informações sobre quais arquivos estão sendo transferidos e um breve resumo no final.
    {:tip}

5. Efetue login no WHM e acesse a configuração de backup clicando em **Página inicial** > **Backup** > **Configuração de backup**.

6. Edite a configuração para salvar os backups no novo ponto de montagem.
    - Mude o diretório de backup padrão inserindo o caminho absoluto no novo local no lugar do
diretório /backup/.
    - Selecione **Ativar para montar uma unidade de backup**. Essa configuração faz com que o processo de configuração de backup verifique o arquivo `/etc/fstab` quanto a uma montagem de backup (`/backup2`). <br />

    Se existir uma montagem com o mesmo nome do diretório temporário, o processo de configuração de backup
montará a unidade e fará o backup das informações na unidade. Depois que o processo de backup é concluído, ele desmonta a unidade.
    {:note}

7. Aplique as mudanças clicando em **Salvar configuração**.

8. **Opcional**: como determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele da conta.
