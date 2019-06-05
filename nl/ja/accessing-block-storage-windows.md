---

copyright:
  years: 2014, 2019
lastupdated: "2019-02-05"

keywords: MPIO iSCSI LUNS, iSCSI Target, MPIO, multipath, block storage, LUN, mounting, mapping secondary storage

subcollection: BlockStorage

---
{:external: target="_blank" .external}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}

# Microsoft Windows での iSCSI LUN への接続
{: #mountingWindows}

開始する前に、{{site.data.keyword.blockstoragefull}} ボリュームにアクセスするホストが、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}を介して許可されていることを確認してください。

1. {{site.data.keyword.blockstorageshort}} のリスト・ページで、新規ボリュームを見つけ、**「アクション」**をクリックします。 **「ホストの許可」**をクリックします。
2. リストから、ボリュームにアクセスするホストを選択し、**「送信」**をクリックします。

あるいは、SLCLI を使用してホストを許可できます。
```
# slcli block access-authorize --help
Usage: slcli block access-authorize [OPTIONS] VOLUME_ID

Options:
  -h, --hardware-id TEXT    The id of one SoftLayer_Hardware to authorize
  -v, --virtual-id TEXT     The id of one SoftLayer_Virtual_Guest to authorize
  -i, --ip-address-id TEXT  The id of one SoftLayer_Network_Subnet_IpAddress
                            to authorize
  --ip-address TEXT         An IP address to authorize
  --help                    Show this message and exit.
```
{:codeblock}

## {{site.data.keyword.blockstorageshort}} ボリュームのマウント
{: #mountWin}

以下に、Windows ベースの {{site.data.keyword.cloud}} コンピューティング・インスタンスをマルチパス入出力 (MPIO) internet Small Computer System Interface (iSCSI) 論理装置番号 (LUN) に接続するために必要なステップを示します。 この例は、Windows Server 2012 に基づいています。 その他の Windows バージョンの場合、オペレーティング・システム (OS) のベンダーの資料に従って、ステップを調整してください。

### MPIO 機能の構成

1. 「サーバー マネージャ」を開始し、**「管理」**、**「役割と機能の追加」**を参照します。
2. **「次へ」**をクリックし、「機能」メニューに進みます。
3. スクロールダウンし、**「マルチパス I/O」**にチェック・マークを付けます。
4. **「インストール」**をクリックして、MPIO をホスト・サーバーにインストールします。
![「サーバー マネージャ」での「役割と機能の追加」](/images/Roles_Features.png)

### MPIO の iSCSI サポートの追加

1. **「スタート」**をクリックし、**「管理ツール」**を指し、**「MPIO」**をクリックして、MPIO プロパティーのウィンドウを開きます。
2. **「マルチパスの検出」**をクリックします。
3. **「iSCSI デバイスのサポートを追加する」**にチェック・マークを付け、**「追加」**をクリックします。 コンピューターの再始動を求めるプロンプトが出されたら、**「はい」**をクリックします。

Windows Server 2008 では、iSCSI のサポートを追加すると、Microsoft Device Specific Module (MSDSM) が MPIO 対応のすべての iSCSI デバイスを要求できるようになります。ただし、そのためには、最初に iSCSI ターゲットに接続する必要があります。
{:note}

### iSCSI イニシエーターの構成

1. 「サーバー マネージャ」から iSCSI イニシエーターを開始し、**「ツール」**、**「iSCSI イニシエーター」**を選択します。
2. **「構成」**タブをクリックをします。
    - 「イニシエーター名」フィールドには、`iqn.1991-05.com.microsoft:` のような項目が既に取り込まれている場合があります。
    - **「変更」** をクリックして、既存の値をご使用の iSCSI 修飾名 (IQN) に置き換えます。![iSCSI イニシエーターのプロパティー](/images/iSCSI.png)

      IQN 名は、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}の「{{site.data.keyword.blockstorageshort}} 詳細」画面で取得できます。
      {: tip}

    - **「探索」**タブをクリックし、**「ポータルの探索」**をクリックします。
    - iSCSI ターゲットの IP アドレスを入力し、ポートはデフォルト値の 3260 のままにします。
    - **「詳細設定」**をクリックして、「詳細設定」ウィンドウを開きます。
    - **「CHAP ログオンを有効にする」**を選択して、CHAP 認証をオンにします。
    ![CHAP ログオンを有効にする](/images/Advanced_0.png)

    「名前」フィールドと「ターゲット シークレット」フィールドでは、大/小文字が区別されます。
    {:important}
         - **「名前」**フィールドで、既存のエントリーをすべて削除し、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}から取得したユーザー名を入力します。
         - **「ターゲット シークレット」**フィールドに、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}から取得したパスワードを入力します。
    - **「詳細設定」**ウィンドウおよび**「ターゲット ポータルの探索」**ウィンドウで**「OK」**をクリックして、メインの「iSCSI イニシエーターのプロパティ」画面に戻ります。 認証エラーを受け取った場合は、ユーザー名とパスワードの項目を確認してください。
    ![非アクティブ・ターゲット](/images/Inactive_0.png)

    ターゲットの名前は、「検出されたターゲット」セクションに`非アクティブ`状態と表示されます。
    {:note}


