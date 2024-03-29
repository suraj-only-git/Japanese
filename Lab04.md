


# 目次

- 概要	
- Dataflow Gen2	

    タスク 1: Snowflake のクエリを Dataflow にコピーする	

    タスク 2: Snowflake への接続を作成する	
    \
    タスク 3: Supplier クエリと PO クエリのデータ送信先を構成する

    タスク 4: Snowflake の Dataflow の名前を変更して公開する	

    タスク 5: Dataverse のクエリを Dataflow にコピーする

    タスク 6: Dataverse への接続を作成する	

    タスク 7: Customer クエリのデータ送信先を作成する	

    タスク 8: Dataverse の Dataflow を公開して名前を変更する	

    タスク 9: SharePoint のクエリを Dataflow にコピーする	

    タスク 10: SharePoint への接続を作成する	

    タスク 11: People クエリのデータ送信先を構成する	

    タスク 12: SharePoint の Dataflow を公開して名前を変更する	

- リファレンス	


# 概要

このシナリオでは、Supplier データが Snowflake に、Customer データが Dataverse に、Employee データが SharePoint にあります。これらのデータ ソースはすべて、異なるタイミングで更新されます。Dataflow のデータ更新回数を最小限に抑えるために、データ ソースごとに個別の Dataflow を作成します。

**注**: 1 つの Dataflow は複数のデータ ソースに対応します。

このラボを終了すると、次のことが学べます。

- Dataflow Gen2 を使用して Snowflake に接続し、データを Lakehouse に取り込む方法

- Dataflow Gen2 を使用して SharePoint に接続し、データを Lakehouse に取り込む方法

- Dataflow Gen2 を使用して Dataverse に接続し、データを Lakehouse に取り込む方法


# Dataflow Gen2
## タスク 1: Snowflake のクエリを Dataflow にコピーする

1. ラボ 2 のタスク 8で作成した Fabric ワークスペース **FAIAD_<ユーザー名>** に戻りましょう。

2. トップ メニューから、**新規 -> データフロー (Gen2)** を選択します。
 

**Dataflow のページ**が表示されます。ここまで Dataflow について説明してきました。次は、Power BI Desktop から Dataflow にクエリをコピーしましょう。


3. まだ開いていない場合は、お使いの**ラボ環境のデスクトップの Report** フォルダーにある **FAIAD.pbix** を開きます。

4. リボンから**ホーム -> データの変換**を選択します。Power Query ウィンドウが開きます。前のラボで確認したように、左パネルのクエリはデータ ソースごとに整理されています。

5. Power Query ウィンドウが開きます。左パネルで、SnowflakeData フォルダーにある次のクエリを **Ctrl+Select** または Shift+Select を押しながら選択します。

    a. SupplierCategories

    b. Suppliers

    c. Supplier

    d. PO

    e. PO Line Items

6. **右クリックしてコピー**を選択します。
 
7. **ブラウザー**に戻ります。

8. **Dataflow のペインで中央のペイン**を選択し、**Ctrl+V** を押します (現時点では右クリックの貼り付けには対応していません)。


## タスク 2: Snowflake への接続を作成する

5 つのクエリが貼り付けられ、左側に [クエリ] パネルが表示されています。Snowflake 用の接続が作成されていないため、接続を構成するよう求める警告メッセージが表示されます。

1. **接続の構成**を選択します。
 
2. [データ ソースへの接続] ダイアログが開きます。**接続**ドロップダウンで、**新しい接続の作成**が選択されていることを確認します。

3. **認証の種類は Snowflake** にします。

4. [環境変数] タブ ([ラボ ガイド] タブの横) で確認できる **Snowflake のユーザー名とパスワード**を入力します。

5. **接続**を選択します。
 
接続が確立され、プレビュー パネルにデータが表示されます。クエリの [適用されたステップ] を自由に見て回ってみましょう。基本的に、Suppliers クエリには仕入先の詳細が、SupplierCategories にはその名前が示すように仕入先のカテゴリがあります。これら 2 つのテーブルを結合され、必要な列がある Supplier ディメンションが作成されます。同様に、PO Line Items と PO がマージされ、PO ファクトが作成されます。次は、Supplier と PO のデータを Lakehouse に取り込む必要があります。

6. 前述したように、ここではこのデータを一切ステージングしません。そのため、[クエリ] ペインの **Supplier** クエリを**右クリックし、ステージングを有効にする**を選択してチェック マークを外します。
 
