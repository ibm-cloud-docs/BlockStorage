---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, block storage, snapshot, snapshot space, snapshot best practices, snapshot usage,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Captura Instantânea
{: #snapshots}

As capturas instantâneas são um recurso do {{site.data.keyword.blockstoragefull}}. Uma captura instantânea representa o conteúdo de um volume em um determinado momento. Com as capturas instantâneas, é possível proteger seus dados sem afetar o desempenho e com consumo de espaço mínimo. As capturas instantâneas são consideradas sua primeira linha de defesa para a proteção de dados. Se um usuário modifica ou exclui acidentalmente dados cruciais de um volume, os dados podem ser restaurados de forma fácil e rápida por meio de uma cópia de captura instantânea.

O {{site.data.keyword.blockstorageshort}} fornece duas maneiras de tomar suas capturas instantâneas.

* Primeiro, por meio de um planejamento de captura instantânea configurável que cria e exclui as cópias de captura
instantânea automaticamente para cada volume de armazenamento. Também é possível criar planejamentos de captura instantânea extra, excluir cópias manualmente e gerenciar planejamentos com base em seus requisitos.
* A segunda maneira é obter uma captura instantânea manual.

Uma cópia de captura instantânea é uma imagem somente leitura de um LUN do {{site.data.keyword.blockstorageshort}} que captura o estado do volume em um momento. As cópias de captura instantânea são eficientes tanto no momento em que é necessário criá-las quanto no espaço de armazenamento. Uma cópia de captura instantânea do {{site.data.keyword.blockstorageshort}} leva somente alguns segundos para ser criada. Normalmente menos de 1 segundo, independentemente do tamanho do volume ou do nível de atividade no armazenamento. Depois que uma cópia de captura instantânea é criada, as mudanças nos objetos de dados são refletidas em atualizações para a versão atual dos objetos, como se as cópias de Captura instantânea não existissem. Enquanto isso, a cópia dos dados permanece estável.

Em uma cópia de Captura instantânea não incorre diminuição de desempenho. Os usuários podem armazenar facilmente até 50 capturas instantâneas planejadas e 50 capturas instantâneas manuais por volume do {{site.data.keyword.blockstorageshort}}, todas as quais são acessíveis como versões somente leitura e on-line dos dados.

Com capturas instantâneas, é possível:

- Criar ininterruptamente pontos de recuperação point-in-time,
- Reverter os volumes para momentos anteriores.

Deve-se comprar alguma quantia de espaço de captura instantânea para seu volume primeiro para que você possa tirar capturas instantâneas dele. O espaço de captura instantânea pode ser incluído durante o pedido inicial ou mais tarde por meio da página **Detalhes do volume**. As capturas instantâneas planejadas e manuais compartilham o espaço de captura instantânea, portanto, certifique-se de pedir espaço de Captura instantânea suficiente. Para obter mais informações, consulte [Pedindo capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingsnapshots).

## Melhores práticas de captura instantânea

O design da captura instantânea depende do ambiente do cliente. As considerações de design a seguir podem ajudá-lo a planejar e implementar cópias de Captura instantânea:
- Até 50 capturas instantâneas podem ser criadas por meio de um planejamento e até 50 manualmente em cada volume ou LUN.
- Não crie capturas instantâneas em excesso. Certifique-se de que a frequência de captura instantânea planejada atenda às suas necessidades de RTO e RPO e às suas necessidades de negócios do aplicativo, planejando capturas instantâneas por hora, diárias ou semanais.
- O AutoDelete da captura instantânea pode ser usado para controlar o crescimento do consumo de armazenamento. <br/>

  O limite de AutoDelete é fixo em 95%.
  {:note}

As capturas instantâneas não são substituições para replicação real de Recuperação de desastre externo ou para backup de longa retenção.
{:important}

## Segurança

Todas as capturas instantâneas e réplicas do {{site.data.keyword.blockstorageshort}} criptografado também são criptografadas por padrão. Esse recurso não pode ser desativado em uma base de volume. Para obter mais informações sobre a criptografia em repouso gerenciada pelo provedor, consulte [Protegendo os dados](/docs/infrastructure/BlockStorage?topic=BlockStorage-encryption).

## Como as capturas instantâneas afetam o espaço em disco

As cópias de captura instantânea minimizam o consumo de disco ao preservar blocos individuais em vez de arquivos inteiros. As cópias de captura instantânea usam espaço extra somente quando os arquivos no sistema de arquivos ativo são mudados ou excluídos.

No sistema de arquivos ativo, os blocos mudados são regravados em diferentes locais no disco ou removidos como blocos de arquivos ativos inteiramente. Quando os arquivos são mudados ou excluídos, os blocos de arquivos originais são preservados como parte de uma ou mais cópias de captura instantânea. Como resultado, o espaço em disco que é usado pelos blocos originais ainda está reservado para refletir o status do sistema de arquivos ativo antes da mudança. Esse espaço é reservado, além do espaço em disco que é usado por blocos no sistema de arquivos ativo modificado.


| Uso de espaço em disco |   |
|-----|-----|
| ![O espaço que é usado antes da obtenção de uma cópia de captura instantânea](/images/bfcircle1.png "Antes da captura instantânea") | Antes que qualquer cópia de captura instantânea seja criada, o espaço em disco é usado somente pelo sistema de arquivos ativo. |
| ![O espaço que é usado quando uma cópia de captura instantânea é obtida](/images/bfcircle3.png "Após a cópia de captura instantânea") | Depois que uma cópia de captura instantânea é criada, o sistema de arquivos ativo e a cópia de captura instantânea apontam para os mesmos blocos de disco. A cópia de captura instantânea não usa espaço em disco extra.  |
| ![O espaço que é usado quando alguma coisa muda após a obtenção de uma cópia de captura instantânea](/images/bfcircle2.png "Mudanças após a cópia de captura instantânea") | Depois que `myfile.txt` é excluído do sistema de arquivos ativo, a cópia de Captura instantânea ainda inclui o arquivo e referencia seus blocos de disco. É por isso que a exclusão de dados do sistema de arquivos ativo nem sempre libera espaço em disco. |
{: caption="A Tabela 1 mostra como as capturas instantâneas afetam o uso de espaço no armazenamento." caption-side="top"}

Para obter mais informações sobre o uso de espaço de captura instantânea, consulte [Gerenciando capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots)
