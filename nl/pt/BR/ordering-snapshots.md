---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Solicitando capturas instantâneas

Para criar capturas instantâneas de seu volume de armazenamento, seja automaticamente ou manualmente, é necessário comprar espaço para mantê-las. É possível comprar capacidade até sua quantia de volume de armazenamento (durante a compra de volume inicial ou posteriormente usando as etapas descritas neste artigo).

1. Acesse seu LUN de armazenamento por meio de **Armazenamento**, guia **{{site.data.keyword.blockstorageshort}}** do [{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Clique em **Incluir espaço de captura instantânea** no quadro Capturas instantâneas.
3. Selecione a quantia de espaço que você precisa.
4. Clique em **Continuar**.
5. Insira qualquer **Código promocional** que você tenha e clique em **Recalcular**. Os campos Encargos para este pedido e Revisão do pedido são concluídos por padrão.
6. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…**
e clique em **Fazer o pedido**. Seu espaço de captura instantânea será provisionado em alguns minutos.

## Como determinar a quantia de espaço de captura instantânea a ser pedida

Genericamente falando, o espaço de captura instantânea é usado por capturas instantâneas com base em duas partes chave de informações:
- a quantia de mudança que seu sistema de arquivos ativo possui e
- por quanto tempo você planeja reter as capturas instantâneas.  

Essencialmente, a maneira de calcular a quantia de espaço necessária é **(Taxa de Mudança)** x **(número de horas/dias/semanas/meses que os dados são retidos)**.  
**Nota**: a primeira captura instantânea usa uma quantia insignificante de espaço, já que é apenas uma cópia dos metadados (ponteiros) indicando os blocos do sistema de arquivos ativo. 

Um volume com muita mudança de dados (como um banco de dados de taxa de mudança alta) e um período longo de retenção de captura instantânea precisará de mais espaço para capturas instantâneas do que um volume com mudança moderada (como um armazenamento de dados da VM) e um planejamento de retenção de captura instantânea mais moderado. 

Se você tomasse 12 capturas instantâneas por hora de um volume com 500 GB de dados reais e visse 1 por cento de mudança entre cada captura instantânea, você acabaria com 60 GB para capturas instantâneas.

*(5 G de taxa de mudança) x (12 capturas instantâneas por hora) = (60 GB de espaço usado)*

Por outro lado, se os 500 GB de dados reais, com 12 capturas instantâneas por hora, vissem 10 por cento de mudança a cada hora, você acabaria com 600 GB.

*(50 GB de taxa de mudança) x (12 capturas instantâneas por hora) = (600 GB de espaço usado)*

Portanto, ao determinar quanto espaço de captura instantânea que você precisa, considere a taxa de
mudança cuidadosamente. Isso influencia enormemente a quantia de espaço de captura instantânea necessária. Embora o tamanho de um volume provavelmente signifique uma quantia maior de mudança, um volume de 500 GB com 5 GB de mudança e um volume de 10 TB com 5 GB de mudança resultariam no mesmo uso de espaço de captura instantânea.

Além disso, para a maioria das cargas de trabalho, quanto maior o volume, menos espaço precisa ser
reservado inicialmente para capturas instantâneas. Isso ocorre principalmente devido às eficiências dos dados subjacentes da nossa plataforma e à natureza de como as capturas instantâneas funcionam em nosso ambiente.