### ターゲットのアクティブ化

1. **「接続」**をクリックして、ターゲットに接続します。
2. **「複数パスを有効にする」**チェック・ボックスを選択して、ターゲットへのマルチパス IO を有効にします。
<br/>
   ![複数パスを有効にする](/images/Connect_0.png)
3. **「詳細設定」**をクリックし、**「CHAP ログオンを有効にする」**を選択します。
</br>
   ![CHAP ログオンを有効にする](/images/chap_0.png)
4. 「名前」フィールドにユーザー名を入力し、「ターゲット シークレット」フィールドにパスワードを入力します。

   「名前」および「ターゲット シークレット」フィールドの値は、「{{site.data.keyword.blockstorageshort}} 詳細」画面から取得できます。
   {:tip}
5. **「iSCSI イニシエーターのプロパティ」**ウィンドウが表示されるまで**「OK」**をクリックします。 **「検出されたターゲット」**セクションのターゲットの状況が、**「非アクティブ」**から**「接続完了」**に変わります。
![「接続完了」状況](/images/Connected.png)


### iSCSI イニシエーターでの MPIO の構成

1. iSCSI イニシエーターを開始し、「ターゲット」タブで**「プロパティ」**をクリックします。
2. 「プロパティ」ウィンドウで**「セッションの追加」**をクリックして、「ターゲットへの接続」ウィンドウを開きます。
3. 「ターゲットへの接続」ダイアログ・ボックスで、**「複数パスを有効にする」**チェック・ボックスを選択し、**「詳細設定...」**をクリックします。
  ![ターゲット](/images/Target.png)

4. 「詳細設定」ウィンドウの ![設定](/images/Settings.png) で、以下のようにします。
   - 「ローカル アダプタ」リストで、「Microsoft iSCSI イニシエーター」を選択します。
   - 「イニシエーター IP」リストで、ホストの IP アドレスを選択します。
   - 「ターゲット ポータル IP」リストで、デバイス・インターフェースの IP を選択します。
   - **「CHAP ログオンを有効にする」**チェック・ボックスをクリックします。
   - ポータルから取得した「名前」と「ターゲット シークレット」の値を入力し、**「OK」**をクリックします。
   - 「ターゲットへの接続」ウィンドウで**「OK」**をクリックして、「プロパティ」ウィンドウに戻ります。

