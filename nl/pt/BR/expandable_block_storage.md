---

copyright:
  years: 2014, 2017
lastupdated: "2017-12-15"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Capacidade de armazenamento de bloco expansível

Com esse novo recurso, os usuários atuais do {{site.data.keyword.blockstoragefull}}
conseguem expandir o tamanho de seu {{site.data.keyword.blockstorageshort}} existente em incrementos
de GB até 12 TB rapidamente, sem a necessidade de criar uma duplicata ou de migrar manualmente dados para
um volume maior.
Não há indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento ocorre.  

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada entre o
novo preço e o ciclo de faturamento atual e, então, a nova quantia total será faturada no próximo ciclo de
faturamento.

Esse recurso está disponível somente nos [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que aproveitar o armazenamento expansível?

- **Gerenciamento de custo** - talvez você saiba que há um
potencial de crescimento de seus dados, mas você precisa de uma quantia de armazenamento menor para iniciar. A capacidade
de expansão permite que nossos clientes economizem custos de armazenamento e, em seguida, aumentem de
acordo com suas necessidades.  

- **Necessidades de armazenamento cada vez maiores** - os clientes
que experimentam um rápido crescimento precisam de uma maneira rápida e fácil para aumentar o tamanho de
seu armazenamento para gerenciar esse crescimento.

## Como a capacidade de expansão de armazenamento afeta a replicação?

A ação de expansão no armazenamento primário resulta em redimensionamento automático da réplica. 

## Existe alguma limitação?

Esse recurso está disponível somente para armazenamento que é provisionado nos [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados. 

O armazenamento que é provisionado no armazenamento atualizado nesses data centers antes da liberação
deste recurso (14 de dezembro de 2017) pode ser aumentado em até 10 vezes apenas com relação ao seu tamanho
original.

Todos os outros armazenamentos provisionados, após essa data, podem ser aumentados até o tamanho máximo de 12 TB. 

As limitações de tamanho existentes para o {{site.data.keyword.blockstorageshort}} provisionado
com Resistência ainda se aplicam (até 4 TB para uma camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Como identificar se meu armazenamento provisionado é expansível?

O armazenamento provisionado com recursos aprimorados é sempre submetido à criptografia de dados em repouso. É possível verificar facilmente se seu armazenamento é elegível quando ele possui um ícone
"bloqueio" adjacente na UI do portal. 

## Como redimensionar meu armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU no Catálogo do {{site.data.keyword.BluSoftlayer_full}} clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Colocar a ordem**.
6. A sua nova alocação de armazenamento deve estar disponível em alguns minutos.
  
