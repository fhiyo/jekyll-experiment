---
layout: post
title:  "ゼロから作るDeepLearningメモ"
date:   2017-07-02 15:27:09 +0900
categories: 機械学習
tags: deep-learning
---
ゼロから作るDeepLearningを読んだメモ．
とりあえず自分が気になった部分のみ書いていきます．


### 2.4 パーセプトロンの限界
単層のパーセプトロンではXORゲートを表現することはできないです．
単層のパーセプトロンは線形分類器であり，XORゲートは非線形な関数だからです．

![xor_perceptron]({{ site.baseurl }}/assets/img/xor.png)

上の図はXORゲートを表現する関数の一例を図示したものですが，緑三角と青丸は
どうやっても直線では分離することができず，線を曲げる必要があります．

そこで，パーセプトロンを複数組み合わせることで非線形な関数を表現します．


### 7.2.1 全結合層の問題点

画像をTraining dataとして入力する際，ピクセル間の距離や位置関係は大切な情報になるはずです．
画像はRGBの情報も考慮すれば3次元の情報であり，空間的に近いピクセルは近い値になったり，
RGBの各チャンネルの間には密接な関係があったり，距離の離れたピクセル同士はあまり関わりがなかったりするはずです．
しかし全結合層は全てのピクセルを同じように繋げて行列計算を行うため，
画像内のピクセルの位置を考慮していない計算になっています．

そこで，CNN (Convolutional Neural Network, 畳み込み層) という概念を新たに導入します．
CNNはフィルタと呼ばれる小さな2次元の配列を使い，画像の各地点で行列計算を行い計算結果をまた行列にして返します．
これによりピクセル同士の位置関係を考慮した計算を行うことができます．


### 8章 ディープラーニング

#### GoogLeNetのインセプション構造について

GoogLeNetの畳み込み層を並列にしている部分，次の層に行くときの計算がわからなかったけど，
単純にそれぞれの層の結果を足し合わせているだけっぽい (たぶん．．．)．

参考: [Going deeper with convolutions](https://arxiv.org/pdf/1409.4842.pdf)

> In order to avoid patchalignment issues, current incarnations of the Inception architecture are restricted to filter sizes 1×1,
> 3×3 and 5×5, however this decision was based more on convenience rather than necessity. It also
> means that the suggested architecture is a combination of all those layers with their output filter
> banks concatenated into a single output vector forming the input of the next stage.


