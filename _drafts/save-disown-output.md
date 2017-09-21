---
layout: post
date: 2017-08-17 22:08
title: "disownしたプロセスの出力を保存したい"
categories: [shell]
tags: [command]
comments: true
published: true
---

[disown](https://www.gnu.org/software/bash/manual/html_node/Job-Control-Builtins.html#Job-Control-Builtins)は実行中のプロセスをSIGHUPシグナルが送出されても殺されないようにするようだ．こいつが正確に何をやっているのかを把握するのは難しい．．．またそのうち記事にして全体像を理解したい．

`disown`のよくある使い方としては，軽い気持ちで実行したコマンドが全く終わらず，実行終了するまでシェルを抜けることもできず帰れない，というときに使う．

```sh
$ sleep 10000  # 数時間経たないと終わらないとき．．
# まずCtrl-zでプロセスをサスペンドする
^Z
[1]+  Stopped                 sleep 10000
$ bg  # ジョブをバックグラウンドで実行
[1]+ sleep 10000 &
$ jobs  # アクティブなジョブを確認
[1]+  Running                 sleep 10000 &
$ disown %1  # %の後の数字はjobsで確認した対象としたいジョブの番号

```

これでプロセスはシェルが閉じられても (SIGHUPシグナルが送出されても) 殺されなくなる．しかし，この方法だと標準出力，標準エラー出力に吐かれていた出力は確認できなくなってしまい困っていた．  
そこで，c/c++のデバッガとしておなじみのgdbを使って標準出力と標準エラー出力をファイルに書き出す方法を調べたので残しておく．  

### 手順

1. disownしてSIGHUPシグナルでプロセスが殺されないようにする
- disownしたプロセスIDを調べる
- 標準出力、標準エラー出力用のファイルを用意する
- gdb -p で接続する
- dup2を使いファイルに接続する
- デタッチ

#### disownしてSIGHUPシグナルでプロセスが殺されないようにする

なお，このときdisownするプロセスがStoppedの状態でdisownするとjobのテーブルには登録されないので注意.
先に`bg`コマンドでrunningの状態にする必要がある．

```sh
[vagrant@localhost ~]$ sleep 10000;
^Z
[1]+  Stopped                 sleep 10000
[vagrant@localhost ~]$ disown %1
-bash: warning: deleting stopped job 1 with process group 3674
[vagrant@localhost ~]$ jobs  # activeなjobの一覧の中にない
[vagrant@localhost ~]$ ps -ef | grep sleep
vagrant   3674  0.0  0.1 107892   608 pts/0    T    14:00   0:00 sleep 10000
vagrant   3676  0.0  0.1 112644   968 pts/0    S+   14:00   0:00 grep --color=auto sleep
[vagrant@localhost ~]$ exit  # この状態でセッションを閉じると．．．
logout
Connection to 127.0.0.1 closed.
$ vagrant ssh
Last login: Mon Aug 28 13:48:54 2017 from 10.0.2.2
[vagrant@localhost ~]$ ps -ef | grep sleep
vagrant   4250  0.0  0.1 112644   968 pts/1    S+   15:01   0:00 grep --color=auto sleep
# プロセスが無くなっている

```

#### disownしたプロセスIDを調べる

```sh
ps -ef | grep HOGE
```



## 参考
[Job control (Unix)](https://en.wikipedia.org/wiki/Job_control_(Unix)#Implementation)
[What is effect of CTRL + Z on a unix\Linux application](https://superuser.com/questions/476873/what-is-effect-of-ctrl-z-on-a-unix-linux-application/476874)
[disownしたプロセスの標準出力や標準エラーを後から保存する](https://blog.be-dama.com/2014/08/12/disown%E3%81%97%E3%81%9F%E3%83%97%E3%83%AD%E3%82%BB%E3%82%B9%E3%81%AE%E6%A8%99%E6%BA%96%E5%87%BA%E5%8A%9B%E3%82%84%E6%A8%99%E6%BA%96%E3%82%A8%E3%83%A9%E3%83%BC%E3%82%92%E5%BE%8C%E3%81%8B%E3%82%89/)
[コンソールから切れたプロセスを標準出力につなげなおす](http://yudoufu.hatenablog.jp/entry/2014/02/06/001440)
