---

copyright:
  years: 2014, 2018
lastupdated: "2018-10-31"

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ajustando as IOPS

Com esse novo recurso, os usuários de armazenamento do {{site.data.keyword.blockstoragefull}} podem ajustar o IOPS de seu {{site.data.keyword.blockstorageshort}} existente imediatamente. Eles não precisam criar uma duplicata nem copiar dados manualmente para um novo armazenamento. Os usuários não enfrentam nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste está ocorrendo.

O faturamento para o armazenamento é atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia total será faturada no próximo ciclo de faturamento.


## Vantagens do IOPS ajustável

- Gerenciamento de custo - Alguns clientes podem precisar de IOPS alto somente durante os horários de pico de uso. Por exemplo, uma grande loja de varejo tem pico de uso durante os feriados e pode então precisar de uma taxa mais alta de IOPS em seu armazenamento. No entanto, eles não precisam de IOPS mais alto no meio do verão. Esse recurso permite que eles gerenciem seus custos e paguem por IOPS mais alto quando precisarem dele.

## Limitações

Esse recurso está disponível somente em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html).

Os clientes não podem alternar entre Endurance e Performance ao ajustarem sua IOPS. No entanto, eles podem especificar uma nova camada de IOPS ou um nível de IOPS para seu armazenamento com base nos critérios/restrições a seguir:

- Se o volume original é a camada de 0,25 do Endurance, a camada de IOPS não pode ser atualizada.
- Se o volume original for Desempenho, com um valor menor ou igual a 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultarem em um valor menor ou igual a 0,30 IOPS/GB.
- Se o volume original for Desempenho, com mais de 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultarem em mais de 0,30 IOPS/GB.

## Efeito do ajuste de IOPS na replicação

Se o volume tiver a replicação em vigor, a réplica será atualizada automaticamente para corresponder à seleção de IOPS do primário.

## Ajustando o IOPS em seu Armazenamento

1. Vá para a sua lista de  {{site.data.keyword.blockstorageshort}}
   - A partir do  {{site.data.keyword.slportal}}, clique em  ** Armazenamento **  >  ** {{site.data.keyword.blockstorageshort}} **
   - No catálogo do  {{site.data.keyword.BluSoftlayer_full}} , clique em  ** Infraestrutura **  >  ** Armazenamento **  >  ** {{site.data.keyword.blockstorageshort}} **.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Endurance (IOPS em camada): selecione uma Camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, o decréscimo está disponível somente uma vez por mês.
    - Performance (IOPS alocado): especifique a nova opção de IOPS para seu armazenamento inserindo um valor no intervalo de 100 a 48.000 IOPS.
    Certifique-se de consultar quaisquer limites específicos que sejam requeridos por tamanho no formulário de
pedido.
    {:tip}
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principais...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.
