---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-28"

keywords:

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}

# {{site.data.keyword.blockstorageshort}} 製品情報
{: #About}

{{site.data.keyword.blockstoragefull}} は、コンピューティング・インスタンスから独立してプロビジョンおよび管理される、永続的で高性能な iSCSI ストレージです。 iSCSI ベースの {{site.data.keyword.blockstorageshort}} LUN は、冗長マルチパス入出力 (MPIO) 接続を介して、許可されたデバイスに接続されます。

{{site.data.keyword.blockstorageshort}} は、他に類のない機能セットによりクラス最高レベルの耐久性および可用性をもたらします。 この製品は、業界標準とベスト・プラクティスを使用して構築されています。 {{site.data.keyword.blockstorageshort}} は、保守イベントや計画外の障害においてもデータの保全性を保護し、可用性を維持し、一貫性のあるパフォーマンス・ベースラインを提供するように設計されています。

## コア機能
{: #corefeatures}

{{site.data.keyword.blockstorageshort}} では以下の機能を利用できます。

- **一貫性のあるパフォーマンス・ベースライン**
   - 個々のボリュームに対するプロトコル・レベルの IOPS の割り振りによって提供されます。
- **強力な耐久性と回復力**
   - データの保全性を保護し、保守イベントおよび計画外の障害の際に可用性を維持します。そのため、独立ディスク (RAID) アレイを使用してオペレーティング・システム・レベルの冗長アレイを作成および管理する必要はありません。
- **Data at Rest 暗号化** ([選択データ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - Data at Rest (保存されたデータ) のためのプロバイダー管理の暗号化を追加コストなしで利用できます。
- **オール・フラッシュ・ストレージによるバッキング** ([選択データ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - 2 IOPS/GB 以上のレベルの「エンデュランス」または「パフォーマンス」でプロビジョンされるボリューム用のオール・フラッシュ・ストレージ。
- **スナップショット** ([選択データ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - 処理を中断せずに、特定時点のデータ・スナップショットをキャプチャーします。
- **レプリケーション** ([選択データ・センターで使用可能](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations))
   - パートナーの {{site.data.keyword.BluSoftlayer_full}} データ・センターにスナップショットを自動的にコピーします。
- **可用性の高い接続**
   - 冗長ネットワーキング接続を使用して可用性を最大化します。
   - iSCSI ベースの {{site.data.keyword.blockstorageshort}} では、マルチパス入出力 (MPIO) を使用します。
- **同時アクセス**
   - クラスター構成の場合、複数のホストがブロック・ボリューム (最大 8 個) に同時にアクセスできます。
- **クラスター・データベース**
   - クラスター・データベースなどの高度なユース・ケースをサポートします。

## 請求
{: #billing}

ブロック LUN に対して、1 時間ごとまたは月ごとの請求処理を選択できます。 LUN に対して選択した請求のタイプは、その LUN のスナップショット・スペースおよびレプリカに適用されます。 例えば、毎時請求を指定して LUN をプロビジョンすると、スナップショット料金またはレプリカ料金は時間単位で請求されます。 月次請求を指定して LUN をプロビジョンすると、スナップショット料金またはレプリカ料金は月単位で請求されます。

**毎時請求**では、ブロック LUN がアカウント上に存在していた時間数は、LUN が削除された時点、または請求サイクルの終了時の、どちらか早い方の時点で計算されます。 使用期間が数日ないし 1 カ月未満のストレージには毎時請求が適しています。 毎時請求は、[限定されたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations)にプロビジョンされたストレージに対してのみ使用可能です。

**月次請求**では、作成日から請求サイクルの終了までの料金が日割りで計算され、 即時に請求されます。 請求サイクルの終了前に LUN が削除された場合、返金はありません。 月次請求は、長期間 (1 カ月以上) の保管およびアクセスが必要なデータを利用する実動ワークロードに使用するストレージに適しています。

**パフォーマンス**
<table>
  <caption>表 1 は、月次請求および毎時請求を使用したパフォーマンス・ストレージの価格を示しています。</caption>
  <tr>
   <th>月次価格</th>
   <td>$0.10/GB + $0.07/IOP</td>
  </tr>
  <tr>
   <th>毎時価格</th>
   <td>$0.0001/GB + $0.0002/IOP</td>
  </tr>
</table>

**エンデュランス**
<table>
  <caption>表 2 は、月次および毎時の請求オプションを持つそれぞれの層の、エンデュランス・ストレージの価格を示しています。</caption>
  <tr>
   <th>IOPS ティア</th>
   <th>0.25 IOPS/GB</th>
   <th>2 IOPS/GB</th>
   <th>4 IOPS/GB</th>
   <th>10 IOPS/GB</th>
  </tr>
  <tr>
   <th>月次価格</th>
   <td>$0.06/GB</td>
   <td>$0.15/GB</td>
   <td>$0.20/GB</td>
   <td>$0.58/GB</td>
  </tr>
  <tr>
   <th>毎時価格</th>
   <td>$0.0001/GB</td>
   <td>$0.0002/GB</td>
   <td>$0.0003/GB</td>
   <td>$0.0009/GB</td>
  </tr>
</table>



## プロビジョニング
{: #provisioning}

{{site.data.keyword.blockstorageshort}} LUN は、20 GB から 12 TB までプロビジョンできます。これには次の 2 つのオプションがあります。 <br/>
- 事前定義されたパフォーマンス・レベル、およびスナップショットやレプリケーションなどの機能を備えた**エンデュランス**層をプロビジョンする。
- 割り振り済みの 1 秒当たりの入出力操作 (IOPS) を使用して高出力の**パフォーマンス**環境を構築する。

### エンデュランス層を持つプロビジョニング
{: #provendurance}

「エンデュランス」{{site.data.keyword.blockstorageshort}} は、さまざまなアプリケーションのニーズをサポートするよう 4 つの IOPS パフォーマンス層で使用できます。 <br />

- **0.25 IOPS/GB** は、入出力負荷が低いワークロード用に設計されています。 通常、これらのワークロードは、どの時点においても非アクティブになっているデータの割合が高いという特徴があります。 アプリケーションの例として、メールボックスの保管や部門レベルのファイル共有などが挙げられます。

- **2 IOPS/GB** は、大部分の一般的な用途のために設計されています。 アプリケーションの例として、Web アプリケーションをバッキングする小規模なデータベースのホスティングや、ハイパーバイザーの VM ディスク・イメージが挙げられます。

- **4 IOPS/GB** は、高負荷ワークロード用に設計されています。 通常、これらのワークロードには、いつも大部分のデータがアクティブであるという特徴があります。 アプリケーションの例として、トランザクション・データベースやその他の高いパフォーマンスを必要とするデータベースが挙げられます。

- **10 IOPS/GB** は、NoSQL データベースや Analytics のデータ処理などで作成される最も厳しいワークロード用に設計されています。 この層は、[限定されたデータ・センター](/docs/infrastructure/BlockStorage?topic=BlockStorage-news#new-locations)にプロビジョンされた最大サイズ 4 TB のストレージに対してのみ使用可能です。

12 TB のエンデュランス・ボリュームでは、最大 48,000 IOPS が使用可能です。

ワークロードに見合った適切なエンデュランス層を選択することが重要です。 それと同じくらいに、最高のパフォーマンスを達成するために適切なブロック・サイズ、イーサネット接続速度、およびホストの数を使用することも重要です。 これらの部分のいずれかが他の部分とうまく調整が取れていない場合、結果のスループットに大きな影響を与える可能性があります。


### 「パフォーマンス」でのプロビジョニング
{: #provperformance}

「パフォーマンス」は、エンデュランス層にうまく適合しないがパフォーマンス要件は解明されている高入出力アプリケーションをサポートするように設計された {{site.data.keyword.blockstorageshort}} のクラスです。 予測可能なパフォーマンスは、個々のボリュームに対するプロトコル・レベル IOPS の割り振りによって達成されます。 100 から 48,000 の範囲のさまざまな IOPS レートを、20 GB から 12 TB までの範囲のストレージ・サイズでプロビジョンできます。

{{site.data.keyword.blockstorageshort}} の「パフォーマンス」は、マルチパス入出力 (MPIO) internet Small Computer System Interface (iSCSI) 接続を介してアクセスおよびマウントされます。 {{site.data.keyword.blockstorageshort}}は、通常、単一のサーバーからボリュームにアクセスする場合に使用されます。 複数のボリュームを 1 つのホストにマウントし、まとめてストライピングすることで、ボリュームを大きくし、IOPS 数を増やすことができます。 表 3 のサイズと IOPS レートに従って、Linux、XEN、および Windows の各オペレーティング・システム用のパフォーマンス・ボリュームを注文できます。


<table cellpadding="1" cellspacing="1" style="width: 99%;">
 <caption>表 3 は、パフォーマンス・ストレージのサイズと IOPS の組み合わせを示しています。<br/>限定されたデータ・センターでは、<sup> <img src="/images/numberone.png" alt="脚注" /></sup> 6,000 を超える IOPS 制限が使用可能です。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
          <tr>
            <th>サイズ (GB)</th>
            <th>最小 IOPS</th>
            <th>最大 IOPS</th>
          </tr>
          <tr>
            <td>20</td>
            <td>100</td>
            <td>1,000</td>
          </tr>
          <tr>
            <td>40</td>
            <td>100</td>
            <td>2,000</td>
          </tr>
          <tr>
            <td>80</td>
            <td>100</td>
            <td>4,000</td>
          </tr>
          <tr>
            <td>100</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>250</td>
            <td>100</td>
            <td>6,000</td>
          </tr>
          <tr>
            <td>500</td>
            <td>100</td>
            <td>6,000 または 10,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>1,000</td>
            <td>100</td>
            <td>6,000 または 20,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>2,000 から 3,000</td>
            <td>200</td>
            <td>6,000 または 40,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>4,000 から 7,000</td>
            <td>300</td>
            <td>6,000 または 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>8,000 から 9,000</td>
            <td>500</td>
            <td>6,000 または 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
          <tr>
            <td>10,000 から 12,000</td>
            <td>1,000</td>
            <td>6,000 または 48,000<sup><img src="/images/numberone.png" alt="脚注" /></sup></td>
          </tr>
</table>


パフォーマンス・ボリュームは、プロビジョンされた IOPS レベルに一貫して近いレベルで動作するように設計されています。 一貫性により、特定レベルのパフォーマンスを持つアプリケーション環境のサイズ変更やスケーリングが容易になります。 さらに、理想的な価格対パフォーマンスの比率でボリュームを構築することによって、環境を最適化することもできます。
