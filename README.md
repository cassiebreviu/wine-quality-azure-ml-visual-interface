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
2. データにフィールド（qualityBool）を追加しているので、下記のリンク（GitHubリポジトリ）からデータをダウンロードして下さい。
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
4. `quality`は利用しないため、除外します。左側にある"With Rules"をクリックし、 "Begin Columns"を"All Columns"にし、"Include"となっていた項目を"Exclude"にし、テキストボックスに`quality`と入力し、OKをクリックします

### データの視覚化
データの視覚化はデータサイエンティストをする上で重要なプロセスです。
1. データを視覚化するには、`Edit Metadata`の下部の丸を右クリックし、"Visualize"を選択します。
2. 各列をクリックすると、右側に視覚化されたデータや統計情報が表示されます。
</br>![editdatavisual][editdatavisual]

### Split the Data
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
6. 中央下部の"Run"をクリックします。
</br>![splittraintestgif][splittraintestgif]

### Check Accuracy of Model
We now have a trained model in Azure Machine Learning Visual Interface. Lets visualize our results to see how it performed.

1. Right click on the button circle of the `Evaluate Model` module.
2. Select "Visualize" from the menu that popped up
3. [How to understand metrics for classification models](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/evaluate-model#bkmk_classification)
4. Our accuracy is ok, but we can probably do better.

### Different ways to Improve Accuracy
1. Evaluate the selected features with data visualizations to see if they are helping or hurting accuracy
2. Try a different machine learning algorithm
3. Do you have enought data? Sometimes a low accuracy means you dont have enough data
4. If the data is noisy it can be hard for the algorithm to read the signal.

### Deploy the Web Service
Once the model has an acceptable or "good enough" accuracy its time to deploy your model to a web service.

1. Click "Create predictive experiment" in the bottom nav of the workspace
2. Click "Run" on the predictive experiment, select the compute and click "Run"
3. Now the model you created will show up under "Trained Models" in the left nav of the workspace. This allows you to import trained models into different experiments
4. Click "Deploy Web Service" in the bottom nav of the workspace
5. Now we need to create a web service compute target (if you dont already have one)
6. Click "Create new" and then click the "Go to azure portal link" think will open a new tab and bring you to the azure machine learning workspace resource with compute selected from the left hand nav. Follow the instructions in the pane to create the compute target for the web service.
7. Once you have created the compute target, click refresh in the corner of the pane to show the newly created compute target
8. Select the Compute target and click "Deploy"
</br>![createapigif][createapigif]

### Test the Web Service

1. Select the "Web Service" icon on the left nav of the workspace. The web service that was created will be listed.
2. Click the web service that was created
3. Here you can test and get the information needed to consume the API created.

### You have now created a machine learning model using Azure Machine Learning Visual Interface! 🎉✨

## Machine Learning Beginner Gotchas

### Selecting what features will give the best accuracy (Feature Enginnering)
* In this example we used all the attributes in the datasets as features. When building a model is it important to think about what features help make a decision or a good prediction. 
* Data visualizations and talking to subject matter experts can help identify what features are best. Additionally it is an iterative process, meaning playing around and using trial and error of what features are going to get the best accuracy.

### Is 100% accuracy always good? What overfitting is.
* Overfitting a model means you dont have enough data for it to actual "learn" so it will do great on your data but as soon as it put out into the real world. It will fail. This is why you want to always test with unseen data. 
* If data is very imbalanced meaning you have lots of one label and little of another - this can also create overfitting and models that look like they are performing really well when they are actually not good models. There are different techniques to work with imbalanced data. Such as: Over Sampling, Creating fake data, collection more data, weighted data, dropout and others.

## Helpful Links
[Machine Learning Visual Interface Overview Doc](https://docs.microsoft.com/en-us/azure/machine-learning/service/ui-concept-visual-interface) </br>
[Flavors of Machine Learning Doc](https://docs.microsoft.com/en-us/azure/machine-learning/studio/algorithm-choice#flavors-of-machine-learning) </br>
[MS Learn Intro to Data Science in Azure](https://docs.microsoft.com/en-us/learn/modules/intro-to-data-science-in-azure/1-introduction) </br>
[Stanford Machine Learning Cheatsheet](https://stanford.edu/~shervine/teaching/cs-229/cheatsheet-supervised-learning)</br>

## Want to see how to build this same model in python?
[Here is a link to the notebook](https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/notebooks/winequality-red.ipynb) included in this repo. If you want to run it I recommend using the Notebook VMs in the Azure Machine Learning Workspace you created in this workshop.


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
