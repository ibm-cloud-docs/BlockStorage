---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-13"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
 
# Configurando o {{site.data.keyword.blockstorageshort}} para backup com o cPanel

Neste artigo, o objetivo é fornecer instruções para configuração de seus backups para que eles sejam
armazenados no {{site.data.keyword.blockstoragefull}} por meio do cPanel. Supõe-se que o SSH raiz ou
sudo e o acesso do WebHost Manager (WHM) total estejam disponíveis. Esse exemplo é baseado em um
host **CentOS 7**.

**Nota**: é possível localizar a documentação do cPanel para Configurando o diretório
de backup
[aqui](https://docs.cpanel.net/display/68Docs/Backup+Configuration#BackupConfiguration-ConfigureBackupDirectory){:new_window}.

1. Conecte-se ao host por meio de SSH.

2. Deve haver um destino de ponto de montagem. <br />
   **Nota**: por padrão, o sistema cPanel salva os arquivos de backup localmente, no
diretório `/backup`. Para os propósitos deste documento, supondo que o
`/backup` já existe e que contém os backups, usaremos o `/backup2` como
o novo ponto de montagem.
   
3. Configure o seu {{site.data.keyword.blockstorageshort}} conforme descrito em [Conectando aos LUNs de iSCSI do MPIO no Linux](accessing_block_storage_linux.html). 
Certifique-se de montá-lo no `/backup2` e de configurá-lo em `/etc/fstab`
para ativar a montagem na inicialização.

4. **Opcional**: copie os backups existentes para o novo armazenamento. Use `rsync` por exemplo:
   ```
   rsync -azv /backup/* /backup2/
   ```
   {: pre}
    
    **Nota**: esse comando transmite os seus dados compactados enquanto preserva tanto quanto possível (exceto hardlinks) e fornece informações sobre quais arquivos estão sendo transferidos, mais um breve resumo no final.
    
5.  Efetue login no WebHost Manager e navegue para a configuração de backup por meio de
**Início** > **Backup** > **Configuração de backup**.

6.  Edite a configuração para salvar os backups no novo ponto de montagem. 
    - Mude o diretório de backup padrão inserindo o caminho absoluto no novo local no lugar do
diretório /backup/. 
    - Selecione **Ativar para montar uma unidade de backup**. Essa configuração faz
com que o processo de Configuração de backup verifique o arquivo `/etc/fstab` para uma
montagem de backup (`/backup2`). <br /> **Nota**: se uma montagem existir
com o mesmo nome que o diretório temporário, o processo de Configuração de backup montará a unidade e fará
backup das informações na montagem.  Depois que o processo de backup é concluído, ele desmonta a unidade. 

7. Aplique as mudanças clicando em **Salvar configuração** na parte inferior da
interface **Configuração de backup**.

8. **Opcional**: como determinado pelo seu caso de uso específico e necessidades de negócios, remova o armazenamento antigo do servidor e cancele da conta.

