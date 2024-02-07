
 
# 目次

- 概要	

- Dataflow Gen2	

    タスク 1: Sales データフローのスケジュールされた更新を構成する	

    タスク 2: Supplier と Customer データフローのスケジュールされた更新
    を構成する	

- データ パイプライン	

    タスク 3: データ パイプラインを作成する	

    タスク 4: シンプルなデータ パイプラインを構築する

    タスク 5: 新しいデータ パイプラインを作成する	

    タスク 6: Until アクティビティを作成する	

    タスク 7: 変数を作成する	

    タスク 8: Until アクティビティを構成する	 

    タスク 9: データフロー アクティビティを構成する	

    タスク 10: 最初の変数の設定アクティビティを構成する	

    タスク 11: 2 番目の変数の設定アクティビティを構成する	

    タスク 12: 3 番目の変数の設定アクティビティを構成する	

    タスク 13: Wait アクティビティを構成する	

    タスク 14: データ パイプラインのスケジュールされた更新を構成する

- リファレンス	

# 概要

さまざまなデータ ソースから Lakehouse にデータを取り込みました。このラボでは、データ ソースの更新スケジュールを設定します。要件を要約しておきましょう。

- **販売データ**: ADLS で、毎日正午 (午後 0 時) に更新されます。

- **仕入先データ**: Snowflake で、毎日真夜中 (午前 0 時) に更新されます。

- **顧客データ**: Dataverse で、常に最新の状態です。1 日に 4 回更新する必要があります (真夜中 (午前 0 時)、午前 6 時、正午 (午後 0 時)、午後 6 時)。

- **従業員データ**: SharePoint で、毎日午前 9 時に更新されます。ただし、15 分から 30 分の遅延が発生する場合があることを確認しています。これに対応できるように更新スケジュールを作成する必要があります。

このラボを終了すると、次のことが学べます。

- Dataflow Gen2 のスケジュールされた更新を構成する方法

- データ パイプラインを作成する方法

- データ パイプラインのスケジュールされた更新を構成する方法


# Dataflow Gen2

## タスク 1: Sales データフローのスケジュールされた更新を構成する

まず、Sales データフローのスケジュールされた更新を構成しましょう。

1. ラボ 2 のタスク 8 で作成した Fabric ワークスペース **FAIAD_<ユーザー名>** に戻りましょう。

2. 作成したすべてのアーティファクトがここに一覧表示されています。画面右側にある**検索ボックス**に、**df** と入力します。これにより、アーティファクトがデータフローに絞り込まれます。
 
3. **df_Sales_ADLS** 行の上にマウスを置きます。見慣れた**更新アイコン**と**スケジュールされている更新アイコン**が表示されます。**省略記号 (…)** を選択します。

4. データフローを削除、編集、エクスポートするオプションがあることがわかります。[プロパティ] を使用して、データフローの名前と説明を更新することができます。[更新履歴] については後ほど見ていきます。設定を選択します。
 
**注**: 設定ページが開きます。左パネルには、すべてのデータフローが一覧表示されます。

5. 中央のペインで、**更新履歴**のリンクを選択します。
 
6. [更新履歴] ダイアログが開きます。少なくとも 1 つの更新が表示されます。これは、データフローを公開するときに発生した更新です。**開始時刻**のリンクを選択します。

**注**: 開始時間はお客様によって異なります。
 
[詳細] 画面が開きます。開始時刻、終了時刻、期間など、更新の詳細情報を確認できます。また、更新されたテーブルとアクティビティも一覧表示されます。失敗が発生した場合は、テーブルやアクティビティの名前をクリックしてさらに調査できます。
 
7. 右上隅の **X** をクリックして、他の場所に移動しましょう。**データフローの設定ページ**に戻ります。

8. [ゲートウェイ接続] の下にある**データソースの資格情報**を展開します。データフローで使用される接続の一覧が表示されます。この場合は、Lakehouse と ADLS です。

    a. **Lakehouse**: データフローからデータを取り込むための接続です。

    b. **ADLS**: ADLS ソース データへの接続です。
 
9. **更新**を展開します。

10. **更新スケジュールを構成する**スライダーを**オン**に設定します。