7. 同様に、PO クエリを右クリックします。**ステージングを有効にする**を選択してチェック マークを外します。

**注**: 他の 3 つのクエリのステージングを無効にする必要はありません (これらのクエリのコピー元である Power BI Desktop で [読み込みを有効にする] がオフになっているため)。

## タスク 3: Supplier クエリと PO クエリのデータ送信先を構成する

1. **Supplier** クエリを選択します。

2. 右下隅の**データ同期先**の横にある + を選択します。

3. ダイアログで**レイクハウス**を選択します。
 
4. [データ変換先に接続] ダイアログが開きます。**接続ドロップダウンから Lakehouse (なし)** を選択します。

5. **次へ**を選択します。

6. [宛先ターゲットの選択] ダイアログが開きます。新しいテーブルを作成しているため、**新しいテーブル ラジオ ボタンがオン**になっていることを確認してください。

7. 先ほど作成した Lakehouse にテーブルを作成する必要があります。左パネルで、**Lakehouse -> FAIAD_<ユーザー名>** に移動します。

8. **lh_FAIAD** を選択します。

9. テーブル名は **Supplier** のままにします。

10. **次へ**を選択します。
 
11.	[宛先の設定を選択する] ダイアログが開きます。Dataflow Gen2 が更新されるたびに、完全な読み込みを実行する必要があります。**更新方法が置換**に設定されていることを確認してください。

12. "一部の列名には、一部の列名にサポートされていない文字が含まれています。問題を解決する必要がありますか?" という警告が表示されます。Lakehouse では列名にスペースを使用できません。警告を消すには**修正する**を選択します。

13.	列マッピングを使用して、データフロー列を既存の列にマップできます。この場合、それは新しいテーブルです。そのため、既定値を使用できます。**設定の保存**を選択します。
 
14.	**Power Query のウィンドウ**に戻ります。**右下隅を見ると、データ同期先がレイクハウスに**設定されています。同様に、**PO クエリのデータ同期先を設定します**。完了すると、以下のスクリーンショットに示すように、PO クエリの**データ同期先がレイクハウスに**設定されます。
 
## タスク 4: Snowflake の Dataflow の名前を変更して公開する

1. 画面上部で、**Dataflow 1 の横にある矢印**を選択して名前を変更します。

2. ダイアログで、名前を **df_Supplier_Snowflake** に変更します。

3. **Enter** キーを押して名前の変更を保存します。
 
4. 右下隅の**公開**を選択します。
 
**Data Factory の画面**に戻ります。Dataflow が公開されるまで、しばらくかかる場合があります。

**注**: Dataflow 名が更新されない場合があります。この場合は、以下のステップを実行してください。Dataflow 名が変更されたら、次のタスクに進むことができます。

5.	Dataflow 1 の公開が完了したら、名前を変更しましょう。Dataflow 

1 の横にある **省略記号 (…)** をクリックします。**プロパティ**を選択します。
 
6. [Dataflow のプロパティ] ダイアログが開きます。名前を **df_Supplier_Snowflake** に変更します。

7. **説明**テキスト ボックスに、**Dataflow to ingest Supplier data from Snowflake to Lakehouse** と入力します。

8. **保存**を選択します。
 
**Data Factory の画面**に戻ります。次は、Dataverse からデータを取り込むデータフローを作成しましょう。

## タスク 5: Dataverse のクエリを Dataflow にコピーする

1. トップ メニューから、**新規 -> データフロー (Gen2)** を選択します。
 
**Dataflow のページ**が表示されます。ここまで Dataflow について説明してきました。次は、Power BI Desktop から Dataflow にクエリをコピーしましょう。

2. まだ開いていない場合は、お使いの**ラボ環境のデスクトップの Report** フォルダーにある **FAIAD.pbix** を開きます。

3. リボンから**ホーム -> データの変換**を選択します。Power Query ウィンドウが開きます。前のラボで確認したように、左パネルのクエリはデータ ソースごとに整理されています。
 
4. Power Query ウィンドウが開きます。左パネルで、DataverseData フォルダーにある次のクエリを **Ctrl+Select** を押しながら選択します。

    a. BabyBoomer

    b. GenX

    c. GenY

    d. GenZ

    e. Customer

5. **右クリックしてコピー**を選択します。
 
6. ブラウザーの **Dataflow のページ**に戻ります。

7. **Dataflow のペイン**で、**Ctrl+V** を押します (現時点では右クリックの貼り付けには対応していません)。

