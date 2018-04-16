---

copyright:
  years: 2014, 2018
lastupdated: "2018-04-16"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Novos locais e recursos do {{site.data.keyword.blockstorageshort}} e
do {{site.data.keyword.filestorage_short}}

O {{site.data.keyword.BluSoftlayer_full}} está introduzindo uma nova versão do
{{site.data.keyword.blockstoragefull}}. 

O novo armazenamento está disponível nos data centers de seleção e é suportado pelo armazenamento
flash a níveis de IOPS mais altos com criptografia de nível de disco para dados em repouso.  Todo o armazenamento
provisionado nos data centers de seleção será provisionado automaticamente com a nova versão do
{{site.data.keyword.blockstorageshort}} e do {{site.data.keyword.filestorage_full}}.

**Nota:** o ponto de montagem do NFS para novos volumes mudou. Consulte
**Novo ponto de montagem para volumes do {{site.data.keyword.filestorage_short}}
criptografados** abaixo para obter detalhes.

Os novos {{site.data.keyword.blockstorageshort}} e {{site.data.keyword.filestorage_short}}
estão atualmente disponíveis nas regiões/data centers a seguir com disponibilidade de data center
adicional em breve!
<table style="width:100%;">
	<caption>Disponibilidade de data center</caption>
	<tbody>
		<tr>
			<td><strong>EUA 2</strong></td>
			<td><strong>União Europeia (UE)</strong></td>
			<td><strong>Austrália</strong></td>
			<td><strong>Canadá</strong></td>
			<td><strong>América Latina</strong></td>
			<td><strong>Ásia-Pacífico</strong></td>
		</tr>
		<tr>
			<td>
				<p>SJC03<br />
				SJC04<br />
				WDC04<br />
				WDC06<br />
				WDC07<br />
				DAL09<br />
				DAL10<br />
				DAL12<br />
				DAL13</p>
			</td>
			<td>
				<p>LON02<br />
				LON04<br />
				LON06<br />
				FRA02<br />
				AMS01<br />
				AMS03<br />
				OSLO1<br />
				PAR01<br />
				MIL01<br /></p>
			</td>
			<td>
				<p>SYD01<br />
				SYD04<br />
				MEL01<br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOR01<br />
				MON01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>MEX01<br />SAO01<br /><br /><br /><br /><br /><br /><br /><br /></p>
			</td>
			<td>
				<p>TOK02<br />
				HKG02<br />
			        SEO01<br />
				SNG01<br />
				CHE01<br /><br /><br /><br /><br /></p>
			</td>
			</tr>
	</tbody>
</table>


O novo armazenamento tem os seguintes recursos e capacidades:

- [Criptografia gerenciada por provedor para dados
em repouso](block-file-storage-encryption-rest.html). O {{site.data.keyword.blockstorageshort}} e
o {{site.data.keyword.filestorage_short}} serão totalmente e automaticamente provisionados
como criptografados sem custo adicional.
- Opção de camada de 10 IOPS por GB. Uma nova camada foi incluída no
{{site.data.keyword.blockstorageshort}} e no {{site.data.keyword.filestorage_short}} do tipo
Resistência para suportar as cargas de trabalho mais exigentes.
- Armazenamento com suporte total para flash. O {{site.data.keyword.blockstorageshort}} e
o {{site.data.keyword.filestorage_short}} são provisionados com Resistência ou Desempenho a
2 IOPS por GB ou superior com armazenamento totalmente suportado para flash.
- Suporte de captura instantânea e replicação com o {{site.data.keyword.blockstorageshort}} e
o {{site.data.keyword.filestorage_short}} provisionados com Resistência ou Desempenho.
- Opção de faturamento por hora incluída para armazenamento que é planejado para ser usado por menos de
um mês completo. 
- Até 48.000 IOPS para o {{site.data.keyword.blockstorageshort}} e
o {{site.data.keyword.filestorage_short}} provisionados com Desempenho.
- As taxas de IOPS são ajustáveis para melhorar o desempenho em caso de mudanças de carga sazonais.
Leia mais sobre esse recurso [aqui](adjustable-iops.html).
- Crie um novo clone de seus dados com o
[Recurso de
duplicação de volume do {{site.data.keyword.blockstorageshort}}](how-to-create-duplicate-volume.html).
- O armazenamento pode ser expandido em incrementos de GB para até 12 TB rapidamente, sem a
necessidade de criar uma duplicata ou de migrar dados manualmente para um volume maior. Leia mais sobre esse recurso
[aqui](expandable_block_storage.html).

## Novo ponto de montagem para volumes de armazenamento criptografados

Todos os volumes de armazenamento criptografados provisionados nestes data centers têm um ponto de
montagem diferente dos volumes não criptografados. Para assegurar que você está usando o ponto de montagem
correto para os volumes de armazenamento criptografados e não criptografados, é possível visualizar as
informações do ponto de montagem na página Detalhes do Volume na UI, assim como acessar o ponto de montagem
correto por meio de uma chamada API: `SoftLayer_Network_Storage::getNetworkMountAddress()`.

Verifique novamente aqui quando data centers adicionais são submetidos a upgrade e
se recursos e capacidades recém-disponíveis estão sendo incluídos no
{{site.data.keyword.blockstorageshort}}.
