---

copyright:
  years: 2014, 2018
lastupdated: "2018-09-12"

---
{:new_window: target="_blank"}

# Expandindo a capacidade do Block Storage

Com esse novo recurso, os usuários atuais do {{site.data.keyword.blockstoragefull}} podem expandir o tamanho de seu {{site.data.keyword.blockstorageshort}} existente em incrementos de GB até 12 TB imediatamente. Eles não precisam criar uma duplicata nem migrar dados manualmente para um volume maior. Não há nenhuma indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento está ocorrendo. 

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

## Vantagens do Armazenamento Expandível

- **Gerenciamento de custo** - Talvez você saiba que há potencial para crescimento de seus dados, mas que precisa de uma quantia menor de armazenamento para iniciar. A capacidade de expansão permite que nossos clientes economizem custos de armazenamento e, em seguida, aumentem de acordo com suas necessidades.  

- **Necessidades de armazenamento cada vez maiores** - os clientes que experimentam um rápido crescimento de dados precisam de uma maneira rápida e fácil para aumentar o tamanho de seu armazenamento para gerenciar isso.

## Efeitos da expansão da capacidade de armazenamento na Replicação

A ação de expansão nos resultados de armazenamento primário resulta no redimensionamento automático da réplica. 

## Limitações

Esse recurso está disponível para armazenamentos provisionados em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

Os armazenamentos provisionados nesses data centers antes da liberação desse recurso, de **abril de 2017 a 14 de dezembro de 2017**, podem ser aumentados para até 10 vezes o seu tamanho original, não mais que isso. Os armazenamentos provisionados após **14 de dezembro de 2017** podem ser aumentados para até 12 terabytes. 

As limitações de tamanho existentes para o {{site.data.keyword.blockstorageshort}} que foram provisionadas com o Endurance ainda se aplicam (até 4 TB para a camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Redimensionando o

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.
