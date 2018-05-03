---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-23"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Ajustando as IOPS

Com esse novo recurso, os usuários de armazenamento do {{site.data.keyword.blockstoragefull}}
poderão ajustar as IOPS de seu {{site.data.keyword.blockstorageshort}} existente rapidamente, sem a
necessidade de criar uma duplicata ou de migrar dados manualmente para o novo armazenamento. O usuário não sofre nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste
ocorre. 

O faturamento para o armazenamento será atualizado para incluir a diferença rateada entre o novo
preço e o ciclo de faturamento atual e, então, a nova quantia total será faturada no próximo ciclo de
faturamento.

Esse recurso está disponível somente nos [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). 

## Por que aproveitar as IOPS ajustáveis?

- Gerenciamento de custo - alguns dos nossos clientes podem precisar de mais IOPS apenas durante
os períodos de pico de uso. Por exemplo, uma loja de varejo grande tem um pico de uso durante os feriados
e pode precisar de mais IOPS no armazenamento do que no verão. Esse recurso permite que eles gerenciem seus custos e paguem por mais IOPS apenas quando realmente
necessário.

## Existe alguma limitação?

Esse recurso está disponível somente para armazenamento que é provisionado nos [data centers](new-ibm-block-and-file-storage-location-and-features.html) com recursos aprimorados. 

Os clientes não podem alternar entre Resistência e Desempenho quando eles ajustam suas IOPS. Os usuários
podem especificar uma nova camada de IOPS ou nível de IOPS para o armazenamento com base nos
critérios/restrições a seguir: 

- Se o volume original for uma camada 0,25 de Resistência, a camada de IOPS não poderá ser atualizada.
- Se o volume original for Desempenho com < 0,30 IOPS/GB, as opções disponíveis deverão
incluir somente combinações de tamanho e de IOPS que resultem em< 0,30 IOPS/GB. 
- Se o volume original for Desempenho com >= 0,30 IOPS/GB, as opções disponíveis deverão incluir
somente as combinações de tamanho e IOPS que resultem em >= 0,30 IOPS/GB. Tamanho (maior ou igual ao volume
original)



## Como o ajuste de IOPS afeta a replicação?

Se a replicação do volume estiver em vigor, a réplica será atualizada automaticamente para
corresponder à seleção de IOPS do primário. 

## Como ajustar as IOPS em Meu armazenamento?

1. No {{site.data.keyword.slportal}}, clique em **Armazenamento** > **{{site.data.keyword.blockstorageshort}}** OU no Catálogo do {{site.data.keyword.BluSoftlayer_full}} clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Resistência (IOPS em camadas): selecione uma camada de IOPS maior que 0,25 IOPS/GB de seu
armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, a diminuição está
disponível apenas uma vez por mês.
    - Desempenho (IOPS alocadas): especifique a nova opção de IOPS para seu armazenamento
inserindo um valor entre 100 e 48.000 IOPS. (Certifique-se de observar quaisquer limites específicos
necessários pelo tamanho no formulário da solicitação).
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal...** e clique em **Colocar a ordem**.
6. A sua nova alocação de armazenamento deve estar disponível em alguns minutos.
