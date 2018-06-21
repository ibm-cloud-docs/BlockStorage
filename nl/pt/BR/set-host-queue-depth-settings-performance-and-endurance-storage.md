---

copyright:
  years: 2014, 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# Recomendação para configurações de profundidade da fila do host

O {{site.data.keyword.BluSoftlayer_full}} recomenda uma profundidade máxima da fila de entrada/saída (E/S) do aplicativo e do host para cada camada de desempenho. A configuração do host não afeta a latência
de disco e do controlador, apenas a latência observada pelo host e pelo aplicativo.

<table align="center">
  <caption>Profundidade da fila recomendada para cada camada de IOPS</caption>
        <thead>
	    <tr>
		<th>Camada de desempenho</th>
		<th>Profundidade máxima da fila do host</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">0,25 IOPS por GB</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">2 IOPS por GB</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">4 IOPS por GB</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

A profundidade da fila acima do número recomendado pode aumentar a latência de E/S do host; enquanto
que a profundidade da fila abaixo do número recomendado pode reduzir o desempenho de E/S do host. Como cada aplicativo é diferente, o ajuste e a observação são necessários para alcançar o desempenho máximo de armazenamento.

A profundidade da fila do host é geralmente ajustada no driver adaptador de barramento de host ou no hypervisor e, às vezes, no aplicativo. Os padrões, como 32 ou 64, podem causar latência excessiva do host ou
do aplicativo.

Se um host ou hypervisor estiver usando múltiplas camadas de desempenho, use a profundidade da fila para
a camada mais rápida e observe a latência na camada de desempenho mais lenta. Se a latência na camada mais baixa for inaceitável, ajuste a profundidade da fila até que o balanceamento da latência e do desempenho seja alcançado em todas as camadas.
