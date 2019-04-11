---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-07"

keywords: Block Storage, IOPS, Security, Encryption, LUN, secondary storage, mount storage, provision storage, ISCSI, MPIO, redundant

subcollection: BlockStorage

---
{:new_window: target="_blank"}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:shortdesc: .shortdesc}

# 入門チュートリアル
{: #getting-started}

{{site.data.keyword.blockstoragefull}} は、コンピューティング・インスタンスから独立してプロビジョンおよび管理される、永続的で高性能な iSCSI ストレージです。 iSCSI ベースの {{site.data.keyword.blockstorageshort}} LUN は、冗長マルチパス入出力 (MPIO) 接続を介して、許可されたデバイスに接続されます。

{{site.data.keyword.blockstorageshort}} は、他に類のない機能によりクラス最高レベルの耐久性および可用性をもたらします。 この製品は、業界標準とベスト・プラクティスを使用して構築されています。 {{site.data.keyword.blockstorageshort}} は、保守イベントや計画外の障害においてもデータの保全性を保護し、可用性を維持し、一貫性のあるパフォーマンス・ベースラインを提供するように設計されています。
{:shortdesc}

## 始めに
{: #prereqs}

{{site.data.keyword.blockstorageshort}} LUN は、20 GB から 12 TB までプロビジョンできます。これには次の 2 つのオプションがあります。 <br/>
- 事前定義されたパフォーマンス・レベル、およびスナップショットやレプリケーションなどの機能を備えた**エンデュランス**層をプロビジョンする。
- 割り振り済みの 1 秒当たりの入出力操作 (IOPS) を使用して高出力の**パフォーマンス**環境を構築する。

{{site.data.keyword.blockstorageshort}} のオファリングについて詳しくは、[{{site.data.keyword.blockstorageshort}} 製品情報](/docs/infrastructure/BlockStorage?topic=BlockStorage-About)を参照してください。

## プロビジョニングの考慮事項

### ブロック・サイズ

エンデュランスとパフォーマンスの IOPS はどちらも、16-KB のブロック・サイズと、読み取り/書き込みの比率が 50/50 でランダム/順次の比率が 50/50 のワークロードに基づいています。 16 KB ブロックは、ボリュームへの 1 回の書き込みに相当します。
{:important}

アプリケーションが使用するブロック・サイズは、ストレージのパフォーマンスに直接影響します。 アプリケーションが使用するブロック・サイズが 16 KB より小さい場合、スループット限度に達する前に IOPS 限度に到達します。 逆に、アプリケーションが使用するブロック・サイズが 16 KB より大きい場合、IOPS 限度に達する前にスループット限度に到達します。

<table>
  <caption>表 4 は、ブロック・サイズと IOPS によるスループットへの影響の例を示しています。</caption>
        <colgroup>
          <col/>
          <col/>
          <col/>
        </colgroup>
        <thead>
          <tr>
            <th>ブロック・サイズ (KB)</th>
            <th>IOPS</th>
            <th>スループット (MB/秒)</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>4 (Linux の場合の標準)</td>
            <td>1,000</td>
            <td>4</td>
          </tr>
          <tr>
            <td>8 (Oracle の場合の標準)</td>
            <td>1,000</td>
            <td>8</td>
          </tr>
          <tr>
            <td>16</td>
            <td>1,000</td>
            <td>16</td>
          </tr>
          <tr>
            <td>32 (SQL Server の場合の標準)</td>
            <td>500</td>
            <td>16</td>
          </tr>          
          <tr>
            <td>64</td>
            <td>250</td>
            <td>16</td>
          </tr>
          <tr>
            <td>128</td>
            <td>128</td>
            <td>16</td>
          </tr>
          <tr>
            <td>512</td>
            <td>32</td>
            <td>16</td>
          </tr>
        </tbody>
</table>

### 許可ホスト

考慮すべきもう 1 つの要因は、ボリュームを使用しているホストの数です。 ボリュームにアクセスしているホストが 1 つだけの場合は、使用可能な最大 IOPS を実現するのは困難な場合があります (特に、IOPS カウントが 10,000 などのように極端に多い場合)。 ワークロードで高スループットが必要な場合は、シングル・サーバーのボトルネックを回避するために、少なくとも 2 台のサーバーがボリュームにアクセスできるように構成することをお勧めします。

### ネットワーク接続

イーサネット接続の速度は、ボリュームから予想される最大スループットよりも高速でなければなりません。 一般的に、イーサネット接続が、使用可能な帯域幅の 70% を超えることはありません。 例えば、6,000 IOPS で 16 KB ブロック・サイズを使用している場合、ボリュームは約 94 MB/秒のスループットを処理できます。 LUN への 1 Gbps イーサネット接続がある場合、使用可能な最大スループットをサーバーが使用しようとすると、ボトルネックが発生します。 その理由は、1 Gbps イーサネット接続の理論上の限度 (125 MB/秒) の 70% では、88 MB/秒しか使用できないからです。

最大 IOPS を実現するには、十分なネットワーク・リソースを用意する必要があります。 その他の考慮事項として、ストレージ外の専用ネットワーク使用、およびホスト・サイドおよびアプリケーション固有のチューニング (IP スタック、[キュー項目数](/docs/infrastructure/BlockStorage?topic=BlockStorage-hostqueuesettings)、およびその他の設定) があります。

ストレージ・トラフィックは、パブリック仮想サーバーの合計ネットワーク使用量に含まれます。 このサービスで設定されている制限について詳しくは、[Virtual Server の資料](/docs/vsi?topic=virtual-servers-public-virtual-servers)を参照してください。
{:tip}

## 注文の送信
{: #submitorder}

注文を送信する準備ができたら、[コンソール](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughConsole)または [SLCLI](/docs/infrastructure/BlockStorage?topic=BlockStorage-orderingthroughCLI) を通してその注文を出すことができます。

## 新規ストレージの接続
{: #mountingstorage}

プロビジョニング要求が完了したら、ホストに対して新規ストレージへのアクセスを許可し、接続を構成します。 ホストのオペレーティング・システムに応じて、適切なリンクをたどってください。
- [Linux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingLinux)
- [CloudLinux での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingCloudLinux)
- [Microsoft Windows での LUN への接続](/docs/infrastructure/BlockStorage?topic=BlockStorage-mountingWindows)
- [cPanel を使用したバックアップ用のブロック・ストレージの構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-cPanelBackups)
- [Plesk を使用したバックアップ用のブロック・ストレージの構成](/docs/infrastructure/BlockStorage?topic=BlockStorage-PleskBackups)

## 新しいストレージの管理

ポータルまたは SLCLI により、ホストの許可や取り消しなど、ファイル・ストレージのさまざまな側面を管理できます。詳しくは、[{{site.data.keyword.blockstorageshort}}の管理](/docs/infrastructure/BlockStorage?topic=BlockStorage-managingstorage)を参照してください。