11. **更新頻度ドロップダウン**を**毎日**に設定します。[毎週] に設定するオプションもあることに注意してください。

12. **タイム ゾーン**を優先するタイム ゾーンに設定します。

**注**: これはラボ環境であるため、タイムゾーンは優先するタイムゾーンに設定できます。実際のシナリオでは、データ ソースの場所に基づいてタイム ゾーンを設定します。

13. **別の時刻を追加**のリンクをクリックします。**時刻**オプションが表示されます。

14. **時刻を正午**に設定します。1 時間または 30 分単位で更新を設定できることがわかります。

15. **適用**を選択してこの設定を保存します。

**注**: [別の時刻を追加] リンクをクリックすると、複数の更新時刻を追加できます。

データフロー所有者や他の連絡先に失敗の通知を送信することもできます。
 

## タスク 2: Supplier と Customer データフローのスケジュールされた更新を構成する

1. 左パネルで、**df_Supplier_Snowflake** を選択します。

2. **毎日真夜中 (午前 0 時)** に更新するように更新スケジュールを構成します。

3. **適用**を選択してこの設定を保存します。
 
4. 左パネルで、**df_Customer_Dataverse** を選択します。

5. 1 日 4 回 で更新スケジュールを構成します **(真夜中 (午前 0 時)、午前 6 時、正午 (午後 0 時)、午後 6 時)**。

6. **適用**を選択してこの設定を保存します。
 

前述したように、SharePoint の従業員ファイルが時間どおりに提供されないシナリオを処理するカスタム ロジックを構築する必要があります。データ パイプラインを使ってこれを解決しましょう。


# データ パイプライン

## タスク 3: データ パイプラインを作成する

1. ブラウザー ウィンドウの**左下**にある **Power BI** を選択します。

2. [Microsoft Fabric] ダイアログが開きます。**Data Factory** を選択します。Data Factory のホーム ページに移動します。
 
3. 上部パネルから**データ パイプライン**を選択して、新しいパイプラインを作成します。

4. [新しいパイプライン] ダイアログが開きます。パイプラインに **pl_Refresh_People_SharePoint** という名前を付けます

5. **作成**を選択します。
 
**データ パイプライン ページ**に移動します。Azure Data Factory を使用したことがある場合は、この画面に見覚えがあるはずです。レイアウトの概要を簡単に確認してみましょう。

**ホーム**画面が表示されています。上部メニューを見ると、よく使うアクティビティ (パイプラインの検証、実行、実行履歴の表示) を追加するオプションがあります。また、中央ペインには、パイプラインの構築を開始するためのクイック オプションがあります。
 
6. 上部メニューから、**アクティビティ**を選択します。これで、メニューによく使うアクティビティのリストが表示されます。

7. メニューの右側にある**省略記号 (…)** を選択すると、他の利用可能なアクティビティがすべて表示されます。これらのアクティビティのいくつかをラボで使用します。
 
8. 上部メニューの**実行**をクリックします。パイプライン実行を実行およびスケジュールするためのオプションが表示されます。[実行履歴を表示する] を使って実行履歴を確認するオプションもあります。

9. 上部メニューから、**表示**を選択します。ここには、コードを JSON 形式で表示するオプションがあります。アクティビティを書式設定するオプションもあります。

**注**: ラボの最後に JSON の予備知識をお持ちの場合は、自由に [JSON コードを表示する] を選択してください。ここでわかるように、デザイン ビューを使って実行しているすべてのオーケストレーションを、JSON で記述することもできます。
 

## タスク 4: シンプルなデータ パイプラインを構築する

パイプラインの構築を始めましょう。データフローを更新するアクティビティが必要です。使えるアクティビティを探してみましょう。

1. 上部メニューから、**アクティビティ -> データフロー**を選択します。データフロー アクティビティが中央デザイン ペインに追加されます。下部ペインにデータフロー アクティビティの構成オプションが表示されることに注目してください。

2. このアクティビティを構成して、df_People_SharePoint アクティビティと接続します。
**下部ペイン**から、設定を選択します。

3. **ワークスペース**がご自身の Fabric ワークスペース **FAIAD_<ユーザー名>** に設定されていることを確認します。

