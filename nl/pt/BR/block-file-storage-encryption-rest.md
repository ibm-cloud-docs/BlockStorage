---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage Encryption, industry standard protocols, IBM Block Storage, LUN, provider-managed encryption

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}

# Criptografia em repouso gerenciada pelo provedor
{: #encryption}

## Criptografia em repouso do {{site.data.keyword.blockstorageshort}}

O {{site.data.keyword.cloud}} leva a necessidade de segurança a sério e entende a importância de ser capaz de criptografar dados para mantê-los seguros. Com a criptografia gerenciada por provedor, o {{site.data.keyword.blockstoragefull}} que é provisionado com a opção Endurance ou Performance, é criptografado por padrão, sem custo extra e sem impacto no desempenho.

O recurso de criptografia em repouso gerenciada por provedor usa os protocolos padrão de mercado a seguir:

* Criptografia AES-256 padrão de mercado
* As chaves são gerenciadas internamente com o padrão de mercado Key Management Interoperability Protocol (KMIP)
* O armazenamento é validado para o Federal Information Processing Standard (FIPS) Publication 140-2, o Federal Information Security Management Act (FISMA) e o Health Insurance Portability and Accountability Act (HIPAA). O armazenamento também é validado para o Payment Card Industry (PCI), o Basel II, o Security Breach Information Act (SB 1386) da Califórnia e a conformidade com a Data Protection Directive 95/46/EC da União Europeia.

## Fornecendo criptografia em repouso para capturas instantâneas ou armazenamento replicado  

Todas as capturas instantâneas e réplicas do {{site.data.keyword.blockstorageshort}} criptografado também são criptografadas por padrão. Esse recurso não pode ser desativado em uma base de volume.

## Fornecimento de armazenamento com criptografia

O recurso de criptografia em repouso gerenciada por provedor está disponível para o {{site.data.keyword.blockstorageshort}} que é provisionado em [data centers selecionados](/docs/infrastructure/BlockStorage?topic=BlockStorage-news). Todo o armazenamento pedido nesses data centers é provisionado automaticamente com criptografia.

Ao pedir o {{site.data.keyword.blockstorageshort}}, selecione um data center anotado com um asterisco (`*`). É possível ver um ícone de bloqueio à direita do campo Nome do LUN/Volume indicando que o volume está criptografado.

![O ícone de bloqueio indica que o LUN está criptografado](/images/encryptedstorage.png)
<caption>Figura 1. Exemplo do ícone de bloqueio mostrando que o LUN está criptografado.</caption>



O armazenamento não criptografado que foi fornecido antes do upgrade do data center **não** é criptografado automaticamente. Se você tiver armazenamento não criptografado em um data center atualizado e desejar armazenamento criptografado, será necessário criar um novo volume e migrar seus dados. Para obter mais informações, consulte
[Migração
do {{site.data.keyword.blockstorageshort}} em data centers com upgrade](/docs/infrastructure/BlockStorage?topic=BlockStorage-migratestorage).
{:important}
