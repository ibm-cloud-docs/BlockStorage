---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Protegendo seus dados - Criptografia em repouso gerenciada por provedor

## Criptografia em repouso do {{site.data.keyword.blockstorageshort}} 

O {{site.data.keyword.BluSoftlayer_full}} leva a necessidade de segurança a sério e entende a importância de ser capaz de criptografar dados para mantê-los seguros. Com a criptografia gerenciada por provedor, o {{site.data.keyword.blockstoragefull}} provisionado com as opções Endurance ou Performance é criptografado por padrão sem nenhum custo extra e nenhum impacto no desempenho.

O recurso de criptografia em repouso gerenciada por provedor usa os protocolos padrão de mercado a seguir:

* Criptografia AES-256 padrão de mercado
* As chaves são gerenciadas internamente com o padrão de mercado Key Management Interoperability Protocol (KMIP)
* O armazenamento é validado para o Federal Information Processing Standard (FIPS) Publication 140-2, o Federal Information Security Management Act (FISMA) e o Health Insurance Portability and Accountability Act (HIPAA). O armazenamento também é validado para a conformidade com o Payment Card Industry (PCI), o Basel II, o California Security Breach Information Act (SB 1386) e o EU Data Protection Directive 95/46/EC.

## Criptografia de dados em repouso para armazenamento Capturas instantâneas ou Replicado  

Todas as capturas instantâneas e réplicas do {{site.data.keyword.blockstorageshort}} criptografado também são criptografadas por padrão. Esse recurso não pode ser desativado em uma base de volume.

## Fornecimento de armazenamento com criptografia

O recurso de criptografia em repouso gerenciada por provedor está disponível somente para o {{site.data.keyword.blockstorageshort}} provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). Todo o armazenamento provisionado nestes data centers é provisionado automaticamente com criptografia para dados em repouso.

Ao pedir o {{site.data.keyword.blockstorageshort}}, selecione um data center marcado com um asterisco (`*`). Você verá um ícone de bloqueio à direita do campo Nome do LUN/volume indicando que ele está criptografado.

![O ícone de bloqueio indica que o LUN está criptografado](/images/encryptedstorage.png)
<caption>Figura 1. Exemplo do ícone de bloqueio indicando que o LUN está criptografado.</caption>



**Nota**: o armazenamento não criptografado provisionado antes de o data center ser submetido a upgrade **não** será criptografado automaticamente. Se você tiver um armazenamento não criptografado em um data center submetido a upgrade, será necessário criar um novo LUN ou volume e executar uma migração de dados. O artigo a seguir pode fornecer orientação:

* [Migração do {{site.data.keyword.blockstorageshort}} em data centers submetidos a upgrade](migrate-block-storage-encrypted-block-storage.html)