4. **データフロー ドロップダウン**から、**df_People_SharePoint** を選択します。このデータフロー アクティビティを実行すると、**df_People_SharePoint** が更新されます。簡単ですね。😊

**注**: 現在、通知オプションはグレー表示になっています。この機能は間もなく有効になる予定です。このアクティビティの成功と失敗に関する通知を設定できるようになります。

このシナリオでは、従業員データがスケジュールどおりに更新されません。遅延が発生することがあります。これに対応できるかどうか見てみましょう。
 
5. **下部ペイン**から、**全般**を選択します。アクティビティの名前と説明を指定しましょう。

6. **名前**フィールドに **dfactivity_People_SharePoint** と入力します

7. **説明**フィールドに **Dataflow activity to refresh df_People_Sharepoint dataflow** と入力します。

8. アクティビティを非アクティブ化するオプションがあることに注意してください。この機能は、テスト中やデバッグ中に役立ちます。**アクティブ化**のままにしておきます。

9. **タイムアウト**を設定するオプションがあります。**既定値**はデータフローを更新するのに十分な時間なので、そのままにしておきます。

**注**: データをスケジュールどおりに利用できない場合は、10 分ごとに 3 回再実行するようにアクティビティを設定しましょう。3 回目の試行でも失敗した場合は、失敗を報告します。

10. **再試行**を **3** に設定します

11. **詳細**セクションを展開します。

12. **再試行のサイクル間隔 (秒)** を **600** に設定します。

13. メニューから**ホーム -> 保存**アイコンを選択して、パイプラインを保存します。
 
 
(以前のデータフローで行ったように) スケジュールされた更新でデータフローを設定する場合と比較して、データ パイプラインを使用する利点に注目してください。

- パイプラインには、更新が失敗する前に複数回再試行するオプションが用意されています。

- パイプラインでは数秒以内に更新することができますが、データフローのスケジュールされた更新は 30 分ごとです。

## タスク 5: 新しいデータ パイプラインを作成する

シナリオをもう少し複雑にしてみましょう。午前 9 時にデータを取得できない場合でも、通常は 5 分以内に取得できることがわかりました。その時間範囲を超えた場合は、ファイルを取得できるまでに 15 分かかります。5 分と 15 分で再試行をスケジュールする必要があります。新しいデータ パイプラインを作成して、これをどのように実現できるかを見てみましょう。

1. 左パネルで **FAIAD_<ユーザー名>** をクリックすると、Data Factory ホーム ページに移動します。

2. 上部メニューから**新規**をクリックし、**ドロップダウン**から**データ パイプライン**をクリックします。

3. [新しいパイプライン] ダイアログが開きます。パイプラインに **pl_Refresh_People_SharePoint_Option2** という**名前**を付けます

4. **作成**を選択します。
 

## タスク 6: Until アクティビティを作成する

1. データ パイプライン画面に戻ります。メニューから**アクティビティ**を選択します。

2. 右側の**省略記号 (…)** をクリックします。

3. アクティビティ一覧から **Until** をクリックします。

**Until**: ある条件が満たされるまで反復処理するために使うアクティビティです。

今回のシナリオでは、成功するか 3 回試行するまで繰り返しデータフローを更新します。

## タスク 7: 変数を作成する

1. 反復処理と状態の設定に使う変数を作成する必要があります。パイプライン デザイン ペインの**空白領域**を選択します。

2. 下部ペインのメニューが変化することに注目してください。**変数**を選択します。

3. **新規**を選択して、新しい変数を追加します。

4. 行が表示されます。**名前テキスト ボックス**に **varCounter** と入力します。この変数を使って、反復処理を 3 回行います。

5. **種類ドロップダウン**から **Integer を**選択します。

6. **既定値**に 0 を入力します。

**注**: 変数名の先頭に var を付けて見つけやすくしています。これは良い習慣です。
 
7. **新規**を選択して、別の新しい変数を追加します。

8. 行が表示されます。**名前テキスト ボックス**に **varTempCounter** と入力します。この変数を使って varCounter 変数を増分します。

9. **種類ドロップダウン**から Integer を選択します。

10. **既定値**に 0 を入力します。

