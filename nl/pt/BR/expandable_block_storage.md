---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacidade de Block Storage expansível

Com esse novo recurso, nossos usuários atuais do {{site.data.keyword.blockstoragefull}} podem expandir o tamanho de seu {{site.data.keyword.blockstorageshort}} existente em incrementos de GB de até 12 TB imediatamente, sem a necessidade de criar uma duplicata ou migrar dados manualmente para um volume maior. Não há nenhuma indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento está ocorrendo. 

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que aproveitar o armazenamento expansível?

- **Gerenciamento de custos** - você pode saber que há potencial para o crescimento de seus dados, mas precisa de uma quantia menor de armazenamento para iniciar. A capacidade de expansão permite que nossos clientes economizem custos de armazenamento e, em seguida, aumentem de acordo com suas necessidades.  

- **Necessidades de armazenamento cada vez maiores** - os clientes que experimentam um rápido crescimento de dados precisam de uma maneira rápida e fácil para aumentar o tamanho de seu armazenamento para gerenciar isso.

## Como a expansão da capacidade de armazenamento afeta a replicação?

A ação de expansão no armazenamento primário resulta em redimensionamento automático da réplica. 

## Há alguma limitação?

Esse recurso está disponível somente para armazenamento que é provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

O armazenamento que foi provisionado nesses data centers antes da liberação deste recurso (14 de dezembro de 2017) pode ser aumentado somente para 10 vezes seu tamanho original. O armazenamento provisionado após essa data pode ser aumentado em até 12 TB de tamanho. 

As limitações de tamanho existentes para o {{site.data.keyword.blockstorageshort}} provisionado
com Resistência ainda se aplicam (até 4 TB para uma camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Como posso saber se meu armazenamento está criptografado?

O armazenamento provisionado com recursos aprimorados é sempre criptografado em repouso. É possível verificar facilmente se seu armazenamento é elegível quando ele possui um ícone
"bloqueio" adjacente na UI do portal. 

## Como posso redimensionar meu armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. A sua nova alocação de armazenamento deve estar disponível em alguns minutos.
  