5. **「プロパティ」**をクリックします。 「プロパティ」ダイアログ・ボックスで、再度**「セッションの追加」**をクリックし、2 番目のパスを追加します。
6. 「ターゲットへの接続」ダイアログ・ボックスで、**「複数パスを有効にする」**チェック・ボックスを選択します。 **「詳細設定」**をクリックします。
7. 「詳細設定」ウィンドウで、以下のようにします。
   - 「ローカル アダプタ」リストで、「Microsoft iSCSI イニシエーター」を選択します。
   - 「イニシエーター IP」リストで、ホストに対応する IP アドレスを選択します。 この場合、ストレージ・デバイス上の 2 つのネットワーク・インターフェースをホスト上の単一のネットワーク・インターフェースに接続します。 したがって、このインターフェースは、最初のセッションで提供されたものと同じです。
   - 「ターゲット ポータル IP」リストで、ストレージ・デバイスで有効になっている 2 番目のデータ・インターフェースの IP アドレスを選択します。

     2 番目の IP アドレスは、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}の「{{site.data.keyword.blockstorageshort}} の詳細」画面にあります。
      {: tip}
   - **「CHAP ログオンを有効にする」**チェック・ボックスをクリックします。
   - ポータルから取得した「名前」と「ターゲット シークレット」の値を入力し、**「OK」**をクリックします。
   - 「ターゲットへの接続」ウィンドウで**「OK」**をクリックして、「プロパティ」ウィンドウに戻ります。
8. 「プロパティー」ウィンドウには、「ID」ペイン内に複数のセッションが表示されています。 iSCSI ストレージには複数のセッションがあります。

   ISCSI ストレージに接続する複数のインターフェースがホストにある場合は、「イニシエーター IP」フィールドに他の NIC の IP アドレスを使用して別の接続を設定できます。 ただし、接続を試行する前に、[{{site.data.keyword.slportal}}](https://control.softlayer.com/){: external}で 2 番目のイニシエーター IP アドレスを許可してください。
   {:note}
9. 「プロパティ」ウィンドウで、**「デバイス」**をクリックして、「デバイス」ウィンドウを開きます。 デバイス・インターフェース名は `mpio` で始まります。 <br/>
  ![デバイス](/images/Devices.png)

10. **「MPIO」**をクリックして、**「デバイスの詳細」**ウィンドウを開きます。 このウィンドウで、MPIO のロード・バランシング・ポリシーを選択でき、iSCSI へのパスが表示されます。 この例では、MPIO で使用可能な 2 つのパスと、「サブセットのラウンドロビン」ロード・バランシング・ポリシーが表示されています。
  ![「デバイスの詳細」ウィンドウに、MPIO で使用可能な 2 つのパスと、「サブセットのラウンドロビン」ロード・バランシング・ポリシーが表示されています](/images/DeviceDetails.png)

11. **「OK」**を数回クリックして、iSCSI イニシエーターを終了します。



## Windows オペレーティング・システムで MPIO が正しく構成されているかどうかの検証
{: #verifyMPIOWindows}

Windows MPIO が構成されているかどうかを検証するには、まず MPIO アドオンが使用可能であることを確認し、サーバーを再始動します。

![Roles_Features_0](/images/Roles_Features_0.png)

再始動が完了し、ストレージ・デバイスが追加されると、MPIO が構成済みで機能しているかどうかを検証できます。 これを行うには、ターゲットの**「デバイスの詳細」**を参照し、**「MPIO」**をクリックします。
![DeviceDetails_0](/images/DeviceDetails_0.png)

MPIO が正しく構成されていないと、ネットワーク障害が発生した場合や、{{site.data.keyword.cloud}} チームが保守を実行するときに、ストレージ・デバイスが切断され、使用不可と表示されます。 MPIO では、そのようなイベントが発生しても追加の接続レベルが保証され、LUN への読み取り/書き込み操作がアクティブな状態で、確立済みセッションが保持されます。

## {{site.data.keyword.blockstorageshort}} ボリュームのアンマウント
{: #unmountingWin}

以下は、MPIO iSCSI LUN から Windows ベースの {{site.data.keyword.Bluemix_short}} コンピューティング・インスタンスを切断するために必要なステップです。 この例は、Windows Server 2012 に基づいています。 その他の Windows バージョンの場合、OS ベンダーの資料に従って、ステップを調整してください。

### iSCSI イニシエーターの開始

1. **「ターゲット」**タブをクリックします。
2. 削除するターゲットを選択し、**「切断」**をクリックします。

### ターゲットの削除
このステップは、iSCSI ターゲットにアクセスする必要がなくなった場合には省略可能です。

1. iSCSI イニシエーターの**「探索」**をクリックします。
2. ストレージ・ボリュームに関連付けられたターゲット・ポータルを強調表示し、**「削除」**をクリックします。