11. 同様の手順に従って、さらに 3 つの変数を追加します。

    a. **varIsSuccess**: 種類は String、既定値は **No** に設定します。この変数を使って、データフローの更新が成功したかどうかを表します。

    b. **varSuccess**: 種類は String、既定値は **Yes** に設定します。データフローの更新が成功した場合、この変数を使って varIsSuccess の値を設定します。

    c. **varWaitTime**: 種類は Integer、既定値は **60** に設定します。この変数を使って、データフローが失敗した場合の待機時間を設定します。(5 分 (300 秒) または 15 分 (900 秒) のいずれかです。)

## タスク 8: Until アクティビティを構成する

1. **Until** アクティビティを選択します。

2. **下部ペイン**から、**全般**を選択します。

3. **名前**に **Iterator** と入力します。

4. **説明**に **Iterator to refresh dataflow. It will retry up to 3 times** と入力します。
 
5. 下部ペインから、設定を選択します。

6. **式テキスト ボックス**を選択します。このテキスト ボックスに、true または false と評価される式を入力する必要があります。Until アクティビティは、この式が false と評価されている間反復処理を行います。式が true と評価されると、Until アクティビティは反復を停止します。

7. テキスト ボックスの下に表示される**動的なコンテンツの追加**リンクを選択します。
 
次の条件が満たされるまで実行する式を記述する必要があります: **varCounter の値が 3** であるか、**varIsSuccess の値が Yes** である。(varCounter と varIsSuccess は先ほど作成した変数です。)

8. **パイプライン式ビルダー** ダイアログが開きます。ダイアログの下半分にはメニューが表示されます。

    a. **パラメーター**: パイプラインの任意の式で使用できるデータ ファクトリ全体の定数です。

    b. **システム変数**: いずれかのサービス内でエンティティを定義するときに、これらの変数を式で使用できます。例: パイプライン ID、パイプライン名、トリガー名など。

    c. **関数**: 式内で関数を呼び出すことができます。各関数は、コレクション、変換、日付、論理、数学、文字列の関数に分類されます。たとえば、concat は文字列関数、add は数学関数です。

    d. **変数**: パイプライン変数は、パイプライン実行中に設定および変更できる値です。パイプライン レベルで定義され、パイプライン実行中に変更できないパイプライン パラメーターとは異なり、パイプライン変数は、変数の設定アクティビティを使って、パイプライン内で設定および変更できます。この後すぐに変数の設定アクティビティを使用します。
 
9. 下部メニューの**関数**をクリックします。

10. **Logical 関数**セクションから、**or 関数**を選択します。動的な式テキスト ボックスに **@or()** が追加されるのがわかります。or 関数は 2 つのパラメーターを受け取ります。ここでは最初のパラメーターに取り組みます。
 
11. @or 関数の**かっこの間に**カーソルを置きます。

12. **Logical 関数**セクションから、**equals** 関数を選択します。それが動的な式テキスト ボックスに追加されます。

**注**: 関数は **@or(equals())** のようになるはずです。equals 関数も 3 つのパラメーターを受け取ります。変数 varCounter が 3 に等しいかどうかを確認します。
 
13. 次に、**@equals** 関数の**かっこの間に**カーソルを置いて、パラメーターを追加します。

14. 下部メニューから、**変数**を選択します。

15. 最初のパラメーターとなる **varCounter** 変数を選択します。

16. equals 関数の 2 番目のパラメーターとして 3 と入力します。以下のスクリーンショットのように、式は **@or(equals(variables('varCounter'),3))** になります
 
17. or 関数に 2 番目のパラメーターを追加する必要があります。最後の 2 つのかっこの間に**コンマを追加します**。今回は関数名を入力してみましょう。**equ** と入力し始めると、使用できる関数のドロップダウンが表示されます (IntelliSense と呼ばれます)。**equals** 関数を選択します。
 
18. equals 関数の最初のパラメーターは変数です。**コンマの前にカーソルを**置きます。