## タスク 6: Dataverse への接続を作成する

5 つのクエリが貼り付けられ、左側に [クエリ] パネルが表示されています。Dataverse 用の接続が作成されていないため、接続を構成するよう求める警告メッセージが表示されます。

1. **接続の構成**を選択します。
 
2. [データ ソースへの接続] ダイアログが開きます。**接続ドロップダウンで、新しい接続の作成が選択されている**ことを確認します。

3. **認証の種類は組織アカウント**にします。

4. **接続**を選択します。
 
## タスク 7: Customer クエリのデータ送信先を作成する

接続が確立され、プレビュー パネルにデータが表示されます。クエリの [適用されたステップ] を自由に見て回ってみましょう。Customer のデータは、カテゴリ別 (BabyBoomer、GenX、GenY、GenZ) に入手できます。これら 4 つのクエリをアペンドして、Customer クエリを作成します。次は、Customer のデータを Lakehouse に取り込む必要があります。

1. 前述したように、ここではこのデータを一切ステージングしません。そのため、[クエリ] ペインの Customer クエリを**右クリックし、ステージングを有効にする**を選択してチェック マークを外します。
 
2. **Customer** クエリを選択します。

3. 右下隅の**データ同期先**の横にある + を選択します。

4. ダイアログで**レイクハウス**を選択します。
 
5. [データ変換先に接続] ダイアログが開きます。**接続ドロップダウン**から **Lakehouse (なし)** を選択します。

6. **次へ**を選択します。

7. [宛先ターゲットの選択] ダイアログが開きます。新しいテーブルを作成しているため、**新しいテーブル ラジオ ボタン**がオンになっていることを確認してください。

8. 先ほど作成した Lakehouse にテーブルを作成する必要があります。左パネルで、**Lakehouse -> FAIAD_<ユーザー名>** に移動します。

9. **lh_FAIAD** を選択します。

10. テーブル名は **Customer** のままにします。

11.	**次へ**を選択します。
 
12.	[宛先の設定を選択する] ダイアログが開きます。Dataflow Gen2 が更新されるたびに、完全な読み込みを実行する必要があります。**更新方法が置換**に設定されていることを確認してください。

13.	"一部の列名には、サポートされていない文字が含まれています。問題を解決する必要がありますか?" という警告が表示されます。Lakehouse では列名にスペースを使用できません。警告を消すには**修正する**を選択します。

14.	列マッピングを使用して、データフロー列を既存の列にマップできます。この場合、それは新しいテーブルです。そのため、既定値を使用できます。**設定の保存**を選択します。
 
## タスク 8: Dataverse の Dataflow を公開して名前を変更する

1. **Power Query のウ**ィンドウに戻ります。**右下隅**を見ると、**データ同期先がレイクハウスに**設定されています。

2. 右下隅の**公開**を選択します。
 
**注: Data Factory の画面**に戻ります。Dataflow が公開されるまで、しばらくかかる場合があります。

3. ここで操作しているデータフローは Dataflow 1 です。先に進む前に名前を変更しましょう。Dataflow 1 の横にある **省略記号 (…)** をクリックします。**プロパティ**を選択します。
 
4. [Dataflow のプロパティ] ダイアログが開きます。**名前を df_Customer_Dataverse** に変更します。

5. **説明**テキスト ボックスに、**Dataflow to ingest Customer data from Dataverse to Lakehouse** と入力します。

6. **保存**を選択します。
 
**Data Factory の画面**に戻ります。次は、SharePoint からデータを取り込むデータフローを作成しましょう。

## タスク 9: SharePoint のクエリを Dataflow にコピーする

1. トップ メニューから、**新規 -> データフロー (Gen2)** を選択します。
 
**Dataflow のページ**が表示されます。ここまで Dataflow について説明してきました。次は、Power BI Desktop から Dataflow にクエリをコピーしましょう。

7. まだ開いていない場合は、お使いのラボ環境の**デスクトップの Report** フォルダーにある **FAIAD.pbix** を開きます。

8. リボンから**ホーム -> データの変換**を選択します。Power Query ウィンドウが開きます。前のラボで確認したように、左パネルのクエリはデータ ソースごとに整理されています。

9. Power Query ウィンドウが開きます。左パネルの SharepointData フォルダーにある **People** クエリを**選択**します。

10.	**右クリック**して**コピー**を選択します。
 
