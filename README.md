# hand-gesture-recognition-using-mediapipe
MediaPipe(Python版)を用いて手の姿勢推定を行い、検出したキーポイントから<br>ハンドサインとフィンガージェスチャーを認識するサンプルプログラムです。
![mqlrf-s6x16](https://user-images.githubusercontent.com/37477845/102222442-c452cd00-3f26-11eb-93ec-c387c98231be.gif)

本リポジトリは以下の内容を含みます。
* サンプルプログラム
* ハンドサイン認識モデル(TFLite)
* フィンガージェスチャー認識モデル(TFLite)
* ハンドサイン認識用学習データ、および、学習用ノートブック
* フィンガージェスチャー認識用学習データ、および、学習用ノートブック

# Requirements
* mediapipe 0.8.1
* OpenCV 3.4.2 or Later
* Tensorflow 2.3.0 or Later<br>tf-nightly 2.5.0.dev or later (LSTMを用いたモデルのTFLiteを学習したい場合のみ)
* scikit-learn 0.23.2 or Later (学習時に混同行列を表示したい場合のみ) 
* matplotlib 3.3.2 or Later (学習時に混同行列を表示したい場合のみ)

# Demo
Webカメラを使ったデモの実行方法は以下です。
```bash
python app.py
```

デモ実行時には、以下のオプションが指定可能です。
* --device<br>カメラデバイス番号の指定 (デフォルト：0)
* --width<br>カメラキャプチャ時の横幅 (デフォルト：960)
* --height<br>カメラキャプチャ時の縦幅 (デフォルト：540)
* --use_static_image_mode<br>MediaPipeの推論にstatic_image_modeを利用するか否か (デフォルト：未指定)
* --min_detection_confidence<br>
検出信頼値の閾値 (デフォルト：0.5)
* --min_tracking_confidence<br>
トラッキング信頼値の閾値 (デフォルト：0.5)

# Directory
<pre>
│  app.py
│  keypoint_classification.ipynb
│  point_history_classification.ipynb
│  
├─model
│  ├─keypoint_classifier
│  │  │  keypoint.csv
│  │  │  keypoint_classifier.hdf5
│  │  │  keypoint_classifier.py
│  │  │  keypoint_classifier.tflite
│  │  └─ keypoint_classifier_label.csv
│  │          
│  └─point_history_classifier
│      │  point_history.csv
│      │  point_history_classifier.hdf5
│      │  point_history_classifier.py
│      │  point_history_classifier.tflite
│      └─ point_history_classifier_label.csv
│          
└─utils
    └─cvfpscalc.py
</pre>
### app.py
推論用のサンプルプログラムです。<br>また、ハンドサイン認識用の学習データ(キーポイント)、<br>
フィンガージェスチャー認識用の学習データ(人差指の座標履歴)を収集することもできます。

### keypoint_classification.ipynb
ハンドサイン認識用のモデル訓練用スクリプトです。

### point_history_classification.ipynb
フィンガージェスチャー認識用のモデル訓練用スクリプトです。

### model/keypoint_classifier
ハンドサイン認識に関わるファイルを格納するディレクトリです。<br>
以下のファイルが格納されます。
* 学習用データ(keypoint.csv)
* 学習済モデル(keypoint_classifier.tflite)
* ラベルデータ(keypoint_classifier_label.csv)
* 推論用クラス(keypoint_classifier.py)

### model/point_history_classifier
フィンガージェスチャー認識に関わるファイルを格納するディレクトリです。<br>
以下のファイルが格納されます。
* 学習用データ(point_history.csv)
* 学習済モデル(point_history_classifier.tflite)
* ラベルデータ(point_history_classifier_label.csv)
* 推論用クラス(point_history_classifier.py)

### utils/cvfpscalc.py
FPS計測用のモジュールです。

# Training
ハンドサイン認識、フィンガージェスチャー認識は学習データの追加、変更、モデルの再トレーニングが出来ます。

### ハンドサイン認識トレーニング方法
#### 1.
<img src="https://user-images.githubusercontent.com/37477845/102235423-aa6cb680-3f35-11eb-8ebd-5d823e211447.jpg" width="60%">
<img src="https://user-images.githubusercontent.com/37477845/102242918-ed328c80-3f3d-11eb-907c-61ba05678d54.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102244114-418a3c00-3f3f-11eb-8eef-f658e5aa2d0d.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102246723-69c76a00-3f42-11eb-8a4b-7c6b032b7e71.png" width="50%">

### フィンガージェスチャー認識トレーニング方法
#### 1.
<img src="https://user-images.githubusercontent.com/37477845/102249074-4d78fc80-3f45-11eb-9c1b-3eb975798871.jpg" width="60%">
<img src="https://user-images.githubusercontent.com/37477845/102244148-49e27700-3f3f-11eb-82e2-fc7de42b30fc.png" width="80%">
<img src="https://user-images.githubusercontent.com/37477845/102246771-7481ff00-3f42-11eb-8ddf-9e3cc30c5816.png" width="50%">
<img src="https://user-images.githubusercontent.com/37477845/102246817-8368b180-3f42-11eb-9851-23a7b12467aa.png" width="60%">

# Reference
* [MediaPipe](https://mediapipe.dev/)

# Author
高橋かずひと(https://twitter.com/KzhtTkhs)
 
# License 
hand-gesture-recognition-using-mediapipe is under [Apache v2 license](LICENSE).
