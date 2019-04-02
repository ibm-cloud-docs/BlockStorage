---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: Block Storage, use of a Block Storage volume, LUN, Block Storage

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

## {{site.data.keyword.blockstorageshort}} ボリュームを共有できるインスタンスの数はいくつですか?
{: faq}

ブロック・ボリュームごとに許可される数の制限は、デフォルトでは 8 です。 つまり、最大 8 個のホストがブロック・ストレージ LUN にアクセスする権限を持つことができます。 制限の引き上げを要求する場合は、営業担当員にお問い合わせください。

## 注文できるボリュームの数はいくつですか?
{: faq}

デフォルトでは、合計 250 の {{site.data.keyword.blockstorageshort}} ボリュームをプロビジョンできます。 ボリュームの制限を引き上げる場合は、営業担当員にお問い合わせください。 詳しくは、[ストレージの制限の管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstoragelimits)を参照してください。

## ホストにマウントできる {{site.data.keyword.blockstorageshort}} ボリュームの数はいくつですか?
{: faq}

これはホスト・オペレーティング・システムが処理できる内容によって異なりますが、{{site.data.keyword.BluSoftlayer_full}} が制限するものではありません。 マウント可能なボリューム数の制限については、お使いの OS の資料を参照してください。

## Block Storage LUN に対してどの Windows バージョンを選択すればよいですか?
{: faq}

LUN を作成するときに、OS タイプを指定する必要があります。 OS タイプは、LUN にアクセスするホストが使用するオペレーティング・システムに基づいている必要があります。 LUN の作成後に OS タイプを変更することはできません。 LUN の実際のサイズは、LUN の OS タイプによって若干異なる場合があります。

**Windows 2008+**
- LUN は、Windows 2008 以降のバージョンの Windows データを保管します。 ホスト・オペレーティング・システムが Windows Server 2008、Windows Server 2012、Windows Server 2016 の場合は、この OS オプションを使用します。 MBR と GPT の両方のパーティショニング方式がサポートされています。

**Windows 2003**
- LUN は、マスター・ブート・レコード (MBR、Master Boot Record) パーティショニング・スタイルを使用して、単一パーティションの Windows ディスクにロー・ディスク・タイプを保管します。 このオプションは、ホスト・オペレーティング・システムが、MBR パーティショニング方式を使用する Windows 2000 Server、Windows Server XP、または Windows Server 2003 である場合にのみ使用します。

**Windows GPT**
-  LUN は、GUID パーティション・タイプ (GPT、GUID Partition Type) パーティショニング・スタイルを使用して、Windows データを保管します。 このオプションは、GPT パーティショニング方式を使用しようとしていて、ホストでその方式の使用が可能な場合に使用します。 Windows Server 2003 (Service Pack 1 以降) では GPT パーティショニング方式を使用でき、64 ビット版のすべての Windows はこの方式をサポートしています。

## 割り振られる IOPS の制限はインスタンスとボリュームのいずれに従って適用されますか?
{: faq}

IOPS は、ボリューム・レベルで適用されます。 すなわち、6000 IOPS を使用できるボリュームに接続されるホストが 2 台あれば、それらのホストは 6000 IOPS を共有します。

## IOPS の測定
{: faq}

IOPS は、ランダムな 50% の読み取りと 50% の書き込みを使って、16 KB のブロックのロード・プロファイルに基づいて測定されます。 このプロファイルと異なるワークロードでは、パフォーマンスが低下する可能性があります。

## 小さいブロック・サイズを使用してパフォーマンスを測定するとどうなりますか?
{: faq}

小さいブロック・サイズを使用しても、最大の IOPS が得られます。 ただし、スループットは小さくなります。 例えば、6000 IOPS のボリュームの場合、さまざまなブロック・サイズでのスループットは以下のようになります。

- 16 KB * 6000 IOPS == ~93.75 MB/秒
- 8 KB * 6000 IOPS == ~46.88 MB/秒
- 4 KB * 6000 IOPS == ~23.44 MB/秒

## 期待するスループットを実現するために、ボリュームを事前にウォーム状態にしておく必要がありますか?
{: faq}

事前にウォーム状態にしておく必要はありません。 ボリュームをプロビジョンすると即時に、示されたスループットが達成されます。

## より高速なイーサネット接続を使用することによって、より多くのスループットを達成できますか?
{: faq}

スループットの限度は、LUN レベルで設定されるため、より高速なイーサネット接続を使用しても、設定された上限は増えません。 ただし、低速イーサネット接続では、帯域幅がボトルネックとなる可能性があります。

