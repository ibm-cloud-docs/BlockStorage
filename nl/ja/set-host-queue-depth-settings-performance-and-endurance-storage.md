---

copyright:
  years: 2014, 2018
lastupdated: "2018-02-14"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}

# ホスト・キュー項目数の設定に関する推奨事項

{{site.data.keyword.BluSoftlayer_full}} では、パフォーマンス層ごとに最大ホストおよびアプリケーション入出力 (I/O) キュー項目数を推奨しています。ホスト設定は、ディスクおよびコントローラーの待ち時間には影響せず、ホストおよびアプリケーションによって監視される待ち時間のみに影響します。

推奨される数を超えるキュー項目数を指定すると、ホストの入出力待ち時間が増加する可能性があります。一方、推奨される数より少ないキュー項目数を指定すると、ホストの入出力パフォーマンスが低下する場合があります。各アプリケーションによって違いがあるため、最大のストレージ・パフォーマンスを実現するには、調整と監視が必要です。

一般に、ホスト・キュー項目数は、ホスト・バス・アダプター・ドライバーまたはハイパーバイザー、および場合によってはアプリケーションで調整されます。32 や 64 などの標準的なデフォルト値では、ホストやアプリケーションの待ち時間が過剰になる可能性があります。

1 つのホストまたはハイパーバイザーが複数のパフォーマンス層を使用している場合は、最高速の層のキュー項目数を使用し、最低速のパフォーマンス層で待ち時間を監視します。最低速の層の待ち時間が許容できない場合は、すべての層に対する待ち時間とパフォーマンスのバランスがとれるまでキュー項目数を調整します。

<table align="center">
	<tbody>
		<tr>
			<td><strong>パフォーマンス層</strong></td>
			<td style="text-align: center; vertical-align: middle;">GB 当たり 0.25 IOPS</td>
			<td style="text-align: center; vertical-align: middle;">GB 当たり 2 IOPS</td>
			<td style="text-align: center; vertical-align: middle;">GB 当たり 4 IOPS</td>
		</tr>
		<tr>
			<td><strong>最大ホスト・キュー項目数</strong></td>
			<td style="text-align: center; vertical-align: middle;">8</td>
			<td style="text-align: center; vertical-align: middle;">24</td>
			<td style="text-align: center; vertical-align: middle;">56</td>
		</tr>
	</tbody>
</table>
