---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Protegendo seus dados - Criptografia de dados em repouso gerenciada por provedor

## Criptografia de dados em repouso do {{site.data.keyword.blockstorageshort}} e do {{site.data.keyword.filestorage_full_notm}} 

O {{site.data.keyword.BluSoftlayer_full}} leva muito a sério a necessidade de segurança e entende a importância da capacidade de criptografia de dados para mantê-los seguros. Com a criptografia gerenciada por provedor, o {{site.data.keyword.blockstoragefull}} e o {{site.data.keyword.filestorage_full}} provisionados com Resistência ou Desempenho são criptografados por padrão sem nenhum custo adicional e sem nenhum impacto no desempenho.

O recurso Criptografia de dados em repouso gerenciada por provedor usa os protocolos padrão de mercado a seguir:

* Criptografia AES-256 padrão de mercado
* As chaves são gerenciadas internamente com o Key Management Improbabilidade Protocol (KMIP) padrão de mercado
* O armazenamento é o Federal Information Processing Standard (FIPS) Publication 140-2 validado para conformidade com o Federal Information Security Management Act (FISMA), Health Insurance Portability and Accountability Act (HIPAA), Payment Card Industry (PCI), Basel II, California Security Breach Information Act (SB 1386) e UE Data Protection Directive 95/46/EC

## Criptografia de dados em repouso para armazenamento Capturas instantâneas ou Replicado   

Todas as capturas instantâneas e réplicas do {{site.data.keyword.blockstorageshort}} criptografado também são criptografadas por padrão. Esse recurso não pode ser desativado em uma base de volume.

## Fornecimento de armazenamento com criptografia

O recurso Criptografia de dados em repouso gerenciada por provedor está disponível apenas para o {{site.data.keyword.blockstorageshort}} que é provisionado em data centers de seleção com nova disponibilidade do data center sendo incluída regularmente. Todo o armazenamento provisionado nestes data centers é provisionado automaticamente com criptografia para dados em repouso. Clique [aqui](new-ibm-block-and-file-storage-location-and-features.html) para ver a lista atual de data centers nos quais a criptografia do {{site.data.keyword.blockstorageshort}} para dados em repouso está disponível.

Ao solicitar seu {{site.data.keyword.blockstorageshort}}, selecione um data center anotado com um * e com uma mensagem informando que a criptografia está disponível. Será exibido um ícone de bloqueio à direita do campo Nome do LUN/volume indicando que ele está criptografado. Consulte a Figura 1.

![O ícone de bloqueio indica que o LUN está criptografado](/images/encryptedstorage.png)
<caption>Figura 1. Exemplo do ícone de bloqueio indicando que o LUN está criptografado.</caption>



**Observe** que um armazenamento não criptografado provisionado antes de um upgrade do data center **não** é criptografado automaticamente. Se você tiver um armazenamento não criptografado em um data center submetido a upgrade, será necessário criar um novo LUN ou volume e executar uma migração de dados. Os artigos a seguir podem fornecer orientação.

* [Migração do {{site.data.keyword.blockstorageshort}} em data centers submetidos a upgrade](migrate-block-storage-encrypted-block-storage.html)
