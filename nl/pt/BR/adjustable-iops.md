---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-05"

keywords: Block storage, new feature, adjusting IOPS, modify IOPS, increase IOPS, decrease IOPS,

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Ajustando as IOPS
{: #adjustingIOPS}

Com esse novo recurso, os usuários de armazenamento do {{site.data.keyword.blockstoragefull}} podem ajustar o IOPS de seu {{site.data.keyword.blockstorageshort}} existente imediatamente. Eles não precisam criar uma duplicata nem copiar dados manualmente para um novo armazenamento. Os usuários não enfrentam nenhum tipo de indisponibilidade ou falta de acesso ao armazenamento enquanto o ajuste está ocorrendo.

O faturamento para o armazenamento é atualizado para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia total será faturada no próximo ciclo de faturamento.


## Vantagens do IOPS ajustável

- Gerenciamento de custo - Alguns clientes podem precisar de IOPS alto somente durante os horários de pico de uso. Por exemplo, uma grande loja de varejo tem pico de uso durante os feriados e pode então precisar de uma taxa mais alta de IOPS em seu armazenamento. No entanto, eles não precisam de IOPS mais alto no meio do verão. Esse recurso permite que eles gerenciem seus custos e paguem por IOPS mais alto quando precisarem dele.

## Limitações
{: #limitsofIOPSadjustment}

Esse recurso está disponível somente em [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news).

Os clientes não podem alternar entre Endurance e Performance ao ajustarem sua IOPS. No entanto, eles podem especificar uma nova camada ou nível IOPS para seu armazenamento com base nos critérios e restrições a seguir:

- Se o volume original é a camada de 0,25 do Endurance, a camada de IOPS não pode ser atualizada.
- Se o volume original for Desempenho, com um valor menor ou igual a 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultarem em um valor menor ou igual a 0,30 IOPS/GB.
- Se o volume original for Desempenho, com mais de 0,30 IOPS/GB, as opções disponíveis incluirão apenas as combinações de tamanho e de IOPS que resultarem em mais de 0,30 IOPS/GB.

## Efeito do ajuste de IOPS na replicação

Se o volume tiver a replicação em vigor, a réplica será atualizada automaticamente para corresponder à seleção de IOPS do primário.

## Ajustando o IOPS em seu Armazenamento
{: #adjustingsteps}

1. Vá para a sua lista de  {{site.data.keyword.blockstorageshort}}
   - A partir do  {{site.data.keyword.slportal}}, clique em  ** Armazenamento **  >  ** {{site.data.keyword.blockstorageshort}} **
   - No console do {{site.data.keyword.BluSoftlayer_full}}, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o LUN na lista e clique em **Ações** > **Modificar LUN**
3. Em **Opções de IOPS de armazenamento**, faça uma nova seleção:
    - Para Endurance (IOPS em camada), selecione uma camada de IOPS maior que 0,25 IOPS/GB de seu armazenamento. É possível aumentar a camada de IOPS a qualquer momento. No entanto, o decréscimo está disponível somente uma vez por mês.
    - Para Performance (IOPS alocado), especifique a nova opção de IOPS para seu armazenamento digitando um valor no intervalo de 100 a 48.000 IOPS.

    Certifique-se de consultar quaisquer limites específicos que sejam requeridos por tamanho no formulário de
pedido.
    {:tip}
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principais...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.


Como alternativa, é possível ajustar o IOPS por meio da CLI do SL.
```
# slcli block volume-modify --help Usage: slcli block volume-modify [OPTIONS] VOLUME_ID

Options:
  -c, --new-size INTEGER        New Size of block volume in GB. ***If no size
                                is given, the original size of volume is
                                used.***
                                Potential Sizes: [20, 40, 80, 100,
                                250, 500, 1000, 2000, 4000, 8000, 12000]
                                Minimum: [the original size of the volume]
  -i, --new-iops INTEGER        Performance Storage IOPS, between 100 and 6000
                                in multiples of 100 [only for performance
                                volumes] ***If no IOPS value is specified, the
                                original IOPS value of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is less than 0.3, new IOPS/GB
                                must also be less than 0.3. If original
                                IOPS/GB for the volume is greater than or
                                equal to 0.3, new IOPS/GB for the volume must
                                also be greater than or equal to 0.3.]
  -t, --new-tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) [only for
                                endurance volumes] ***If no tier is specified,
                                the original tier of the volume will be
                                used.***
                                Requirements: [If original IOPS/GB
                                for the volume is 0.25, new IOPS/GB for the
                                volume must also be 0.25. If original IOPS/GB
                                for the volume is greater than 0.25, new
                                IOPS/GB for the volume must also be greater
                                than 0.25.]
  -h, --help                    Show this message and exit.
```
{:codeblock}
