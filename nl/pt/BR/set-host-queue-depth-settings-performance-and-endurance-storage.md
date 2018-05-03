---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Recomendação para configurações de profundidade da fila do host

O {{site.data.keyword.BluSoftlayer_full}} recomenda um host máximo e uma profundidade da fila de
entrada/saída (E/S) de aplicativo para cada camada de desempenho. A configuração do host não afeta a latência
de disco e do controlador, apenas a latência observada pelo host e pelo aplicativo.

A profundidade da fila acima do número recomendado pode aumentar a latência de E/S do host; enquanto
que a profundidade da fila abaixo do número recomendado pode reduzir o desempenho de E/S do host. Como cada
aplicativo é diferente, ajuste e observação são necessários para atingir o desempenho máximo de armazenamento.

A profundidade da fila do host geralmente é ajustada no driver do adaptador de barramento do host ou no
hypervisor e às vezes no aplicativo. Os padrões, como 32 ou 64, podem causar latência excessiva do host ou
do aplicativo.

Se um host ou hypervisor estiver usando múltiplas camadas de desempenho, use a profundidade da fila para
a camada mais rápida e observe a latência na camada de desempenho mais lenta. Se a latência na camada mais baixa for inaceitável, ajuste a profundidade da fila até que o balanceamento
da latência e do desempenho seja observado em todas as camadas.

<table align="center">
	<tbody>
		<tr>
			<td><strong>Camada de desempenho</strong></td>
			<td style="text-align: center; vertical-align: middle;">0,25 IOPS por GB</td>
			<td style="text-align: center; vertical-align: middle;">2 IOPS por GB</td>
			<td style="text-align: center; vertical-align: middle;">4 IOPS por GB</td>
		</tr>
		<tr>
			<td><strong>Profundidade máxima da fila do host</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
