---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, performance tuning, host performance improvement,

subcollection: BlockStorage

---
{:external: target="_blank" .external}

# ホスト・キュー項目数の設定の調整
{: #hostqueuesettings}

{{site.data.keyword.BluSoftlayer_full}} では、パフォーマンス層ごとに最大ホストおよびアプリケーション入出力 (I/O) キュー項目数を示しています。

<table align="center">
  <caption>各 IOPS 層の推奨キュー項目数</caption>
        <thead>
	    <tr>
		<th>パフォーマンス層</th>
		<th>最大ホスト・キュー項目数</th>
	    </tr>
	</thead>
	<tbody>
   	    <tr>
		<td style="text-align: center; vertical-align: middle;">GB 当たり 0.25 IOPS</td>
		<td style="text-align: center; vertical-align: middle;">8</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">GB 当たり 2 IOPS</td>
		<td style="text-align: center; vertical-align: middle;">24</td>
	    </tr>
	    <tr>
		<td style="text-align: center; vertical-align: middle;">GB 当たり 4 IOPS</td>
		<td style="text-align: center; vertical-align: middle;">56</td>
            </tr>
         </tbody>
</table>

ホスト設定は、ディスクおよびコントローラーの待ち時間には影響しません。 これは、ホストおよびアプリケーションによって監視される待ち時間にのみ影響します。

リストされている数を超えるキュー項目数を指定すると、ホストの入出力待ち時間が増加する可能性があります。一方、リストされている数より少ないキュー項目数を指定すると、ホストの入出力パフォーマンスが低下する可能性があります。 各アプリケーションによって違いがあるため、最大のストレージ・パフォーマンスを実現するには、調整と監視が必要です。

一般に、ホスト・キュー項目数は、ホスト・バス・アダプター・ドライバーまたはハイパーバイザー、および場合によってはアプリケーションで調整されます。 32 や 64 などの標準的なデフォルト値では、ホストやアプリケーションの待ち時間が過剰になる可能性があります。

1 つのホストまたはハイパーバイザーが複数のパフォーマンス層を使用している場合は、最高速の層のキュー項目数を使用し、最低速のパフォーマンス層で待ち時間を監視します。

最低速の層の待ち時間が許容できない場合は、すべての層での待ち時間とパフォーマンスのバランスがとれるまでキュー項目数を調整します。
