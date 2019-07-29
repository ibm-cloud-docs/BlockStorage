---

copyright:
  years: 2018, 2019
lastupdated: "2019-07-22"

keywords: Block Storage, new feature, adjusting capacity, modify capacity, increase capacity, Storage Capacity

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Expandindo a capacidade do Block Storage
{: #expandingcapacity}

Com esse recurso, os usuários atuais do {{site.data.keyword.blockstoragefull}} podem
expandir imediatamente o tamanho do seu {{site.data.keyword.blockstorageshort}} existente em incrementos de GB até 12 TB. Eles não precisam criar uma duplicata nem migrar dados manualmente para um volume maior. Não há nenhuma indisponibilidade ou falta de acesso ao armazenamento enquanto o redimensionamento está ocorrendo.

O faturamento para o volume é atualizado automaticamente para incluir a diferença rateada do novo preço no ciclo de faturamento atual. A nova quantia integral será faturada no próximo ciclo de faturamento.

Esse recurso está disponível na [maioria dos data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

## Vantagens do Armazenamento Expandível

- **Gerenciamento de custo** - Talvez você saiba que há potencial para crescimento de seus dados, mas que precisa de uma quantia menor de armazenamento para iniciar. A capacidade de expansão
permite que nossos clientes economizem no custo de armazenamento e, mais tarde, aumentem o armazenamento para acomodar suas
necessidades.  

- **Necessidades de armazenamento cada vez maiores** - os clientes que experimentam um rápido crescimento de dados precisam de uma maneira rápida e fácil para aumentar o tamanho de seu armazenamento para gerenciar isso.

## Efeitos da expansão da capacidade de armazenamento na Replicação

A ação de expansão nos resultados de armazenamento primário resulta no redimensionamento automático da réplica.

## Limitações
{: #limitsofexpandingstorage}

Esse recurso está disponível para o armazenamento que é provisionado na [maioria dos data centers](/docs/infrastructure/BlockStorage?topic=BlockStorage-selectDC).

O armazenamento que foi fornecido nesses data centers antes da liberação desse recurso, durante **abril de 2017 e 14 de dezembro de 2017**, pode ser aumentado para 10 vezes seu tamanho original, não mais que isso. O armazenamento que foi fornecido após **14 de dezembro de 2017** pode ser aumentado até 12 TB.

As limitações de tamanho existentes para o {{site.data.keyword.blockstorageshort}} que foram provisionadas com o Endurance ainda se aplicam (até 4 TB para a camada de 10 IOPS e até 12 TB para todas as outras camadas).

## Redimensionando o
{: #resizingsteps}

1. No console do {{site.data.keyword.cloud}}, clique no ícone de **menu**. Em seguida, clique em **Infraestrutura** > **Armazenamento** > **{{site.data.keyword.blockstorageshort}}**.
2. Selecione o volume iSCSI na lista e clique em **...** > **Modificar LUN**
3. Insira o novo tamanho de armazenamento em GB.
4. Revise a sua seleção e a nova precificação.
5. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principais...** e clique em **Fazer pedido**.
6. Sua nova alocação de armazenamento estará disponível em alguns minutos.

Como alternativa, é possível redimensionar o volume por meio da CLI do SL.

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

Para obter mais informações sobre como expandir o sistema de arquivos (e partições, se houver
alguma) no volume para usar o novo espaço, verifique a documentação do S.O.
{:tip}
