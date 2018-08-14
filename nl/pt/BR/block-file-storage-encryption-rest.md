---

copyright:
  years: 2014, 2018
lastupdated: "2018-07-18"

---
{:new_window: target="_blank"}

# Protegendo seus dados - Criptografia em repouso gerenciada por provedor

## Criptografia em repouso do {{site.data.keyword.blockstorageshort}} 

O {{site.data.keyword.BluSoftlayer_full}} leva a necessidade de segurança a sério e entende a importância de ser capaz de criptografar dados para mantê-los seguros. Com a criptografia gerenciada por provedor, o {{site.data.keyword.blockstoragefull}} que é provisionado com a opção Endurance ou Performance, é criptografado por padrão, sem custo extra e sem impacto no desempenho.

O recurso de criptografia em repouso gerenciada por provedor usa os protocolos padrão de mercado a seguir:

* Criptografia AES-256 padrão de mercado
* As chaves são gerenciadas internamente com o padrão de mercado Key Management Interoperability Protocol (KMIP)
* O armazenamento é validado para o Federal Information Processing Standard (FIPS) Publication 140-2, o Federal Information Security Management Act (FISMA) e o Health Insurance Portability and Accountability Act (HIPAA). O armazenamento também é validado para o Payment Card Industry (PCI), o Basel II, o Security Breach Information Act (SB 1386) da Califórnia e a conformidade com a Data Protection Directive 95/46/EC da União Europeia.

## Fornecendo criptografia em repouso para capturas instantâneas ou armazenamento replicado  

Todas as capturas instantâneas e réplicas do {{site.data.keyword.blockstorageshort}} criptografado também são criptografadas por padrão. Esse recurso não pode ser desativado em uma base de volume.

## Fornecimento de armazenamento com criptografia

O recurso de criptografia em repouso gerenciada por provedor está disponível para o {{site.data.keyword.blockstorageshort}} que é provisionado em [data centers selecionados](new-ibm-block-and-file-storage-location-and-features.html). Todo o armazenamento pedido nesses data centers é provisionado automaticamente com criptografia.

Ao pedir o {{site.data.keyword.blockstorageshort}}, selecione um data center anotado com um asterisco (`*`). É possível ver um ícone de bloqueio à direita do campo Nome do LUN/Volume indicando que o volume está criptografado.

![O ícone de bloqueio indica que o LUN está criptografado](/images/encryptedstorage.png)
<caption>Figura 1. Exemplo do ícone de bloqueio mostrando que o LUN está criptografado.</caption>



**Nota** - O armazenamento não criptografado que foi provisionado antes do upgrade do data center **não é** criptografado automaticamente. Se você possuir um armazenamento não criptografado em um data center submetido a upgrade e desejar armazenamento criptografado, será necessário criar um novo LUN/volume e migrar seus dados. Para obter mais informações, consulte [Migração do {{site.data.keyword.blockstorageshort}} em data centers submetidos a upgrade](migrate-block-storage-encrypted-block-storage.html).