## ファイアウォールおよびセキュリティー・グループはパフォーマンスに影響しますか?
{: faq}

ファイアウォールをバイパスするように VLAN 上でストレージ・トラフィックを実行することをお勧めします。 ソフトウェア・ファイアウォールを介してストレージ・トラフィックを実行すると、待ち時間が増加し、ストレージ・パフォーマンスに悪影響を与えます。

## {{site.data.keyword.blockstorageshort}} からの待ち時間はどのくらいですか?   
{: faq}

ストレージ内のターゲット待ち時間は <1 ms です。 ストレージは共有ネットワーク上のコンピューティング・インスタンスに接続されるため、正確なパフォーマンス待ち時間は、操作中のネットワーク・トラフィックによって異なります。

## エンデュランス 10 IOPS/GB 層の {{site.data.keyword.blockstorageshort}} を注文できるデータ・センターと、できないデータ・センターがあるのはなぜですか?
{: faq}

エンデュランス・タイプの 10 IOPS/GB 層 {{site.data.keyword.blockstorageshort}} は、限定されたデータ・センターのみで使用可能ですが、使用可能な新しいデータ・センターが徐々に追加されています。 アップグレードされたデータ・センターと使用可能な機能の完全なリストを[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)で見つけることができます。

## どの {{site.data.keyword.blockstorageshort}} ボリュームが暗号化されているかを知る方法はありますか?
{: faq}

[{{site.data.keyword.slportal}} ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://control.softlayer.com/){:new_window} で {{site.data.keyword.blockstorageshort}} のリストを表示したとき、暗号化されている LUN のボリューム名の横にロック・アイコンがあります。

## アップグレードされたデータ・センターで {{site.data.keyword.blockstorageshort}} をプロビジョンしていることはどこで分かりますか?
{: faq}

{{site.data.keyword.blockstorageshort}} を注文すると、アップグレードされたデータ・センターはすべて、注文フォームにアスタリスク (`*`) が表示されるほか、暗号化機能を備えたストレージをプロビジョンしようとしていることが示されます。 ストレージがプロビジョンされると、ストレージ・リストに、そのストレージが暗号化されていることを示すアイコンが表示されます。 暗号化された LUN はすべて、アップグレードされたデータ・センターでのみプロビジョンされます。 アップグレードされたデータ・センターと使用可能な機能の完全なリストを[ここ](/docs/infrastructure/BlockStorage?topic=BlockStorage-news)で見つけることができます。

## 最近アップグレードされたデータ・センターに暗号化されていない {{site.data.keyword.blockstorageshort}} を所有している場合は、その {{site.data.keyword.blockstorageshort}} を暗号化できますか?
{: faq}

データ・センターのアップグレード前にプロビジョンされた {{site.data.keyword.blockstorageshort}} は、暗号化できません。
アップグレードされたデータ・センターで新たにプロビジョンされた {{site.data.keyword.blockstorageshort}} は、自動的に暗号化されます。 つまり、暗号化するかどうかを選択する設定はありません。自動で暗号化されます。
アップグレードされたデータ・センター内にある非暗号化ストレージ上のデータを暗号化するには、新しいブロック LUN を作成してから、ホスト・ベースのマイグレーションを使用してそのデータを暗号化された新しい LUN にコピーします。 手順については、[ここ](migrate-block-storage-encrypted-block-storage.html)をクリックしてください。

## {{site.data.keyword.blockstorageshort}} は、DB2 pureScale 用に I/O フェンシングを実装するための SCSI-3 永続予約をサポートしますか?
{: faq}

はい。{{site.data.keyword.blockstorageshort}} は、SCSI-2 と SCSI-3 永続予約の両方をサポートします。

## {{site.data.keyword.blockstorageshort}} LUN が削除されると、データはどうなりますか?
{: faq}

{{site.data.keyword.blockstoragefull}} は、再使用前にワイプされた物理ストレージ上のお客様にブロック・ボリュームを提供します。 メディアのサニタイズに関する NIST 800-88 ガイドラインなどのコンプライアンスに関する特別な要件をお持ちのお客様は、ストレージを削除する前にデータのサニタイズ手順を実行する必要があります。

## クラウド・データ・センターから使用禁止にされたドライブはどうなりますか?
{: faq}

ドライブが使用禁止になると、IBM は、それらのドライブが処分される前に、破棄します。 ドライブは使用できなくなります。 ドライブに書き込まれたすべてのデータにアクセスできなくなります。