11. ブラウザーの **Dataflow の画面**に戻ります。

12.	**Dataflow のペイン**で、**Ctrl+V** を押します (現時点では右クリックの貼り付けには対応していません)。

クエリが貼り付けられ、左パネルで使用できます。SharePoint への接続が作成されていないため、接続を構成するよう求める警告メッセージが表示されます。

## タスク 10: SharePoint への接続を作成する

1. **接続の構成**を選択します。
 
2. [データ ソースへの接続] ダイアログが開きます。**接続**ドロップダウン
で、**新しい接続の作成**が選択されていることを確認します。

3. **認証の種類は組織アカウント**にします。

4. **接続**を選択します。
 

## タスク 11: People クエリのデータ送信先を構成する

接続が確立され、プレビュー パネルにデータが表示されます。クエリの [適用されたステップ] を自由に見て回ってみましょう。次は、People のデータを Lakehouse に取り込む必要があります。

1. 前述したように、ここではこのデータを一切ステージングしません。そのため、[クエリ] ペインの **People クエリを右クリックし、ステージングを有効にする**を選択してチェック マークを外します。
 
2. **People** クエリを選択します。

3. 右下隅の**データ同期先**の横にある + を選択します。

4. ダイアログで**レイクハウス**を選択します。
 
5. [データ変換先に接続] ダイアログが開きます。**接続ドロップダウン**から **Lakehouse (なし)** を選択します。

6. **次へ**を選択します。
 

7. [宛先ターゲットの選択] ダイアログが開きます。新しいテーブルを作成しているため、**新しいテーブル ラジオ ボタン**がオンになっていることを確認してください。

8. 先ほど作成した Lakehouse にテーブルを作成する必要があります。左パネルで、**Lakehouse -> FAIAD_<ユーザー名>** に移動します。

9. **lh_FAIAD** を選択します。

10. テーブル名は **People** のままにします。

11. **次へ**を選択します。
 
12.	[宛先の設定を選択する] ダイアログが開きます。Dataflow Gen2 が更新されるたびに、完全な読み込みを実行する必要があります。**更新方法が置換**に設定されていることを確認してください。

13.	"一部の列名には、サポートされていない文字が含まれています。問題を解決する必要がありますか?" という警告が表示されます。Lakehouse では列名にスペースを使用できません。警告を消すには**修正する**を選択します。

14.	列マッピングを使用して、データフロー列を既存の列にマップできます。この場合、それは新しいテーブルです。そのため、既定値を使用できます。**設定の保存**を選択します。
 
## タスク 12: SharePoint の Dataflow を公開して名前を変更する

1. **Power Query のウィンドウ**に戻ります。**右下隅**を見ると、データ同期先が**レイクハウス**に設定されています。

2. 右下隅の**公開**を選択します。
 
**注: Data Factory の画面**に戻ります。Dataflow が公開されるまで、しばらくかかる場合があります。

3. ここで操作しているデータフローは Dataflow 1 です。先に進む前に名前を変更しましょう。Dataflow 1 の横にある**省略記号 (…)** をクリックします。**プロパティ**を選択します。

 
4. [Dataflow のプロパティ] ダイアログが開きます。**名前を df_People_SharePoint** に変更し
ます。

5. **説明**テキスト ボックスに、**Dataflow to ingest People data from SharePoint to Lakehouse** と入力します。

6. **保存**を選択します。
 
**Data Factory の画面**に戻ります。これで、すべてのデータが Lakehouse に取り込まれました。次のラボでは、Dataflow の更新スケジュールを設定します。

# リファレンス

Fabric Analyst in a Day (FAIAD) では、Microsoft Fabric で使用できる主要な機能の一部をご紹介します。サービスのメニューにあるヘルプ (?) セクションには、いくつかの優れたリソースへのリンクがあります。

 
Microsoft Fabric の次のステップに役立つリソースをいくつか以下に紹介します。

