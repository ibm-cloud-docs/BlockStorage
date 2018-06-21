---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ajustando as IOPS

Com esse novo recurso, os usuários de armazenamento do {{site.data.keyword.blockstoragefull}} poderão ajustar a IOPS de seu {{site.data.keyword.blockstorageshort}} existente imediatamente, sem a necessidade de criar uma duplicata ou de migrar dados manualmente para o novo armazenamento. Os usuários não terão nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste estiver ocorrendo. 

O faturamento do armazenamento será atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia total será faturada no próximo ciclo de faturamento.

Esse recurso está disponível somente nos [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que aproveitar as IOPS ajustáveis?

- Gerenciamento de custos - alguns dos nossos clientes podem precisar de IOPS alto somente durante os horários de pico de uso. Por exemplo, uma grande loja de varejo tem pico de uso durante os feriados e pode precisar de uma taxa mais alta de IOPS no armazenamento do que no meio do verão. Esse recurso permite gerenciar seus custos e pagar IOPS mais alta somente quando realmente necessário.

## Há alguma limitação?

Esse recurso está disponível somente para armazenamento que é provisionado em [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados. 

Os clientes não podem alternar entre Endurance e Performance ao ajustarem sua IOPS. Os usuários podem especificar uma nova camada de IOPS ou nível de IOPS para o armazenamento com base nos critérios/restrições a seguir: 

- Se o volume original for uma camada 0,25 de Resistência, a camada de IOPS não poderá ser atualizada.
- Se o volume original for Performance com < 0,30 IOPS/GB, as opções disponíveis deverão incluir somente combinações de tamanho e de IOPS que resultem em < 0,30 IOPS/GB. 
- Se o volume original for Desempenho com >= 0,30 IOPS/GB, as opções disponíveis deverão incluir
somente as combinações de tamanho e IOPS que resultem em >= 0,30 IOPS/GB. Tamanho (maior ou igual ao volume
original)



## Como o ajuste de IOPS afeta a replicação?

Se o volume tem a replicação em vigor, a réplica é atualizada automaticamente para corresponder à seleção de IOPS do primário. 

## Como ajustar as IOPS em Meu armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU, no catálogo do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Endurance (IOPS em camada): selecione uma Camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, a diminuição está disponível somente uma vez por mês.
    - Performance (IOPS alocada): especifique a nova opção de IOPS para seu armazenamento inserindo um valor entre 100 e 48.000 IOPS. (Certifique-se de observar quaisquer limites específicos
necessários pelo tamanho no formulário da solicitação).
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Fazer pedido**.
6. A sua nova alocação de armazenamento deve estar disponível em alguns minutos.
