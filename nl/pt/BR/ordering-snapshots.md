---

copyright:
  years: 2014, 2018
lastupdated: "2018-03-07"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Solicitando capturas instantâneas

Para criar capturas instantâneas de seu volume de armazenamento, seja automaticamente ou manualmente,
é necessário comprar espaço para mantê-las. É possível comprar capacidade até a quantia de seu volume de
armazenamento (durante a compra de volume inicial ou posteriormente usando as etapas abaixo).

1. Acesse seu LUN de armazenamento por meio da guia **Armazenamento**,
**{{site.data.keyword.blockstorageshort}}** do
[{{site.data.keyword.slportal}}](https://control.softlayer.com/){:new_window}.
2. Clique no link **Incluir espaço de captura instantânea**
no quadro Capturas instantâneas.
3. Selecione a quantidade de espaço que você precisa clicando no botão de opções ao lado da quantia
apropriada.
4. Clique em **Continuar**.
5. Insira qualquer Código promocional que tenha e clique em **Recalcular**. Os Encargos para essa ordem e Revisão de ordem terão valores padrão.
6. Clique na caixa de seleção **Eu li o Contrato de Prestação de Serviços Principal…** e clique em **Colocar a ordem**. 
Seu espaço de captura instantânea será provisionado em alguns minutos.

## Como determinar a quantia de espaço de captura instantânea a ser solicitada

Infelizmente, não há uma melhor prática com relação à melhor recomendação com base no aplicativo. 
Genericamente falando, o espaço de captura instantânea é consumido por capturas instantâneas com base em duas
partes chave de informações:
- a quantia de mudança que seu sistema de arquivos possui e 
- por quanto tempo você planeja reter as capturas instantâneas.  

Essencialmente, o caminho para calcular a quantia de espaço necessária é **(Taxa de
Mudança)** x **(número de horas/dias/semanas/meses de retenção)**.  
**Nota**: a primeira captura instantânea consome uma quantia de espaço irrisória,
já que ela é apenas uma cópia de metadados (ponteiros) indicando os blocos do sistema de arquivos ativo. 

Um volume com muita mudança de dados (por exemplo, um banco de dados de alta taxa de mudança) e um
período longo de retenção de captura instantânea precisará de mais espaço para capturas instantâneas do
que um volume com mudança moderada (por exemplo, um armazenamento de dados VM) e um planejamento de
retenção de captura instantânea mais moderado. 

Na instância de um volume que possui 500 GB de dados reais, se tiver que obter 12 capturas instantâneas
por hora e observar 1% de mudança entre cada captura instantânea, que você acabaria com (Taxa de
mudança de 5 G) x (12 capturas instantâneas por hora) = 60 GB para capturas instantâneas.

Por outro lado, se nesses 500 GB de dados reais, com 12 capturas instantâneas por hora, for observado
10% de mudança por hora, você acabaria com (Taxa de mudança de 50 GB) x (12 capturas instantâneas por
hora) = 600 GB.

Portanto, ao determinar quanto espaço de captura instantânea que você precisa, considere a taxa de
mudança cuidadosamente. Isso influencia enormemente a quantia de espaço de captura instantânea que você precisa.
Enquanto o tamanho de um volume provavelmente significará uma quantia maior de mudança, um volume
de 500 G com 5 G de mudança e um volume de 10 TB com 5 G de mudança resultariam no mesmo uso de espaço de
captura instantânea.

Além disso, para a maioria das cargas de trabalho, quanto maior o volume, menos espaço precisa ser
reservado inicialmente para capturas instantâneas. Isso é principalmente devido à eficiência dos dados
subjacentes da nossa plataforma, bem como devido à natureza de como as capturas instantâneas funcionam em nosso
ambiente.



