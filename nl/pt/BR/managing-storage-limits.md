---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-24"

keywords: Block Storage, limit increase, global quota, quota increase

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="DomainName"}

# Gerenciando limites de armazenamento
{: #managingstoragelimits}

Por padrão, é possível fornecer um total combinado de 250 volumes de {{site.data.keyword.blockstorageshort}} e
de {{site.data.keyword.filestorage_short}} globalmente.

Se não tiver certeza de quantos volumes tem, será possível listar os volumes para cada data center usando o comando
`slcli` a seguir.
```
# slcli block volume-count --help
Usage: slcli block volume-count [OPTIONS]

Options:
  -d, --datacenter TEXT  Datacenter shortname
  --sortby TEXT          Column to sort by
  -h, --help             Show this message and exit.
```

É possível solicitar um aumento de limite ao enviar um caso de suporte no [portal](https://{DomainName}/unifiedsupport/cases/add){: external}. Quando a solicitação for aprovada, você obterá um limite de volume configurado para um data center específico.  

Para solicitar um aumento de limite, abra um caso e direcione-o para o seu representante de vendas.

No caso, forneça as informações a seguir:

- **Assunto do chamado**: Solicitação para aumentar o limite de armazenamento da contagem de volume do data center

- **Qual é o caso de uso para a solicitação de volumes adicionais?** <br />
*Por exemplo, sua resposta pode ser algo semelhante a um novo armazenamento de dados do VMware, a um novo ambiente de desenvolvimento e teste (desenv./teste), a um banco de dados SQL ou a uma criação de log.*

- **Quantos volumes de Bloco extras são necessários por tipo, tamanho, IOPS e local?** <br />
*Por exemplo, sua resposta pode ser algo semelhante a "25x Endurance 2 TB @ 4 IOPS em DAL09" ou "25x Performance 4 TB @ 2 IOPS em WDC04".*

- **Quantos volumes de Arquivo extras são necessários por tipo, tamanho, IOPS e local?** <br />
*Por exemplo, sua resposta pode ser algo semelhante a "25x Performance 20 GB a 10 IOPS em DAL09" ou "50x Endurance 2 TB a 0,25 IOPS em SJC03".*

- **Forneça uma estimativa de quando você espera ou planeja fornecer todo o aumento de volume solicitado.** <br />
 "*Por exemplo, sua resposta pode ser algo semelhante a "90 dias".*

- **Forneça uma previsão de 90 dias de uso médio esperado de capacidade desses
volumes.** <br />
*Por exemplo, sua resposta pode ser algo semelhante a "espera-se que 25 por cento sejam usados em 30 dias, 50 por cento em 60 dias e 75 por cento em 90 dias".*

Responda a todas as perguntas e instruções na sua solicitação. Elas são necessárias para o processamento e a aprovação.
{:important}

Você será notificado da atualização para seus limites por meio do processo de caso.
