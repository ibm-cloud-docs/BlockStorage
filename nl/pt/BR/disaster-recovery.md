---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-05"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Recuperação de desastre: failover com um volume primário inacessível
{: #dr-inaccessible}

No caso de uma falha catastrófica ou de um desastre que cause uma indisponibilidade no site primário, os clientes podem executar as seguintes ações para acessar rapidamente seus dados no site secundário.

## Failover com uma duplicata de um volume de réplica no site secundário

1. Efetue login no [console do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://
{DomainName}/){:new_window} e clique no ícone **Menu** na parte superior esquerda. Selecione **Infraestrutura clássica**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
2. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
3. Clique na réplica do LUN na lista para visualizar a sua página de **Detalhes**.
4. Na página de **Detalhes**, role para baixo e selecione uma captura instantânea existente e, em seguida, clique em **Ações** > **Duplicar**.
5. Faça quaisquer atualizações necessárias para a capacidade (para aumentar o tamanho) ou os IOPs para o novo volume.
6. Atualize o espaço de captura instantânea para o novo volume, se necessário.
7. Clique em **Continuar** para fazer o pedido da duplicata.

Assim que o volume é criado, ele pode ser anexado a um host e executar operações de leitura/gravação. Enquanto os dados estão sendo copiados do volume original para a duplicata, a página de detalhes mostra que a duplicação está em andamento. Quando o processo de duplicação for concluído, o novo volume se tornará independente do original e poderá ser gerenciado com capturas instantâneas e replicação normalmente.

## Failback para o site primário original

Se você deseja retornar a produção para o site primário original, execute as seguintes etapas.

1. Efetue login no [console do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://
{DomainName}/){:new_window} e clique no ícone **Menu** na parte superior esquerda. Selecione **Infraestrutura clássica**.

   Como alternativa, é possível efetuar login no [{{site.data.keyword.slportal}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://control.softlayer.com/){:new_window}.
2. Clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
3. Clique no nome do LUN e crie um planejamento de captura instantânea (se ainda não existir um).

   Para obter mais informações sobre os planejamentos de captura instantânea, consulte [Gerenciando as capturas instantâneas](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingSnapshots#addingschedule).
   {:tip}
4. Clique em **Réplica** e em **Comprar uma replicação**.
5. Selecione o planejamento de captura instantânea existente que você deseja que a replicação siga. A lista contém todos os planejamentos de captura instantânea ativos.
6. Clique em **Localização** e selecione o data center que era o site de produção original.
7. Clique em **Continuar**.
8. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços principal…** e clique em **Fazer pedido**.

Após a conclusão da replicação, é necessário criar um volume duplicado da nova réplica.
{:important}

1. Volte para **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Clique na réplica do LUN na lista para visualizar a sua página de **Detalhes**.
3. Na página de **Detalhes**, role para baixo e selecione uma captura instantânea existente e, em seguida, clique em **Ações** > **Duplicar**.
4. Faça quaisquer atualizações necessárias para a capacidade (para aumentar o tamanho) ou os IOPs para o novo volume.
5. Atualize o espaço de captura instantânea para o novo volume, se necessário.
6. Clique em **Continuar** para colocar a sua ordem para a duplicata.

Quando o processo de duplicação for concluído, será possível cancelar a replicação e os volumes que foram usados para obter os dados de volta ao site primário original. A duplicata torna-se o armazenamento primário e a replicação para o site secundário original pode ser estabelecida novamente.
