---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-01"

---
{:new_window: target="_blank"}

# Captura Instantânea

As capturas instantâneas são um recurso do {{site.data.keyword.blockstoragefull}}. Uma captura instantânea representa o conteúdo de um volume em um determinado momento. As capturas instantâneas permitem proteger seus dados sem impacto de desempenho, com um consumo mínimo de espaço e são consideradas como a sua primeira linha de defesa para proteção de dados. Se um usuário modifica ou exclui acidentalmente dados cruciais de um volume, os dados podem ser restaurados de forma fácil e rápida por meio de uma cópia de captura instantânea.

O {{site.data.keyword.blockstorageshort}} fornece duas maneiras de tomar suas capturas instantâneas.

– A primeira maneira é por meio de um planejamento de captura instantânea configurável que cria e exclui cópias de captura instantânea automaticamente para cada volume de armazenamento. Também é possível criar planejamentos de captura instantânea extra, excluir cópias manualmente e gerenciar planejamentos com base em seus requisitos. 
- A segunda maneira é obter uma captura instantânea manual.

Uma cópia de captura instantânea é uma imagem somente leitura de um LUN do {{site.data.keyword.blockstorageshort}} que captura o estado do volume em um momento. As cópias de captura instantânea são eficientes tanto no momento em que é necessário criá-las quanto no espaço de armazenamento. Uma cópia de captura instantânea do {{site.data.keyword.blockstorageshort}} leva somente alguns segundos para ser criada. Normalmente menos de 1 segundo, independentemente do tamanho do volume ou do nível de atividade no armazenamento. Depois que uma cópia de captura instantânea é criada, as mudanças nos objetos de dados são refletidas em atualizações para a versão atual dos objetos, como se as cópias de Captura instantânea não existissem. Enquanto isso, a cópia dos dados permanece estável. 

Em uma cópia de Captura instantânea não incorre diminuição de desempenho. Os usuários podem armazenar facilmente até 50 capturas instantâneas planejadas e 50 capturas instantâneas manuais por volume do {{site.data.keyword.blockstorageshort}}, todas as quais são acessíveis como versões somente leitura e on-line dos dados.

Com capturas instantâneas, é possível: 

- Criar ininterruptamente pontos de recuperação point-in-time,
- Reverter os volumes para momentos anteriores.

Deve-se comprar alguma quantia de espaço de captura instantânea para seu volume primeiro para que você possa tirar capturas instantâneas dele. O espaço de captura instantânea pode ser incluído durante o pedido inicial ou mais tarde por meio da página **Detalhes do volume**. As capturas instantâneas planejadas e manuais compartilham o espaço de captura instantânea, portanto, certifique-se de pedir espaço de Captura instantânea suficiente. Consulte o artigo [Solicitando capturas instantâneas](ordering-snapshots.html) para obter mais detalhes e orientação.

**Melhores práticas de captura instantânea**

O design da captura instantânea depende do ambiente do cliente. As considerações de design a seguir podem ajudá-lo a planejar e implementar cópias de Captura instantânea: 
- Até 50 capturas instantâneas podem ser criadas por meio de um planejamento e até 50 manualmente em cada volume ou LUN. 
- Não crie capturas instantâneas em excesso. Certifique-se de que a frequência de captura instantânea planejada atenda às suas necessidades de RTO e RPO e às suas necessidades de negócios do aplicativo, planejando capturas instantâneas por hora, diárias ou semanais. 
- O AutoDelete da captura instantânea pode ser usado para controlar o crescimento do consumo de armazenamento. <br/>
  >**Nota** - o limite de AutoDelete é fixado em 95 por cento.
    
As capturas instantâneas não são substituições para replicação real de Recuperação de desastre externo ou para backup de longa retenção.
    
** Como as capturas instantâneas afetam o espaço em disco **

As cópias de captura instantânea minimizam o uso de espaço em disco preservando blocos individuais em vez de arquivos inteiros. As cópias de captura instantânea usam espaço extra somente quando os arquivos no sistema de arquivos ativo são mudados ou excluídos. Quando isso acontece, os blocos de arquivos originais ainda são preservados como parte de uma ou mais cópias de captura instantânea.

No sistema de arquivos ativo, os blocos mudados são regravados em diferentes locais no disco ou removidos como blocos de arquivos ativos inteiramente. Como resultado, além do espaço em disco usado por blocos no sistema de arquivos ativo modificado, o espaço em disco usado pelos blocos originais ainda é reservado para refletir o status do sistema de arquivos ativo antes da mudança.

<table>
    <colgroup>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
      <col style="width: 33.3%;"/>
    </colgroup>
      <tr>
        <th colspan="3" style="border: 0.0px;text-align: center;">Uso de espaço em disco antes e depois da cópia de captura instantânea</th>
     </tr><tr>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle1.png" alt="Antes da cópia de captura instantânea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle3.png" alt="Depois da cópia de captura instantânea"></td>
        <td style="border: 0.0px;text-align: center;"><img src="/images/bfcircle2.png" alt="Mudanças depois da cópia de captura instantânea"></td>
     </tr><tr>
        <td style="border: 0.0px;">Antes que qualquer cópia de captura instantânea seja criada, o espaço em disco é usado somente pelo sistema de arquivos ativo.</td>
        <td style="border: 0.0px;">Depois que uma cópia de captura instantânea é criada, o sistema de arquivos ativo e a cópia de captura instantânea apontam para os mesmos blocos de disco. A cópia de captura instantânea não usa espaço em disco extra.</td>
        <td style="border: 0.0px;">Depois que <i>myfile.txt</i> é excluído do sistema de arquivos ativo, a cópia de Captura instantânea ainda inclui o arquivo e referencia seus blocos de disco. É por isso que a exclusão de dados do sistema de arquivos ativo nem sempre libera espaço em disco.</td>
      </tr>
</table>

Para visualizar a quantia de espaço de captura instantânea que é usada, siga as instruções no artigo [Gerenciando capturas instantâneas](working-with-snapshots.html).
