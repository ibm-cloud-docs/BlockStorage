---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, performance tuning, host performance improvement,

subcollection: BlockStorage

---
{:external: target="_blank" .external}

# Ajustando as configurações de profundidade da fila do host
{: #hostqueuesettings}

O {{site.data.keyword.cloud}} sugere uma profundidade máxima de fila de entrada/saída (E/S) do host e do aplicativo para cada camada de desempenho.

| Camada de desempenho | Profundidade máxima da fila do host |
|:------:|:------:|
| 0,25 IOPS por GB | 8 |
| 2 IOPS por GB | 24 |
| 4 IOPS por GB | 56 |
{: caption="Profundidade da fila recomendada para cada camada de IOPS" caption-side="top"}

A configuração do host não afeta a latência do disco e do controlador. Ela afeta somente a latência observada pelo host e pelo aplicativo.

A profundidade da fila que excede os números listados pode aumentar a latência de E/S do host, enquanto a profundidade da fila menor do que o número listado pode reduzir o desempenho de E/S do host. Como cada aplicativo é diferente, o ajuste e a observação são necessários para alcançar o desempenho máximo de armazenamento.

A profundidade da fila do host é geralmente ajustada no driver adaptador de barramento de host ou no hypervisor e, às vezes, no aplicativo. Padrões, como 32 ou 64, podem causar latência excessiva de host ou aplicativo.

Se um host ou hypervisor estiver usando múltiplas camadas de desempenho, use a profundidade da fila para
a camada mais rápida e observe a latência na camada de desempenho mais lenta.

Se a latência na camada mais baixa for inaceitável, ajuste a profundidade da fila até que o balanceamento da latência e do desempenho seja alcançado em todas as camadas.