- ブログ記事で [Microsoft Fabric の GA に関するお知らせ](https://aka.ms/Fabric-Hero-Blog-Ignite23)の全文を確認する

- [ガイド付きツアー](https://aka.ms/Fabric-GuidedTour)を通じて Fabric を探索する

- [Microsoft Fabric の無料試用版](https://aka.ms/try-fabric)にサインアップする

- [Microsoft Fabric の Web サイト](https://aka.ms/microsoft-fabric)にアクセスする

- [Fabric の学習モジュール](https://aka.ms/learn-fabric)で新しいスキルを学ぶ

- [Fabric の技術ドキュメント](https://aka.ms/fabric-docs)を参照する

- [Fabric 入門編の無料の e-book ](https://aka.ms/fabric-get-started-ebook)を読む

- [Fabric コミュニティ](https://aka.ms/fabric-community)に参加し、質問の投稿やフィードバックの共有を行い、他のユーザーから学びを得る


より詳しい Fabric エクスペリエンスのお知らせに関するブログを参照してください。


- [Fabric の Data Factory エクスペリエンスに関するブログ](https://aka.ms/Fabric-Data-Factory-Blog) 

- [Fabric の Synapse Data Engineering エクスペリエンスに関するブログ](https://aka.ms/Fabric-DE-Blog) 

- [Fabric の Synapse Data Science エクスペリエンスに関するブログ](https://aka.ms/Fabric-DS-Blog) 

- [Fabric の Synapse Data Warehousing エクスペリエンスに関するブログ](https://aka.ms/Fabric-DW-Blog) 

- [Fabric の Synapse Real-Time Analytics エクスペリエンスに関するブログ](https://aka.ms/Fabric-RTA-Blog)

- [Power BI のお知らせに関するブログ](https://aka.ms/Fabric-PBI-Blog)

- [Fabric の Data Activator エクスペリエンスに関するブログ](https://aka.ms/Fabric-DA-Blog) 

- [Fabric の管理とガバナンスに関するブログ](https://aka.ms/Fabric-Admin-Gov-Blog)

- [Fabric の OneLake に関するブログ](https://aka.ms/Fabric-OneLake-Blog)

- [Dataverse と Microsoft Fabric の統合に関するブログ](https://aka.ms/Dataverse-Fabric-Blog)


© 2023 Microsoft Corporation. All rights reserved.

このデモ/ラボを使用すると、次の条件に同意したことになります。

このデモ/ラボで説明するテクノロジまたは機能は、ユーザーのフィードバックを取得し、学習エクスペリエンスを提供するために、Microsoft Corporation によって提供されます。ユーザーは、このようなテクノロジおよび機能を評価し、Microsoft にフィードバックを提供するためにのみデモ/ラボを使用できます。それ以外の目的には使用できません。このデモ/ラボまたはその一部を、変更、コピー、配布、送信、表示、実行、再現、発行、ライセンス、著作物の作成、転送、または販売することはできません。

複製または再頒布のために他のサーバーまたは場所にデモ/ラボ (またはその一部) をコピーまたは複製することは明示的に禁止されています。

このデモ/ラボは、前に説明した目的のために複雑なセットアップまたはインストールを必要としないシミュレーション環境で潜在的な新機能や概念などの特定のソフトウェア テクノロジ/製品の機能を提供します。このデモ/ラボで表されるテクノロジ/概念は、フル機能を表していない可能性があり、最終バージョンと動作が異なることがあります。また、そのような機能や概念の最終版がリリースされない場合があります。物理環境でこのような機能を使用するエクスペリエンスが異なる場合もあります。

**フィードバック**。このデモ/ラボで説明されているテクノロジ、機能、概念に関するフィードバックを Microsoft に提供する場合、ユーザーは任意の方法および目的でユーザーのフィードバックを使用、共有、および商品化する権利を無償で Microsoft に提供するものとします。また、ユーザーは、フィードバックを含む Microsoft のソフトウェアまたはサービスの特定部分を使用したり特定部分とインターフェイスを持ったりする製品、テクノロジ、サービスに必要な特許権を無償でサード パーティに付与します。ユーザーは、フィードバックを含めるために Microsoft がサード パーティにソフトウェアまたはドキュメントをライセンスする必要があるライセンスの対象となるフィードバックを提供しません。これらの権限は、本契約の後も存続します。

Microsoft Corporation は、明示、黙示、または法律上にかかわらず、商品性のすべての保証および条件、特定の目的、タイトル、非侵害に対する適合性など、デモ/ラボに関するすべての保証および条件を拒否します。Microsoft は、デモ/ラボから派生する結果、出力の正確さ、任意の目的に対するデモ/ラボに含まれる情報の適合性に関して、いかなる保証または表明もしません。

**免責事項**

このデモ/ラボには、Microsoft Power BI の新機能と機能強化の一部のみが含まれています。一部の機能は、製品の将来のリリースで変更される可能性があります。このデモ/ラボでは、新機能のすべてではなく一部について学習します。
