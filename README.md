# 【初心者向けチュートリアル】そのワイン、良いものですか？ Azure Machine Learning Visual Interfaceを利用してコーディング無しで機械学習モデルの作成する方法

## Azure Tools and Data
### Azure内にリソースを作成
1. [Azure Portal](https://portal.azure.com/)にアクセスし、ログインをしてください。（アカウントのない人は[こちら](https://azure.microsoft.com/en-us/free/)でアカウントを作成してください）
2. "Create a resource"をクリックしてください。
3. "AI + Machine Learning"を選択すると、それに属するServiceが一覧で表示されるので "Machine Learning service workspace"をクリックします。
4. 名前など必要な項目を埋めていきます。"Resources Group"は、その下部の"Create new"をクリックし、新規に作成することをおすすめします。（コンテンツ終了後、一括削除等が行いやすくなるため）
5. 入力が完了したら"Review + Create"をクリックします。レビュー画面が表示されるので、"Create"をクリックします。
 </br> ![createamlresource][createamlresource]
6. ワークスペースに必要なリソースの作成には数分かかります。以下は作成されるリソースのリストになります。
</br> ![workspaceresourcelist][workspaceresourcelist]

### Azure Machine Learning Visual Interfaceを起動
1. "Machine Learning Service Workspace"を作成したリソースグループに移動します。
2. リソースグループの"Machine Learning Service Workspace"をクリックします。
3. 左側のリストから"Visual Interface"をクリックします。
4. "Launch visual interface"をクリックします。
5. これでビジュアルインターフェイスの操作画面が開きます。
</br> ![launchamlvi][launchamlvi]

### データの取得
1. 今回はKaggleで公開されているデータを利用します。Kaggleはデータサイエンティストのオンラインコミュニティです。
2. データにフィールド（qualityBool、ワインの良し悪しのラベル、モデルの予測対象）を追加しているので、下記のリンク（GitHubリポジトリ）からデータをダウンロードして下さい。
* [今回利用するワインのデータセット](https://github.com/cassieview/IntroToAzureMLInterface/blob/master/dataset/winequality-red.csv)
* [元データ（Kaggle Dataset）](https://www.kaggle.com/uciml/red-wine-quality-cortez-et-al-2009) 
</br> _関連出版物: P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. Modeling wine preferences by data mining from physicochemical properties. In Decision Support Systems, Elsevier, 47(4):547-553, 2009._

### 入手したデータをAzure Machine Learning Visual Interfaceへインポート
ビジュアルインターフェイスへデータをインポートする方法はいくつかあります。例えば[Import Data Module](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/import-data)を使うとAzure Blob StorageやWeb URLからHTTP経由でデータをインポートする事もできます。今回は"My Datasets"としてアップロードして利用します
1. 左下隅の"New"をクリックします。
2. 左のバーから"Datasets"を選択します。
3. "Upload from Local file"を選択します。
4. 先程ダウンロードしたデータを選択します。
5. お好みで名前や説明などを変更してください。 (データが増えてくると説明があるとわかりやすいです。)
</br>![uploaddataset][uploaddataset]

### 新しい実験の作成
1. 左下隅の"New"をクリックします。
2. "Blank Experiment"をクリックします。
3. ワークスペース左上の"Experiment created on xx/xx/xxxx"を選択すると、名前を編集できます。わかりやすいようにあとから"ワイン判定"などとつけておくと良いかもしれません。
4. "Saved Datasets" > "My Datasets"から先程アップロードしたでデータを見つけて下さい。
5. 見つけたデータをドラックしてワークスペースに置きます。
</br> ![createexpadddata][createexpadddata]

## モデルの構築
### データセットにラベル属性を割り当て
ここまでで、実験が作成されデータがインポートされました。ここからはモデルの構築をしていきましょう。左側にはモデルを構築するための様々なモジュールが準備されています。これらのモジュールをドラッグアンドドロップで配置していきます。
1. "Transformation" > "Manipulation"から"Edit Metadata"をドラッグアンドドロップでワークスペースに配置します。
2. 先程配置したデータモジュールと"Edit Metadata"を接続します。接続するにはデータモジュールの下部についている丸をクリックし、ドラックすると紐がでるので、そのまま"Edit Metadata"の上の丸までドラックします。
3. 配置した"Edit Metadata"をクリックすると、ワークスペース右側にPropertiesが表示されます。そのなかの"Edit Columns"をクリックします。
4. テキストボックスに `qualityBool` と入力し、OKをクリックします。
5. "Data type"を"Unchanged"から"Boolean"に変更すれば完了です。
</br>![editmetalabel][editmetalabel]

### Runをしてみる
1. ワークスペース中央下部にある"Run"をクリックします。
2. "Create new"をクリックしてトレーニングに利用するコンピューティングリソースを作成します。
3. コンピューティングリソースの名前を入力します。
4. "Run"をクリックします。
</br>![runexp][runexp]

### 利用するカラム（列）の選択
1. "Transformation" > "Manipulation"から"Select Columns in dataset"をドラッグアンドドロップでワークスペースに配置します。
2. "Edit Metadata"と"Select Columns in dataset"を接続します。"Edit Metadata"の下部についている丸をクリックし、そのまま"Select Columns in dataset"の上の丸までドラックします。
3. "Select Columns in dataset"をクリックし、ワークスペース右側に表示されたProperties内の"Edit Columns"をクリックします。
4. `quality`は予測させてたい対象の `qualityBool`と同義で答えとなってしまうためデータから除外します。左側にある"With Rules"をクリックし、 "Begin Columns"を"All Columns"にし、"Include"となっていた項目を"Exclude"にし、テキストボックスに`quality`と入力し、OKをクリックします

### データの視覚化
データの視覚化はデータサイエンティストをする上で重要なプロセスです。
1. データを視覚化するには、`Edit Metadata`の下部の丸を右クリックし、"Visualize"を選択します。
2. 各列をクリックすると、右側に視覚化されたデータや統計情報が表示されます。
</br>![editdatavisual][editdatavisual]

### データの分割
モデルをトレーニングする標準的な方法は、入力したデータを行で分割してモデルの作成（トレーニング）に利用するものと作成したモデルの評価に利用するものに分けます。データ件数にもよりますが、今回は70%をトレーニング用に利用し、残った30%を評価ように利用します。
なぜトレーニング用と評価用でデータを分けるかといいますと、もしモデル作成で利用したデータを、モデルの評価に利用した場合、そのモデルは入力されたデータの結果を知っていることなり、正答率が不当に上がってしまいます。こういったことを避けるため、トレーニング用と評価用でデータを分けます。

1. 左上部のテキストボックスに"Split Data"と入力します。
2. "Split Data"モジュールが表示されるので、ドラッグアンドドロップでワークスペースに配置します。
3. "Edit Meta"と"Split Data"を今までと同じように接続します。
4. "Split Data"を選択し、左側の"Fraction of rows in the first output dataset"を`0.5` から `0.7`に変更します。

### モデルのトレーニング、スコアリング、評価
ここまででデータの準備ができました。ここからはモデルをトレーニングするように構築していきます。

1. モデルを構築するとき、どのアルゴリズムでモデルを作成するかを選択する必要があります。一般的には、いろんなアルゴリズムを試して、どれが良いかを決めたり、 [チートシート](https://docs.microsoft.com/en-us/azure/machine-learning/studio/algorithm-cheat-sheet)を利用して決定したりします。 今回のモデルでは`Two-Class Logistic Regression`を利用します。
2. ワークスペースに以下のモジュールを追加します。これらはテキストボックスから検索してもいいですし、左側の"Machine Learning"の配下にすべて揃っているので、そこから探して持ってきてもいいです。 `Two-Class Logistic Regression`, `Train Model`, `Score Model`, `Evaluate Model`
</br> _ヒント: モジュールについて質問や疑問がある場合、そのモジュールをクリックし、右下に表示される"more help"をクリックするとそのモジュールに対する詳しい説明を読むことができます。_
3. 下の画像のようにモジュールを接続します。
4. `Train Model`をクリックし、右側の"Edit Columns"をクリックします。
5. テキストボックスに`qualityBool`と入力します。ここで選択した対象がこのモデルで予測する対象となります。
6. 中央下部の"Run"をクリックします。これで少しするとトレーニング済みモデルが作成されます。
</br>![splittraintestgif][splittraintestgif]

### モデルの精度を確認
ここまででトレーニング済みモデルが作成されました。結果を視覚化して確認します。

1. `Evaluate Model`の下部の丸を右クリックします。
2. "Visualize"を選択します。
3. モデルを評価した結果（統計情報）が表示されます。[よく理解したいのであれば、このページを見て下さい。](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/evaluate-model#bkmk_classification)
4. 十分な精度となっていますが、工夫をすればもっと精度を高めることができると思われます。

### 精度を向上させる様々な方法
1. データを視覚化して、予測対象と相関関係のあるカラムがどれかを確認する
2. 他のアルゴリズムを試してみる。今回は`Two-Class Logistic Regression`を利用しましたが、これを他のアルゴリズムに置き換えるだけで違ったアルゴリズムで学習済みモデルを作成できます。アルゴリズムの選び方はチートシートをみてみましょう。
3. 十分な量のデータを用意する。データの量が少ないと、そのデータから読み取れるパターンが少ないため、新しいパターンに対応できず良い精度で予測はできません。また、例えば季節要因がある予測なのに夏だけのデータで作ったモデルで冬のことを予測しても上手く行かないので十分に網羅しているかにも気をつけましょう。
4. データにノイズ（異常）が多い場合、そのノイズに引っ張られて精度が上がらない場合があるので、利用するデータを見極めましょう。

### Webサービスとしてデプロイ
トレーニングしたモデルの精度が十分な域に達したら、次はそのモデルWebサービスにデプロイしましょう。

1. 下部にある"Create predictive experiment"をクリックします。
2. 組んだモジュールが変形します。"Run"をクリックし、計算リソースを確認してもう一度"Run"をクリックします。
3. これで作成したモデルが左の"Trained Models"に表示され、他の実験でも利用できるようになりました。
4. 下部の"Deploy Web Service"をクリックします。
5. 次にこのモデルのデプロイ先であるコンピューティングリソースを作成する必要があります。(まだ作成していない場合)
6. "Create new"をクリックして"Go to azure portal link"のリンクをクリックします。すると、新しいタブが開くので、そこでコンピューティングリソースを作成します。Compute nameは任意の名前を入力し、Compute typeはKubemetes Serviceを選択し、リージョンは任意の場所（東日本など）を指定し、他の設定はそのままで"Create"をクリックします。詳しくは下のgifを御覧ください。
7. コンピューティングリソースを作成したら、ビジュアルインターフェイスに戻り、"Select exiting"をクリックし、隅にある"Refresh"をクリックして、今作成したコンピューティングリソースを表示させます。
8. 作成したコンピューティングリソースを選択し、"Deploy"をクリックします。これでWebサービスとしてデプロができました。
</br>![createapigif][createapigif]

### Webサービスをテストする

1. 左端にある丸いマーク（"Web Service"のアイコン）をクリックします。作成されたWeb サービスが確認できます。
2. 作成したWeb サービスをクリックします。
3. ここでAPIを利用するために必要な情報が確認でき、テストも可能です。

### これでAzure Machine Learning Visual Interfaceを使用して機械学習モデルが作成されました！  🎉✨

## 機械学習初心者向けの落とし穴

### 最高の精度を得るために利用する特徴を選択しましょう (Feature Enginnering)
* 今回はすべてのカラム（≒特徴）（`quality`は`qualityBool`と同義のため除外）を利用してモデルを構築しましたが、最適な予測を行うには利用するカラムを検討することが大切です。
* データの視覚化と、各分野の専門家との会話はどのカラムを利用するかの判断に役立ちます。利用するカラムは一回で決めるのではなく何度も反復で実験を行い、どの特徴を利用することで精度が高くなるか試行錯誤を行います。

### 精度100%はよいことなのか。 オーバーフィッティングについて
* オーバーフィッティングとは、モデルが評価対象の予測に特化した状態で、実験をしているときはとても良い精度となりますが、実際にデプロイして使い始めてみると全然正しい予測をしてくれません。これは、評価対象のデータをモデルの作成に利用した場合によく起こります。
* データのバランスが悪い場合（例えば、夏のデータが1000件あるのに冬のデータは100件しかない）、これもオーバーフィッティングしたモデルになりやすいです。こういった不均衡なデータを処理する手法は多数あります。例えば、オーバーサンプリング、偽データの作成、より多くのデータ収集、果汁データ、ドロップアウトなどです。
もし、不均衡なデータに出会ったら試してみてください。

## リンク集
[Machine Learning Visual Interface Overview Doc](https://docs.microsoft.com/en-us/azure/machine-learning/service/ui-concept-visual-interface) </br>
[Flavors of Machine Learning Doc](https://docs.microsoft.com/en-us/azure/machine-learning/studio/algorithm-choice#flavors-of-machine-learning) </br>
[MS Learn Intro to Data Science in Azure](https://docs.microsoft.com/en-us/learn/modules/intro-to-data-science-in-azure/1-introduction) </br>
[Stanford Machine Learning Cheatsheet](https://stanford.edu/~shervine/teaching/cs-229/cheatsheet-supervised-learning)</br>

## 同じモデルをPythonで構築する方法
[このリンクからNotebookでのコードを取得できます。](https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/notebooks/winequality-red.ipynb) Pythonで実行する場合はここで作成したAzure Machine Learning WorkspaceのNotebook VMを利用することをオススメします。


[create-resource]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createresource.png "Create Resource"
[select-workspace]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/selectworkspace.PNG "Select Workspace"
[workspaceresourcelist]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/workspaceresourcelist.PNG

[viportal]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/viportal.PNG
[navvisualinterface]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/navvisualinterface.PNG
[launchvisualinterface]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/launchvisualinterface.PNG
[runexperiment]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/runexperiment.PNG
[datasetuploadfields]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/datasetuploadfields.PNG
[newdatasetupload]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/newdatasetupload.png
[createexpadddata]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createexpadddata.gif
[createamlresource]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/createamlresource.gif
[launchamlvi]:https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/launchamlvi.gif
[uploaddataset]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/uploaddataset.gif
[editmetalabel]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/editmetalabel.gif
[runexp]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/runexp.gif
[splittraintestvid]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/splittraintestvid.mp4
[splittraintestgif]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/splittraintestgif.gif
[editmetaselectcolumns]:https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/editmetaselectcolumns.gif
[editdatavisual]:https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/editdatavisual.gif
[createapigif]:https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/createapigif.gif