19. **variables(** と入力を開始します

20. IntelliSense の助けを借りて **variables('varIsSuccess')** を選択します

21. コンマの後に 2 番目のパラメーターを入力しましょう。**variables(** と入力を開始します

22. IntelliSense の助けを借りて **variables('varSuccess')** を選択します。ここでは、varIsSuccess の値と varSuccess の値を比較しています。(varSuccess の既定値は Yes です。)
 
23. 式は次のようになるはずです。
**@or(equals(variables('varCounter'),3),equals(variables('varIsSuccess'), variables('varSuccess')))**

24. **OK** を選択します。
 

## タスク 9: データフロー アクティビティを構成する

1. デザイン画面に戻ります。**Until アクティビティ**を選択した状態で、**下部ペイン**から**アクティビティ**を選択します。ここでは、実行する必要があるアクティビティを追加します。

2. 最初の行の**編集アイコン**を選択します。空白の反復処理デザイン画面に戻ります。
 
3. 上部メニューから、**アクティビティ -> データフロー**を選択します。データフロー アクティビティがデザイン ペインに追加されます。

4. **データフロー アクティビティを選択した**状態で、下部ペインで**全般**を選択します。アクティビティの名前と説明を指定しましょう。

5. **名前**フィールドに **dfactivity_People_SharePoint** と入力します

6. **説明**フィールドに **Dataflow activity to refresh df_People_Sharepoint dataflow** と入力します。
 
7. 下部ペインから設定を選択します。

8. **ワークスペース**がご自身のワークスペース **FAIAD_<ユーザー名>** に設定されていることを確認します。
 
9. **データフロー ドロップダウン**から、**df_People_SharePoint** を選択します。このデータフロー アクティビティを実行すると、**df_People_SharePoint** が更新されます。
 
## タスク 10: 最初の変数の設定アクティビティを構成する

ラボで前に行ったように、データフロー アクティビティを構成しました。次に、新しいロジックを追加します。データフローの更新が成功した場合は、Until の反復処理から抜ける必要があります。反復処理から抜ける条件の 1 つは、varIsSuccess 変数の値を Yes に設定することだったことを思い出してください。

1. 上部メニューから、**アクティビティ -> 変数の設定**を選択します。変数の設定アクティビティがデザイン キャンバスに追加されます。

2. **変数の設定アクティビティ**を選択した状態で、下部ペインで**全般**を選択します。アクティビティの名前と説明を指定しましょう。

3. **名前** フィールドに、**set_varIsSuccess** と入力します。

4. **説明**フィールドに、**Set variable varIsSuccess to Yes** と入力します。

    **注**: データフロー アクティビティの上にカーソルを合わせてください。アクティビティ ボックスの右側に 4 つのアイコンがあります。これらを使って、このアクティビティの結果に基づいて次のアクティビティに接続できます。

    a. アクティビティをスキップするときは**灰色の曲がった矢印**アイコンを使います。

    b. アクティビティの成功時は**緑色のチェック マーク** アイコンを使います。

    c. アクティビティの失敗時は**赤い x マーク** アイコンを使います。

    d. アクティビティの完了時は**青い真っすぐな矢印**アイコンを使います。

5. dfactivity_People_SharePoint データフロー アクティビティから**緑色のチェック マーク**をクリックしてドラッグし、新しい **set_varIsSuccess 変数の設定アクティビティ**に接続します。つまり、データフローの更新が成功したら、変数の設定アクティビティを実行します。
 
6. **変数の設定アクティビティ**を選択した状態で、下部メニューから**設定**をクリックします。

7. 下部ペインで、**変数の種類**が**パイプライン変数**であることを確認します。

8. **名前**フィールドで、**varIsSucces** を選択します。この変数の値を設定します。

9. **値**フィールドで、**テキスト ボックス**を選択します。**動的なコンテンツの追加**リンクを選択します。
 
10. パイプライン式ビルダー ダイアログが開きます。**式、関数、システム変数の任意の組み合わせを使用して以下に動的コンテンツを追加しますのテキスト領域**を選択します。

11. 下部メニューから、**変数 -> varSuccess** を選択します。以下に動的コンテンツを追加しますのテキスト領域に @variables('varSuccess') と入力されることがわかります。変数を作成したときに、varSuccess 変数の値をあらかじめ Yes に設定したことを思い出してください。そのため、Yes の値が varIsSuccess 変数に代入されます。

12. **OK** を選択します。**反復処理デザイン ペイン**に戻ります。
 
次に、データフロー アクティビティが失敗した場合にカウンターを設定する必要があります。データ パイプラインでは、変数を自己参照することはできません。つまり、カウンター変数 varCounter の値に 1 を加えて増分することはできません (varCounter = varCounter + 1)。そこで、varTempCounter 変数を利用します。


## タスク 11: 2 番目の変数の設定アクティビティを構成する

1. 上部メニューから、**アクティビティ -> 変数の設定**を選択します。変数の設定アクティビティがデザイン キャンバスに追加されます。

2. **変数の設定アクティビティ**を選択した状態で、下部ペインで**全般**を選択します。アクティビティの名前と説明を指定しましょう。

3. **名前**フィールドに、**set_varTempCounter** と入力します

4. **説明**フィールドに、**Increment variable varTempCounter** と入力します。

5. データフロー アクティビティの**赤い x マーク**をクリックして、新しい変数の設定アクティビティに接続します。つまり、データフローの更新が失敗したら、この変数の設定アクティビティを実行します。
 
6. **変数の設定アクティビティ**を選択した状態で、下部メニューから設定を選択します。

7. 下部ペインで、**変数の種類がパイプライン変数**であることを確認します。

8. **名前**フィールドで、**varTempCounter** を選択します。この変数の値を設定します。

9. **値**フィールドで、**テキスト ボックス**を選択します。**動的なコンテンツの追加**リンクを選択します。

10. パイプライン式ビルダー ダイアログが開きます。**@add(variables('varCounter'),1)** と入力します

**注**: この式を入力しても、メニューを使って関数を選択しても、またはコピーして貼り付けても構いません。

**注**: この関数は、変数 varTempCounter の値を、変数 varCounter の値に 1 を加えたものに設定します (varTempCounter = varCounter + 1)。
 

次に、varCounter 変数の値を varTempCounter の値に設定する必要があります。


## タスク 12: 3 番目の変数の設定アクティビティを構成する

1. 上部メニューから、**アクティビティ -> 変数の設定**を選択します。変数の設定アクティビティがデザイン キャンバスに追加されます。

2. **変数の設定アクティビティ**を選択した状態で、下部ペインで**全般**を選択します。アクティビティの名前と説明を指定しましょう。

3. **名前**フィールドに、**set_varCounter** と入力します。

4. **説明**フィールドに、**Increment variable varCounter** と入力します。

5. set_varTempCounter 変数の設定アクティビティから**緑色のチェック マーク**をクリックしてドラッグし、新しい **set_varCounter 変数の設定アクティビティ**に接続します。
 
6. **set_varCounter 変数の設定アクティビティ**を選択した状態で、下部メニューから設定をクリックします。

7. 下部ペインで、**変数の種類がパイプライン変数**であることを確認します。

8. **名前**フィールドで、**varCounter** を選択します。この変数の値を設定します。

9. **値**フィールドで、**テキスト ボックス**を選択します。**動的なコンテンツの追加リ**ンクを選択します。

10. パイプライン式ビルダー ダイアログが開きます。**@variables('varTempCounter')** と入力します。この式を入力しても、メニューを使って関数を選択しても、またはコピーして貼り付けても構いません。

**注**: この関数は、変数 varCounter の値を、変数 varTempCounter の値に設定します (varCounter = varTempCounter)。各反復の終わりには、varCounter と varTempCounter の両方が同じ値になります。
 

## タスク 13: Wait アクティビティを構成する

次に、データフローの更新が初めて失敗した場合は、再試行する前に 5 分 (300 秒) 待機する必要があります。データフローの更新が 2 度目に失敗した場合は、15 分 (900 秒) 待機してから再試行する必要があります。Wait アクティビティと変数 varWaitTime を使って待機時間を設定します。

1. 上部メニューから、**アクティビティ -> 省略記号 (…) -> Wait** を選択します。Wait アクティビティがデザイン キャンバスに追加されます。

2. **Wait アクティビティ**を選択した状態で、下部ペインで**全般**を選択します。アクティビティの名前と説明を指定しましょう。

3. **名前**フィールドに、**wait_onFailure** と入力します。

4. **説明**フィールドに、**Wait for 300 seconds on 2nd try and 900 seconds on 3rd try** と入力します。

5. set_varCounter 変数の設定アクティビティから**緑色のチェック マーク**をクリックしてドラッグし、新しい **wait_onFailure Wait アクティビティ**に接続します。
 
6. **Wait アクティビティ**を選択した状態で、下部メニューから設定をクリックします。

7. **待機時間 (秒)** フィールドで、**テキスト ボックス**を選択し、**動的なコンテンツの追加リ**ンクを選択します。

8. パイプライン式ビルダー ダイアログが開きます。以下を入力します

```
@if(
    greater(variables('varCounter'), 1),
    if(equals(variables('varCounter'), 2),
        mul(variables('varWaitTime'),15 ), 
        mul(variables('varWaitTime'), 0)
    ),
    mul(variables('varWaitTime'),5 )
)
```

この式を入力しても、メニューを使って関数を選択しても、またはコピーして貼り付けても構いません。

 
ここでは 2 つの新しい関数を使用しています。

- **greater**: 2 つの数値をパラメーターとして受け取り、どちらが大きいかを比較します。

- **mul**: これは乗算関数であり、乗算する 2 つのパラメーターを受け取ります。

この式は入れ子になった if ステートメントです。varCounter 変数の値が 1 より大きいかどうかをチェックします。true の場合は、varCounter 変数の値が 2 であるかどうかをチェックします。true の場合は、待機時間を varWaitTime の 15 倍に設定します。varWaitTime の既定値を 60 に設定したことを思い出してください。これは 60*15 = 900 秒になります。varCounter 変数の値が 2 でない場合 (2 より大きい場合、つまりデータフローの更新が 3 回失敗したことを意味し、反復処理は終了です。もう待機する必要はありません)、待機時間を varWaitTime * 0 に設定します。つまり、0 です。varCounter 変数の値が 1 の場合、varWaitTime に 5 を掛けます。これは 60*5 = 300 秒になります。

9. **OK** を選択します。

**チェックポイント**: Until 反復処理は下のスクリーンショットのようになっているはずです。
 
10. デザイン キャンバスの左上で **pl_Refresh_People_Sharepoint_Option2** を選択して、Until 反復処理から移動します。
 
11. これで、データ パイプラインの作成が完了しました。上部メニューから**ホーム -> 保存アイコン**を選択して、データ パイプラインを保存します。
 

## タスク 14: データ パイプラインのスケジュールされた更新を構成する

1. データ パイプラインをテストするには、**ホーム -> 実行**を選択します。

**注**: データ パイプラインの更新が完了するまでに数分かかる場合があります。これはトレーニング環境であるため、SharePoint のファイルは常に利用可能です。したがって、データ パイプラインが失敗することはありません。

2. スケジュールに従って実行するようにデータ パイプラインを設定できます。上部メニューから、**ホーム -> スケジュール**を選択します。[スケジュール] ダイアログが開きます。

3. **スケジュールされた実行**のラジオ ボタンを**オン**に設定します。

4. **繰り返しドロップダウン**を**毎日**に設定します。

5. **時刻を午前 9 時**に設定します。

6. **開始日時を今日**に設定します。

7. **終了日時を将来の日付**に設定します。

8. **タイム ゾーン**を設定します。

**注**: これはラボ環境であるため、タイムゾーンは優先するタイムゾーンに設定できます。
実際のシナリオでは、データ ソースの場所に基づいてタイム ゾーンを設定します。

9. **適用**を選択します。
10. ダイアログの右上の **X** マークを選択して、閉じます。
 
11. 左パネルでご自身の Fabric ワークスペース **FAIAD_<ユーザー名>** を選択し、**Data Factory ホーム**画面に移動します。

**注**: [スケジュール] 画面には、(データフローのスケジュールのように) 成功または失敗を通知するオプションはありません。通知は、データ パイプラインにアクティビティを追加することで実行できます。これはラボ環境であるため、このラボでは実行しません。

さまざまなデータ ソースの更新をスケジュールしました。次のラボでは、リレーションシップとメジャーを作成し、その他のモデリング アクティビティを実行します。


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
