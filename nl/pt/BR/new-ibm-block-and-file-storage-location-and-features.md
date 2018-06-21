---

copyright:
  years: 2014, 2018
lastupdated: "2018-05-29"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Novos locais e recursos do {{site.data.keyword.blockstorageshort}}

O {{site.data.keyword.BluSoftlayer_full}} está introduzindo uma nova versão do
{{site.data.keyword.blockstoragefull}}.

O novo armazenamento está disponível nos data centers de seleção e é suportado pelo armazenamento
flash a níveis de IOPS mais altos com criptografia de nível de disco para dados em repouso.  Todo o armazenamento que é fornecido nos data centers selecionados são criados automaticamente com a nova versão.

**Nota:** o ponto de montagem do NFS para novos volumes mudou. Consulte
**Novo ponto de montagem para volumes do {{site.data.keyword.filestorage_short}}
criptografados** abaixo para obter detalhes.

O novo {{site.data.keyword.blockstorageshort}} está atualmente disponível em regiões/data centers a seguir com mais disponibilidade do data center em breve!
<table style="width:100%;">
 <caption>A Tabela 1 mostra nossa disponibilidade do Data Center. Cada região possui sua própria coluna. Algumas cidades, como Dallas, San Jose, Washington DC, Amsterdã, Frankfurt, Londres e Sydney, têm múltiplos data centers.</caption>
	 <tr>
	   <td><strong>EUA 2</strong></td>
	   <td><strong>União Europeia (UE)</strong></td>
	   <td><strong>Austrália</strong></td>
	   <td><strong>Canadá</strong></td>
	   <td><strong>América Latina</strong></td>
	   <td><strong>Ásia-Pacífico</strong></td>
	</tr>
	<tr>
	   <td><p>SJC03<br />
		SJC04<br />
		WDC04<br />
		WDC06<br />
		WDC07<br />
		DAL09<br />
		DAL10<br />
		DAL12<br />
		DAL13<br /><br /><br /></p>
	   </td>
	   <td><p>LON02<br />
		LON04<br />
		LON06<br />
		FRA02<br />
		FRA04<br />
		FRA05<br />
		AMS01<br />
		AMS03<br />
		OSLO1<br />
		PAR01<br />
		MIL01</p>
            </td>
	    <td><p>SYD01<br />
		SYD04<br />
		MEL01<br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>TOR01<br />
		MON01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /><br /><br /></p>
	    </td>
	    <td><p>TOK02<br />
		HKG02<br />
		SEO01<br />
		SNG01<br />
		CHE01<br /><br /><br /><br /><br /><br /><br /></p>
	   </td>
	</tr>
</table>


O novo armazenamento tem os seguintes recursos e capacidades:

- **[Criptografia gerenciada por provedor para dados em repouso](block-file-storage-encryption-rest.html)**.
  Todo {{site.data.keyword.blockstorageshort}} é provisionado automaticamente como criptografado sem encargo extra.
- **Opção de camada de 10 IOPS por GB**.
  Uma nova camada está disponível com o {{site.data.keyword.blockstorageshort}} do tipo Endurance para suportar as cargas de trabalho mais exigentes.
- **Armazenamento totalmente suportado em flash.**
  Todo {{site.data.keyword.blockstorageshort}} provisionado com um tipo Endurance ou Performance a 2 IOPS por GB ou superior é suportado por armazenamento totalmente em flash.
- Suporte de **captura instantânea e replicação** com o {{site.data.keyword.blockstorageshort}}
- A opção **Faturamento por hora** está disponível para armazenamento que é planejado para ser usado por menos de um mês integral.
- **Até 48.000 IOPS** para o {{site.data.keyword.blockstorageshort}} que é provisionado com o Performance.
- **As taxas de IOPS são ajustáveis** para melhorar o desempenho durante mudanças sazonais de carga. Leia mais sobre esse recurso [aqui](adjustable-iops.html).
- Crie um clone de seus dados com o **recurso de duplicação de volume do [{{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html)**.
- **O armazenamento é expansível** em incrementos de GB até 12 TB, sem a necessidade de criar uma duplicata ou mover manualmente os dados para um volume maior. Leia mais sobre esse recurso
[aqui](expandable_block_storage.html).

## Novo ponto de montagem para volumes de armazenamento criptografados

Todos os volumes de armazenamento aprimorado que são provisionados nesses data centers têm um ponto de montagem diferente dos volumes não criptografados. Para assegurar que você esteja usando o ponto de montagem correto para seus volumes de armazenamento, é possível visualizar as informações de ponto de montagem na página **Detalhes do volume** na IU. Também é possível acessar o ponto de montagem correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Verifique novamente aqui quando mais data centers são submetidos a upgrade e os recursos e capacidades recém-disponíveis que estão sendo incluídos para o {{site.data.keyword.blockstorageshort}}.
