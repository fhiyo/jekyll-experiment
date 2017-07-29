---
layout: post
title:  "tensorflowについてメモ置き場"
date:   2017-07-20 16:11:02 +0900
categories: machine-learning
tags: tensorflow
comments: true
---


[ディープラーニングで一般物体検出する手法”YOLO”のTensorFlow版で独自データセットを使えるようにしてみた](http://arkouji.cocolog-nifty.com/blog/2017/07/yolotensorflow-.html)
[画像認識モデルを作るための鉄板レシピ](https://www.slideshare.net/takahirokubo7792/ss-71453093)
画像認識のデータセット作成の方法について

protobufのインストール (3系)
```sh
sudo yum install -y autoconf automake libtool unzip gcc-c++ git
git clone https://github.com/google/protobuf.git
cd protobuf
./autogen.sh
./configure
make
sudo make install
which protobuf
# /usr/local/bin/protobuf
# コンパイル時は以下のコマンドを実行する
cd path/to/tensorflow/models
protoc object_detection/protos/*.proto --python_out=.
```

→x11経由でec2のcentos/7に入る
```sh
ssh -X username@ec2-server
sudo yum -y install xauth xterm xorg-x11-apps
xeyes  # x11の起動、接続が正常にできていれば、目がマウスを追うGUIアプリケーションが起動される。
sudo yum -y eog  --enablerepo=epel
eog sample.jpg
```

ec2上でBoundingbox作成のためのGUIを起動する
```sh
git clone --recursive https://github.com/tzutalin/ImageNet_Utils.git
cd ImageNet_Utils/
./downloadutils.py --downloadImages --wnid n02084071  # 画像を適当にダウンロード
sudo yum -y install qtwebkit-devel --enablerepo=epel
cd labelImgGUI/
make all
sudo yum -y install PyQt4-devel
sudo easy_install pip
sudo pip2.7 install numpy opencv-python lxml
./labelImg.py
```



### archive

[【Tensorflow】Tensorflow Object Detection API 学習させてみた](http://app.road.jp.net/?p=1985)
excelに書いた学習方法の元ネタ.
object_detection/samples/configs/ssd_inception_v2_pets.configが参考になる、と言っている。



### checkpointとは
[Checkpoint Files](https://www.tensorflow.org/programmers_guide/variables)
> Checkpoint Files
> 
> Variables are saved in binary files that, roughly, contain a map from variable names to tensor values.
> 
> When you create a Saver object, you can optionally choose names for the variables in the checkpoint files. By default, it uses the value of the tf.Variable.name property for each variable.
> 
> To understand what variables are in a checkpoint, you can use the inspect_checkpoint library, and in particular, the print_tensors_in_checkpoint_file function.

checkpoint fileは、モデルの構築時に各変数に名前をつけておいたものを再度参照できるようにするためのファイルのようだ。
要するに変数が保存してあるファイル。

eventsファイルはtensorboard用のログファイルか。



[あなたの会社は本当に機械学習を導入すべきなのか？](http://bohemia.hatenablog.com/entry/2017/04/03/205852)

超いいこと書いてあるなぁ

[高度に発達したシステムの異常は神の怒りと見分けがつかない - IPSJ-ONE2017](http://blog.yuuk.io/entry/ipsjone2017)


### protocol bufferについて
[XMLはもう不要!? Google製シリアライズツール「Protocol Buffer」](http://news.mynavi.jp/articles/2008/07/18/protocolbuffer/)
[google/protobuf](https://github.com/google/protobuf)
2008年からある相当古いやつなのか。。


> Protocol Bufferを用いてデータのシリアライズを行うには、まず対象となるデータの構造を示した「protoファイル」を作成する必要がある。
> protoファイルは、拡張子が「.proto」となるファイルで、独自の記述言語を用いてデータ構造を記述する。データ構造は「メッセージタイプ」と呼ばれ、一つのprotoファイル内に複数記述することができる。

[faster-rnnを自作の学習セット使って作りたい人の質問](https://github.com/rbgirshick/py-faster-rcnn/issues/243)

[Train Tensorflow Object Detection on own dataset](https://stackoverflow.com/questions/44973184/train-tensorflow-object-detection-on-own-dataset)

[TensorflowのFaster RCNN実装を試す](http://qiita.com/shouta-dev/items/865270ead5931d3f8dbb)

[Training Faster R-CNN on Custom Dataset](http://sgsai.blogspot.jp/2016/02/training-faster-r-cnn-on-custom-dataset.html)
↑
自作でfaster-r-cnnを作る感じのことが書いてある

[TensorFlowによるももクロメンバー顔認識（前編）](http://qiita.com/kenmaz/items/4b60ea00b159b3e00100)
これだ。。。！多分。
[github - momo_mind](https://github.com/kenmaz/momo_mind)

[using_your_own_dataset.md](https://github.com/tensorflow/models/blob/master/object_detection/g3doc/using_your_own_dataset.md)
↑
ここの通りにやれば何とかできるはず。。。。

[TensorFlow Data Input (Part 1): Placeholders, Protobufs & Queues](https://indico.io/blog/tensorflow-data-inputs-part1-placeholders-protobufs-queues/)
TFRecordWriterの使い方が後半に書いてある？
Protobuf and binary formatの項は参考になるかも。自動でデータセットのロードはよしなにやってくれるから、GPUで動かすときのデータ量の制約が無くなる可能性あり？
[Quick Start: Distributed Training on the Oxford-IIIT Pets Dataset on Google Cloud](https://github.com/tensorflow/models/blob/master/object_detection/g3doc/running_pets.md)
tfrecordのファイルを作成した後の方法が書いてあるようだ

[Configuring the Object Detection Training Pipeline](https://github.com/tensorflow/models/blob/master/object_detection/g3doc/configuring_jobs.md)
pipelineファイルを書く方法について

