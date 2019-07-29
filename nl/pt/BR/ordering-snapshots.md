---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: Block Storage, snapshot space, ordering snapshots,

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# Pedindo capturas instantâneas
{: #orderingsnapshots}

Para criar capturas instantâneas de seu volume de armazenamento, seja automaticamente ou manualmente, é necessário comprar espaço para mantê-las. É possível comprar capacidade até sua quantia de volume de armazenamento (durante a compra do volume inicial ou posteriormente usando as etapas descritas aqui).

## Determinando a quantidade de espaço de captura instantânea a ser pedido

Genericamente falando, o espaço de captura instantânea é usado por capturas instantâneas com base em dois fatores principais
- Quanto seu sistema de arquivos ativo muda ao longo do tempo,
- Quanto tempo você planeja reter as capturas instantâneas.  

A maneira de calcular a quantia de espaço necessário é **(Taxa de mudança)** x **(número de horas/dias/semanas/meses em que os dados são retidos)**.

A primeira captura instantânea usa uma quantidade insignificante de espaço, pois é apenas uma cópia dos metadados
(ponteiros) que indica os blocos do sistema de arquivos ativo.
{:note}

Um volume com várias mudanças e um período de retenção longo precisa de mais espaço do que um volume com mudança moderada e um planejamento de retenção moderado. Um exemplo para o primeiro tipo é um banco de dados com uma alta taxa de mudança. Um exemplo para o segundo tipo é um armazenamento de dados do VMware.

Se você usar 12 capturas instantâneas por hora de 500 GB de dados reais e houver 1 por cento de mudança entre cada captura instantânea, terminará com 60 GB para capturas instantâneas.

*(Taxa de mudança de 5 GB) x (12 capturas instantâneas por hora) = (60 GB de espaço usado)*

Por outro lado, se esses 500 GB de dados reais, com 12 capturas instantâneas por hora, vissem 10 por cento de mudança a cada hora, o espaço de captura instantânea usado seria de 600 GB.

*(Taxa de mudança de 50 GB) x (12 capturas instantâneas por hora) = (600 GB de espaço usado)*

Portanto, quando você determinar quanto espaço de Captura instantânea precisará, considere a taxa de mudança atentamente. Isso influencia enormemente a quantia de espaço de captura instantânea necessária. É mais provável que um volume maior mude mais vezes. No entanto, um volume de 500 GB com 5 GB de mudança e um volume de 10 TB com 5 GB de mudança usam a mesma quantidade de espaço de captura instantânea.

Além disso, para a maioria das cargas de trabalho, quanto maior for um volume, menos espaço precisará ser reservado inicialmente. É principalmente devido às eficiências de dados subjacentes e à natureza de como as capturas instantâneas funcionam no ambiente.

## Pedindo o espaço de captura instantânea por meio do console do {{site.data.keyword.cloud_notm}}

1. Efetue login no [console do {{site.data.keyword.cloud_notm}}](https://{DomainName}/catalog){: external} e clique no ícone de menu na parte superior esquerda. Em seguida, selecione **Infraestrutura clássica**.
2. Acesse o LUN de armazenamento por meio de **Armazenamento** >**{{site.data.keyword.blockstorageshort}}**.
2. Clique em **Mudar o espaço de captura instantânea** no quadro Capturas instantâneas.
3. Selecione a quantidade de espaço que você precisa e o método de pagamento.
4. Clique em **Continuar**.
5. Insira qualquer **Código promocional** que você tiver e clique em **Recalcular**. Os campos Encargos para este pedido e Revisão do pedido são concluídos por padrão.

   Os descontos são aplicados quando o pedido é processado.
   {:note}
6. Marque a caixa **Eu li o Contrato de Prestação de Serviços Principal e concorde com os termos contidos nele** e clique em **Fazer pedido**. Seu espaço de captura instantânea será provisionado em alguns minutos.

## Pedindo Espaço de captura instantânea por meio da SLCLI

```
# slcli block snapshot-order --help
Usage: slcli block snapshot-order [OPTIONS] VOLUME_ID

Options:
  --capacity INTEGER    Size of snapshot space to create in GB  [required]
  --tier [0.25|2|4|10]  Endurance Storage Tier (IOPS per GB) of the block
                        volume for which space is ordered [optional, and only
                        valid for endurance storage volumes]
  --upgrade             Flag to indicate that the order is an upgrade
  -h, --help            Show this message and exit.
```
{:codeblock}
