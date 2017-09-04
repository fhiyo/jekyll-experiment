---
layout: post
title:  "投稿記事のネタ置き場"
date:   2017-07-03 22:12:32 +0900
categories: ネタ置き場
tags: hoge
comments: true
---
次に投稿できるようにネタだけ置いておく場所


## bashのReadlineでもキーボードマクロが使える

[The GNU Readline Library](http://cnswww.cns.cwru.edu/php/chet/readline/rltop.html)

[Bash - ArchWiki#コマンドライン](https://wiki.archlinuxjp.org/index.php/Bash#.E3.82.B3.E3.83.9E.E3.83.B3.E3.83.89.E3.83.A9.E3.82.A4.E3.83.B3)
> Bash のコマンドラインは Readline という名前の別のライブラリによって処理されています。Readline にはコマンドラインを使用するための多数のショートカットがあります。単語ごとに前後に移動、単語の削除など。また、入力したコマンドの履歴を管理するのも Readline の仕事です。最後に、また重要なことですが、Readline はマクロを作成するのを可能にします。

bashのコマンドラインはReadlineという名前のライブラリが処理をしていたのか．zshはzleか．


```sh
\C-x (  # マクロ開始
hoge    # マクロを登録する
\C-x )  # マクロ終了
\C-x e  # マクロ実行
```

キーボードマクロの例: [Readline - ArchWiki Readline#マクロ](https://wiki.archlinuxjp.org/index.php/Readline#.E3.83.9E.E3.82.AF.E3.83.AD)

`${HOME}/.inputrc`に書かなくても`${HOME}/.bashrc`にfunctionを追加したり，[pet](https://github.com/knqyf263/pet)を使ってもいい．でもマクロという原始的なやり方な分，細かい制御ができそうな印象．

### 関連
[もっと早くから知りたかった、bashのreadline系オプション](http://qiita.com/gyu-don/items/6077bca176150de024b9)

## シングルトンが駄目な理由

[シングルトンの賢い使用法](https://www.ibm.com/developerworks/jp/webservices/library/co-single.html)

いくつかの理由をざっと見した感じ、ピンと来ないのは自分がテストを書く意識が足りないからだと思う。
**コードを書くときはテストを書かなければならない。**



## devsecopsとは
[What is DevSecOps?](http://www.devsecops.org/blog/2015/2/15/what-is-devsecops)



## 楽観的ロックと悲観的ロック

railsのロックについての説明: [2010/08/17(火) 楽観的ロックと悲観的ロック](http://snjx.info/diary/snjx/041)
> 悲観的ロックは、レコードを読み込むときかける形式のロックで、要するに「これからこのレコードをいろいろいじるのでほかのやつらは触るなよ」と言う宣言。実装はDBエンジンに依存している。MySQLなりPostegrasなり、主要なDBエンジンはたいてい対応しているはず。
楽観的ロックは、レコードを書き込むときに問い合わせる形式のロックで、要するに「とりあえず変更したレコードを手元に持っているけどほんとにこれ書き込んでいいのかな」という確認。実装はActiveRecordが担う。テーブルにlock_versionという項目を作っておく必要がある。詳細はぐぐってくれ。
理解するポイントは、実はロックをかけるタイミングではなく、ロックをはずすタイミングがあるかないか。だと思う。

悲観的ロックっていうのは、ロックをかけた人が責任持ってロックをはずす必要がある。saveなりなんなりを契機にしてDBエンジンがちゃんとロック解除の世話を見てくれるのでユーザはあんまり意識しなくてすんでいるけど、確実に解除はしておかないと、つまりデッドロックに陥ってしまう。

対して、楽観的ロックにはそもそもロックをかけるという概念が薄い。railsでいえば、saveするたびにlock_versionがインクリメントされる。もし、あるレコードをsaveしたときに、そのレコードをfindしてきたときのlock_versionと違っていたらエラーをraiseするのが楽観的ロック。

railsでロックをかけるという場合、これはもう、ほとんどWebシステムで、editで画面を作るときにfindされupdateをpostされたときにsaveされることになる。findの時点とpostの時点でセッションが違うのだ。
悲観的ロックとは、バッチ処理などで一連のトランザクションが進行する間だけレコードの整合性を保障するための仕組みである。ひとつのプロセスなりスレッドなりセッションなりで処理が完結する場合に有効な戦略ということ。
findの時点とsaveの時点でセッションが違うと、システムが責任もって処理を完結させることができない可能性が出てくる。たとえばeditで画面を表示(つまりfindでレコードをロード)したあとブラウザを閉じちゃって、ついにupdateを呼び出さなかった場合、ロックを解除する契機がなくなってしまう。
これが楽観的ロックだと、saveするときにだけ上書きできるかどうか確認する。楽観的ロックであれば、findとsaveの同期を取る必要がない。saveの時だけ判断すればいいのだ。

てなわけで、結局railsではほとんど楽観的ロックしか、出番がないってことですな。



## ovirt
[oVirt](http://searchservervirtualization.techtarget.com/definition/oVirt)
何か公式のページの証明書がエラーになってて警告が出たから見てない．．



## torch image inastall

```sh
$ luarocks install image  # 1.1.alpha-0
$ sudo yum -y libjpeg-dev libpng12-dev
# Qtを使用して画像の可視化
$ luarocks install qtlua
$ luarocks install qttorch
$ which qlua
/home/me/torch/install/bin/qlua
$ qlua
Lua 5.1 Copyright (...)
> require('image')
> l = image.lena()
> image.display(l)
# utility関数用のライブラリをインストール
$ luarocks install penlight  # 1.5.4-1  for file io
$ luarocks install fun  # 0.1.3-1  for high order functions
$ luarocks install dkjson  # 2.5-2  json parser
$ luarocks install lpeg  # 1.0.1-1  dkjson dependencies
$ luarocks install luacheck  # 0.20.0-1  lint tool
```



## 08/28/17勉強内容
- Qtとは
- GPLとは (GNU Privacy Guard, GPGとは違う)
- Luaの概要
- DLL Hellについて
- userlandについて (kernelとの権限の関係)
- DMCA: デジタルミレニアム著作権法(Digital Millennium Copyright Act)の略称


## dllって何であるの？

[DLL Hell の終焉](https://msdn.microsoft.com/ja-jp/library/ms811694.aspx)

> Unix は、伝統的に完全にリンクされたイメージでアプリケーションを出荷しています。現在の Unix は共有ライブラリをサポートしていますが、多くの Unix ベンダが、引き続き静的にリンクされたイメージを出荷しています。この場合、アプリケーションは完全に自立しているため、新しいライブラリやアプリケーションをインストールすることで、既存のプログラムが動かなくなるようなことはありません。アプリケーション ベンダは、別のソフトウェアのインストールによって自分たちの製品が壊されるという心配をせずに済むのです。そこで、この質問になります。「DLL のアプローチが静的にリンクされたイメージのアプローチよりも堅牢でないのなら、なぜ DLL を使うアプリケーションを出荷したいと思うのでしょうか？」
> 主張 1: DLL は、ディスク スペースを節約します。ほとんどすべてのアプリケーションは、メモリや他のリソースを割り当てる必要があります。もしコンピュータ上に Msvcrt.dll を使うアプリケーションやユーティリティが 501 個あって、これらが静的にリンクすると、少なめに見積もっても Msvcrt.dll のサイズの 500 倍のディスク スペースを無駄にすることになります。
> 注意:     使用しているファイル システムによっては、実際の浪費スペースがさらに多くなる場合があります。FAT16 ファイル システムでは、整数単位の固定長ファイル ブロックでファイルを格納します。ファイルの最後のブロックは完全に埋まらないことがあるため、平均でファイルごとに 2 分の 1 ブロックずつディスク スペースが無駄になります。
> 現実: DLL は確かにディスク スペースを節約します。しかし、ディスク スペースはただ同然であり、また安くなる一方でしょう。それでもなお、共通のコードが安全に共有可能である限りにおいて、DLL は有益だといえます。
> 主張 2: DLL はメモリ マッピングと呼ばれる共有テクニックを使って、メモリを節約します。Windows はグローバル ヒープに DLL をロードしてから、その DLL をロードする各アプリケーションのアドレス スペースに DLL のアドレス範囲をマップ しようとします 。10 の異なるプロセスがあって、それらがすべて Msvcrt.dll を使う場合、そのコピーを 10 個ロードする代わりに Msvcrt.dll の同じインスタンスを共有することができます。
> 現実: ほとんどのプロセスでは、それぞれ特定の DLL をロードし、またその DLL の 1 つのグローバル インスタンスを共有できます。共通の DLL を共有することは、メモリ ロードをかなり節約します。しかし、Windows では、複数プロセスによってロードされた 1 つの DLL インスタンスを常に共有できるわけではありません。



## POSIX準拠のshell scriptかどうかをテストするには？

[How can I test for POSIX compliance for shell scripts?](https://unix.stackexchange.com/questions/48786/how-can-i-test-for-posix-compliance-for-shell-scripts)によると，POSIX準拠なshell scriptを書くことよりも，移植性がある方がはるかに強い要件なのだそうだ．任意のPOSIX shellで動作させるようにスクリプトを書くのは大変ではないが，世界中のshell全てで動作するスクリプトを得ることははるかに難しい．  
ここの回答に書かれている[Autoconf guide to portable shell](https://www.gnu.org/software/autoconf/manual/autoconf.html#Portable-Shell)を参考にして

## tensorflowのcpu拡張命令有効版をインストールする

#### 参考
[Python: Keras/TensorFlow の学習を CPU の拡張命令で高速化する (Mac OS X)](http://blog.amedama.jp/entry/2017/03/08/223308)
[Installing TensorFlow from Sources](https://www.tensorflow.org/install/install_sources)
[jarutis/tf_serving.sh](https://gist.github.com/jarutis/6c2934705298720ff92a1c10f6a009d4)

```sh
sudo su

# bazelのインストール
yum -y install java-1.8.0-openjdk-devel
yum -y install gcc gcc-c++ kernel-devel make automake autoconf swig git unzip libtool binutils

# Bazel
wget https://github.com/bazelbuild/bazel/releases/download/0.5.2/bazel-0.5.2-installer-linux-x86_64.sh | sh
chmod +x bazel-*
./bazel-*
# export PATH=/usr/local/bin:$PATH  # パスが通ってない場合は追加

# Tensorflowのソースコードを取得
cd /usr/local/src
git clone https://github.com/tensorflow/tensorflow.git

$ ./configure
Extracting Bazel installation...
......................................................................
You have bazel 0.5.2 installed.
Please specify the location of python. [Default is /bin/python]: /usr/bin/python3.5
Found possible Python library paths:
  /usr/lib/python3.5/site-packages
  /usr/lib64/python3.5/site-packages
Please input the desired Python library path to use.  Default is [/usr/lib/python3.5/site-packages]

Using python library path: /usr/lib/python3.5/site-packages
Do you wish to build TensorFlow with MKL support? [y/N] N
No MKL support will be enabled for TensorFlow
Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]:
Do you wish to use jemalloc as the malloc implementation? [Y/n] N
jemalloc disabled
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] n
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with Hadoop File System support? [y/N] n
No Hadoop File System support will be enabled for TensorFlow
Do you wish to build TensorFlow with the XLA just-in-time compiler (experimental)? [y/N] n
No XLA JIT support will be enabled for TensorFlow
Do you wish to build TensorFlow with VERBS support? [y/N] n
No VERBS support will be enabled for TensorFlow
Do you wish to build TensorFlow with OpenCL support? [y/N] n
No OpenCL support will be enabled for TensorFlow
Do you wish to build TensorFlow with CUDA support? [y/N] n
No CUDA support will be enabled for TensorFlow
Do you wish to build TensorFlow with MPI support? [y/N] n
MPI support will not be enabled for TensorFlow
Configuration finished

$ bazel build -c opt --copt=-msse4.1 --copt=-msse4.2 --copt=-mavx --copt=-mavx2 --copt=-mfma //tensorflow/tools/pip_package:build_pip_package

```


## torchのインストール

```sh
$ git clone https://github.com/torch/distro.git ~/torch --recursive
$ cd ~/torch
$ bash install-deps;
# No package python-ipython available.

$ ./install.sh
Do you want to automatically prepend the Torch install location
to PATH and LD_LIBRARY_PATH in your /home/fhiyo/.bashrc? (yes/no)
[yes] >>>
yes
$ source ${HOME}/.bashrc
$ which th
~/torch/install/bin/th  # このように出力されれば成功
$ th

  ______             __   |  Torch7
 /_  __/__  ________/ /   |  Scientific computing for Lua.
  / / / _ \/ __/ __/ _ \  |  Type ? for help
 /_/  \___/_/  \__/_//_/  |  https://github.com/torch
                          |  http://torch.ch

th>
# replが起動されれば成功

```

Torchの使い方 (in Jupyter): [Deep Learning with Torch: the 60-minute blitz](https://github.com/soumith/cvpr2015/blob/master/Deep%20Learning%20with%20Torch.ipynb)



## built-in commandについて
[Why is echo a shell built in command?](https://unix.stackexchange.com/questions/1355/why-is-echo-a-shell-built-in-command)


## cd -@の意味

```sh
bash-4.4$ cd -@ tmp
bash: cd: -@: invalid option
cd: usage: cd [-L|[-P [-e]] [-@]] [dir]
```

なぜ怒られる？
> On systems that support it, the -@ option presents the extended attributes associated with a file as a directory.

サポートしているシステムとは？



## \[と\[\[の違い

[What is the difference between test, \[ and \[\[ ?](http://mywiki.wooledge.org/BashFAQ/031)
\[\[はbashやksh, zshなど，特定のPOSIX実装版のshellでしか使えない．
\[\[の方がハマりどころが少ない模様．



## シェルの変数を整理する

[Advanced Bash-Scripting Guide:Chapter 3. Special Characters](http://tldp.org/LDP/abs/html/special-chars.html)


```sh
$ echo ${!}
74573
$ echo ${-}
05679BJNXZghilms
$ echo ${_}
echo
$ echo ${:}
zsh: unrecognized modifier

$ echo ${array}
1 2 3
^_^  ~/Dropbox/code/c (development) <U>
$ echo ${#array[@]}
3
^_^  ~/Dropbox/code/c (development) <U>
$ echo ${#array}
3
```

[2.5.2 Special Parameters](http://pubs.opengroup.org/onlinepubs/9699919799/utilities/V3_chap02.html#tag_18_05_02)


## shellのbuilt-in commandのmanを見るには

```sh
$ man bash
# 該当のコマンドを検索

```



## python Tips

```python
>>> 'hoge' * True + 'fuga' * False
'hoge'
```


## virtualboxのprivate_networkを動かすには
[VirtualBox Internal Network](https://www.vagrantup.com/docs/virtualbox/networking.html)

```ruby
Vagrant.configure("2") do |config|
  config.vm.network "private_network", ip: "192.168.50.4", virtualbox__intnet: true
end
```

`virtualbox__intnet: true`の記述が必要．
(デフォルトではhostonlyネットワークが指定されている？ようだ？)

[VirtualBox Internal Network](https://www.vagrantup.com/docs/virtualbox/networking.html#virtualbox-internal-network)

```ruby
Vagrant.configure("2") do |config|
  config.vm.network "private_network", ip: "192.168.50.4"
end
```

ではipが振られていなかった．なぜ？Vagrant側に問い合わせるか，ソースコード読んで解析したい．．．



## /usr/bin/envの復習

[What type of path in shebang is more preferable?](https://askubuntu.com/questions/88257/what-type-of-path-in-shebang-is-more-preferable)
`/usr/bin/env python`は`${PATH}`を頭から順に見ていき，`python`を発見したところでそれを実行するようだ．




## Gemfileの内容を刷新したい
> Main problem is that you are using the wrong dependencies, comment everything in your Gemfile and just use github-pages. Then work from there and everything you do locally will be reflected online.



## Cythonについて
[Pythonを高速化するCythonを使ってみた](http://kesin.hatenablog.com/entry/20120306/1331043675)
> 何が遅いかというと、致命的なことに四則演算が遅いです。でも他の動的型付け言語でスクリプト言語と呼ばれるPerl, Ruby, Javascript も C, Javaのようなコンパイルを行う静的型付け言語に比べれば圧倒的に遅いです（近年ではJavascriptのように著しく進歩した言語もあるので必ずしもそうだとは言えませんが）。
スクリプト言語が遅い原因の一つは、**変数の型が指定されていないので型のチェックを毎回行う必要があるからです。**この特性があるおかげ自動的に型を変換してオーバーフローを防いでくれるというメリットもあるのですが、どうしても静的型付け言語よりは速度を出すことができません。
なら**Pythonのコードに型指定を加えてコンパイルしちゃえばいいじゃん！というのがCythonです**。正確にはPythonライクな文法で書いたコードをC/C++に変換してコンパイルします。噂では単純な計算だとPythonのコードを実行するより100倍以上も高速化することもあるらしい（！）ということで試してみました。





## git-secretsを使って安心なgithub生活を
[AWSアカウントを取得したら速攻でやっておくべき初期設定まとめ](http://qiita.com/tmknom/items/303db2d1d928db720888)
[クラウド破産しないように git-secrets を使う](http://qiita.com/pottava/items/4c602c97aacf10c058f1)


## Shippableについて
[Shippable](http://docs.shippable.com/)


## deployment vs release
[What is the difference between “deployment” and “release”?](https://english.stackexchange.com/questions/182724/what-is-the-difference-between-deployment-and-release?newreg=2e014edc71344b3089a05088f4d1b617)


## レンダリングモードについて

[DOCTYPEスイッチ－HTMLの基本](http://www.htmq.com/htmlkihon/302.shtml)

> 最近のブラウザには、「標準モード(Standards)」と、「互換モード(Quirks)」、２つのレンダリングモードがあります。
>
> 標準モードでは、文法を厳格に解釈するため、大文字・小文字の違いは区別されます。例えば、「Post」と「post」は異なるクラス名として解釈します。一方、互換モードは文法が不正確でも従来のブラウザのように解釈するため、「Post」と「post」は同じものとして解釈します。
>
> テスト環境のブラウザが互換モードになっている場合、CSSセレクタの大文字・小文字にミスがあってもスタイルは適用されますが、いざ本番環境で標準モードにしてみたら適用されない、といったことが起こります。


## ruby 2.4.1でgem install nokogiriがエラーになる

```sh
$ gem install nokogiri
Building native extensions.  This could take a while...
ERROR:  Error installing nokogiri:
	ERROR: Failed to build gem native extension.

    current directory: /usr/local/lib/ruby/gems/2.4.0/gems/nokogiri-1.8.0/ext/nokogiri
/usr/local/opt/ruby/bin/ruby -r ./siteconf20170813-19479-1gdd134.rb extconf.rb
checking if the C compiler accepts ... yes
checking if the C compiler accepts -Wno-error=unused-command-line-argument-hard-error-in-future... no
Building nokogiri using packaged libraries.
Using mini_portile version 2.2.0
checking for iconv.h... yes
checking for gzdopen() in -lz... yes
checking for iconv using --with-opt-* flags... yes
************************************************************************
IMPORTANT NOTICE:

Building Nokogiri with a packaged version of libxml2-2.9.4
with the following patches applied:
	- 0001-Fix-comparison-with-root-node-in-xmlXPathCmpNodes.patch
	- 0002-Fix-XPointer-paths-beginning-with-range-to.patch
	- 0003-Disallow-namespace-nodes-in-XPointer-ranges.patch

Team Nokogiri will keep on doing their best to provide security
updates in a timely manner, but if this is a concern for you and want
to use the system library instead; abort this installation process and
reinstall nokogiri as follows:

    gem install nokogiri -- --use-system-libraries
        [--with-xml2-config=/path/to/xml2-config]
        [--with-xslt-config=/path/to/xslt-config]

If you are using Bundler, tell it to use the option:

    bundle config build.nokogiri --use-system-libraries
    bundle install

Note, however, that nokogiri is not fully compatible with arbitrary
versions of libxml2 provided by OS/package vendors.
************************************************************************
Extracting libxml2-2.9.4.tar.gz into tmp/x86_64-apple-darwin16.4.0/ports/libxml2/2.9.4... OK
Running git apply with /usr/local/lib/ruby/gems/2.4.0/gems/nokogiri-1.8.0/patches/libxml2/0001-Fix-comparison-with-root-node-in-xmlXPathCmpNodes.patch... OK
Running git apply with /usr/local/lib/ruby/gems/2.4.0/gems/nokogiri-1.8.0/patches/libxml2/0002-Fix-XPointer-paths-beginning-with-range-to.patch... OK
Running git apply with /usr/local/lib/ruby/gems/2.4.0/gems/nokogiri-1.8.0/patches/libxml2/0003-Disallow-namespace-nodes-in-XPointer-ranges.patch... OK
Running 'configure' for libxml2 2.9.4... OK
Running 'compile' for libxml2 2.9.4... ERROR, review '/usr/local/lib/ruby/gems/2.4.0/gems/nokogiri-1.8.0/ext/nokogiri/tmp/x86_64-apple-darwin16.4.0/ports/libxml2/2.9.4/compile.log' to see what happened. Last lines are:
========================================================================
    unsigned short* in = (unsigned short*) inb;
                         ^~~~~~~~~~~~~~~~~~~~~
encoding.c:815:27: warning: cast from 'unsigned char *' to 'unsigned short *' increases required alignment from 1 to 2 [-Wcast-align]
    unsigned short* out = (unsigned short*) outb;
                          ^~~~~~~~~~~~~~~~~~~~~~
4 warnings generated.
  CC       error.lo
  CC       parserInternals.lo
  CC       parser.lo
  CC       tree.lo
  CC       hash.lo
  CC       list.lo
  CC       xmlIO.lo
xmlIO.c:1450:52: error: use of undeclared identifier 'LZMA_OK'
    ret =  (__libxml2_xzclose((xzFile) context) == LZMA_OK ) ? 0 : -1;
                                                   ^
1 error generated.
make[2]: *** [xmlIO.lo] Error 1
make[1]: *** [all-recursive] Error 1
make: *** [all] Error 2
========================================================================
*** extconf.rb failed ***
Could not create Makefile due to some reason, probably lack of necessary
libraries and/or headers.  Check the mkmf.log file for more details.  You may
need configuration options.

Provided configuration options:
	--with-opt-dir
	--with-opt-include
	--without-opt-include=${opt-dir}/include
	--with-opt-lib
	--without-opt-lib=${opt-dir}/lib
	--with-make-prog
	--without-make-prog
	--srcdir=.
	--curdir
	--ruby=/usr/local/Cellar/ruby/2.4.1_1/bin/$(RUBY_BASE_NAME)
	--help
	--clean
	--use-system-libraries
	--enable-static
	--disable-static
	--with-zlib-dir
	--without-zlib-dir
	--with-zlib-include
	--without-zlib-include=${zlib-dir}/include
	--with-zlib-lib
	--without-zlib-lib=${zlib-dir}/lib
	--enable-cross-build
	--disable-cross-build
/usr/local/lib/ruby/gems/2.4.0/gems/mini_portile2-2.2.0/lib/mini_portile2/mini_portile.rb:400:in `block in execute': Failed to complete compile task (RuntimeError)
	from /usr/local/lib/ruby/gems/2.4.0/gems/mini_portile2-2.2.0/lib/mini_portile2/mini_portile.rb:371:in `chdir'
	from /usr/local/lib/ruby/gems/2.4.0/gems/mini_portile2-2.2.0/lib/mini_portile2/mini_portile.rb:371:in `execute'
	from /usr/local/lib/ruby/gems/2.4.0/gems/mini_portile2-2.2.0/lib/mini_portile2/mini_portile.rb:114:in `compile'
	from /usr/local/lib/ruby/gems/2.4.0/gems/mini_portile2-2.2.0/lib/mini_portile2/mini_portile.rb:153:in `cook'
	from extconf.rb:365:in `block (2 levels) in process_recipe'
	from extconf.rb:257:in `block in chdir_for_build'
	from extconf.rb:256:in `chdir'
	from extconf.rb:256:in `chdir_for_build'
	from extconf.rb:364:in `block in process_recipe'
	from extconf.rb:262:in `tap'
	from extconf.rb:262:in `process_recipe'
	from extconf.rb:548:in `<main>'

To see why this extension failed to compile, please check the mkmf.log which can be found here:

  /usr/local/lib/ruby/gems/2.4.0/extensions/x86_64-darwin-16/2.4.0/nokogiri-1.8.0/mkmf.log

extconf failed, exit code 1

Gem files will remain installed in /usr/local/lib/ruby/gems/2.4.0/gems/nokogiri-1.8.0 for inspection.
Results logged to /usr/local/lib/ruby/gems/2.4.0/extensions/x86_64-darwin-16/2.4.0/nokogiri-1.8.0/gem_make.out

$ gem update --system
$ xcode-select --install
$ gem install nokogiri  # Success!

```

## トランスパイルとは
新しいJavaScriptの仕様で書かれたソースコードを現状のブラウザで使用できるように変換すること？
[Babel6でトランスパイル](http://christina04.hatenablog.com/entry/2016/02/24/224116)

## MovableTypeで // から始まるURL、スラッシュから始まる絶対パスを使う - HTTPSとHTTPを上手に使い分ける小技
`//`から始まるurlは，前のプロトコルを引き継いでそのurlにリクエストを送るようだ．

## LLVM
[コラム19: LLVMとClang](http://c-lang.sevendays-study.com/column-19.html)

## JITコンパイラ
[JITコンパイラ](https://ja.wikipedia.org/wiki/%E5%AE%9F%E8%A1%8C%E6%99%82%E3%82%B3%E3%83%B3%E3%83%91%E3%82%A4%E3%83%A9)
> 実行時コンパイラ（Just-In-Time Compiler、JITコンパイラ、その都度のコンパイラ）とは、ソフトウェアの実行時にコードのコンパイルを行い実行速度の向上を図るコンパイラのこと。通常のコンパイラはソースコード（あるいは中間コード）から対象CPUの機械語への変換を実行前に事前に行い、これをJITと対比して事前コンパイラ (Ahead-Of-Timeコンパイラ、AOTコンパイラ)と呼ぶ。
インタプリタ方式との違いは、インタプリタ方式がその都度コードを解釈しながら実行するのに対して、JIT方式は機械語に変換したものを実行することである。とはいえ、インタプリタ方式であっても究極的にはCPUが実行しているのは機械語である。そのため**インタプリタ方式とJIT方式の本質的な違いは機械語に変換する単位の大きさである**と言える。JIT方式ではモジュールやクラスといった比較的大きい単位で機械語に変換しているのに対し、インタプリタ方式では行ごと、ステートメントごとなどのごく小さい単位となる。 また、インタプリタ方式と同様に実行時にJava仮想マシンや共通言語ランタイムのようなランタイム環境を必要とする。
インタプリタ方式と比較すると性能面では以下のような差が出てくる
- 機械語に変換されるため、コンパイル後の実行速度はインタプリタ方式の数倍の性能となる
- モジュールやクラス、関数のロード時にコンパイルが行われるため、プログラムの起動には時間がかかる
- 一度コンパイルしたコードを保持するために、より多くのメモリ容量を必要とする



## covariance (共変) と invariance (不変) について (用語説明)
[Covariance, Invariance and Contravariance explained in plain English?](https://stackoverflow.com/questions/8481301/covariance-invariance-and-contravariance-explained-in-plain-english)


## lsの代わりにexaコマンド、progressコマンドでcpなどの進捗を表示

[Linuxメモ : 「exa」Rustで書かれたカラフルなls代替コマンドを試す](http://wonderwall.hatenablog.com/entry/2017/08/07/222350)
[Linuxメモ : progressでLinuxコマンド(cp, mv, dd, tar, cat…)の進捗を表示](http://wonderwall.hatenablog.com/entry/2017/08/04/073000)


## x11でec2インスタンスに接続

```sh
sudo yum -y install xauth
sudo yum -y install xterm
sudo yum -y install xorg-x11-apps
xeyes

sudo yum -y install pango pango-devel cairo glib2 redhat-lsb redhat-lsb-graphics libtiff libtiff-devel libjpeg-devel gcc
sudo yum -y install eog  # 画像ファイルを見るためのアプリケーション
sudo yum -y install firefox
firefox  # エラーが出る
```

- https://blog.masu-mi.me/2015/01/10/use_firefox.html
- [Xvfb を使って仮想ディスプレイを作る](http://blog.amedama.jp/entry/2016/01/03/115602)

```sh
# ssh server側
$ sudo yum -y install firefox
$ sudo yum -y install xorg-x11-xauth
$ cat /etc/ssh/sshd_config
X11Forwarding yes
X11UseLocalhost no
$ sudo systemctl restart sshd

# ssh cliant側
$ tail ~/.ssh/config
ForwardX11 yes
```

Andmore, I tried to xvfb, but fail.

```sh
$ sudo yum -y install xorg-x11-server-Xvfb
$ Xvfb :1 -screen 0 1024x768x24 &
$ export DISPLAY=:1
$ firefox
```

## markdownの改行に空行を使うな，それはパラグラフを分けるときに使うものだ

## uqmobileで使えるメーラーを作る

## 監視ツールのprometheusが気になる

## windowsのdisable hyper vについて

## pythonの可変長引数の変数名を取るには？

## awsのIAMロールを再作成する
[ユーザーアクセスキーを作成、修正、削除するには](http://docs.aws.amazon.com/ja_jp/IAM/latest/UserGuide/id_credentials_access-keys.html)

[【Tips】AWS CLIを使ってAmazon EC2を起動・停止するワンライナーまとめ](http://dev.classmethod.jp/cloud/aws/awscli-tips-ec2-start-stop/#link-2-1)

## docker関係

```sh
[fhiyo@ip-172-31-27-113 ~]$ ls -aZ /usr/bin/docker*
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-containerd
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-containerd-current
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-containerd-shim
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-containerd-shim-current
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-ctr-current
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-current
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/dockerd
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/dockerd-current
-rwxr-xr-x. root root system_u:object_r:container_runtime_exec_t:s0 /usr/bin/docker-storage-setup
[fhiyo@ip-172-31-27-113 ~]$ man docker
[fhiyo@ip-172-31-27-113 ~]$ man chcon
[fhiyo@ip-172-31-27-113 ~]$ sudo chcon -t docker_exec_t /usr/bin/docker*
[fhiyo@ip-172-31-27-113 ~]$ ls -aZ /usr/bin/docker*
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-containerd
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-containerd-current
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-containerd-shim
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-containerd-shim-current
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-ctr-current
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-current
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/dockerd
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/dockerd-current
-rwxr-xr-x. root root system_u:object_r:docker_exec_t:s0 /usr/bin/docker-storage-setup
```

```sh
[fhiyo@ip-172-31-27-113 ~]$ sudo systemctl stop docker
[fhiyo@ip-172-31-27-113 ~]$ sudo docker daemon -D -s devicemapper
```


## Homebrewでlua付きのvimをupgradeしようとするとエラーが出る
[Homebrew で Vim がインストールできない](https://ja.stackoverflow.com/questions/23350/homebrew-で-vim-がインストールできない)

Python.h not foundと言われる．自分はpyenvで環境構築をしているので，そのせいで上手くパスが読み込めずにエラーになることがある．
解決方法としては，上の記事の回答と同じだが，

1. .zshrcに設定しているpyenvの設定をコメントアウト
2. `source ~/.zshrc`でシェルの設定を再読込
3. `brew upgrade`を行いvimのupgradeを完了
4. .zshrcの設定を元に戻す

という流れで行った．

## pyenv使ってもjedi-vimで補完
[え？君せっかく Python のバージョン管理に pyenv 使ってるのに Vim の補完はシステムライブラリ参照してるの？](http://lambdalisue.hatenablog.com/entry/2014/05/21/065845)

## linuxのtimezoneを変更
[【設定確認から変更方法まで】Linuxでのタイムゾーンの扱い方](https://eng-entrance.com/linux-time-timezone)


## ec2のcentos/7でdocker + redmine環境を構築する
#### centos/7のdockerのdata shareがPermission deniedでできない
selinuxを切ったらいけた．でもあまりやりたくないから，他の手段も考えたい．．．暫定ではまあ切っとくけど．

## redmineでプラグインをインストールするときの手順
[Plugins installation on Redmine installed from packages](https://www.redmineup.com/pages/help/installation/how-to-install-redmine-plugins-from-packages)
`bundle exec rake redmine:plugins NAME=plugin's name RAILS_ENV=production`


## dbusとは?


## pythonのis not a packageエラーについて
ライブラリ内のパッケージと自分が作成していたスクリプトファイルの名前が競合していたために起こっていたエラーだった．
色々調べたところsys.pathの0, 1番目の要素を削除したら正常に読み込まれることを発見したので，そこから推測した．

## カバレッジ分析について



## phpのTraitについて，他の言語での同等の機能についてもまとめ
[Trait とは? その使い道を考えてみる](https://www.slideshare.net/tlync/trait)
[PHPのトレイトをいまさら使う](http://qiita.com/kazuhsat5/items/6ced7492daaddf1cd3d9)

MixinがRubyでは同等の機能っぽい．Pythonでもそうなのかな？
Pythonでtraitは別のライブラリに当っているようだ．

継承，委譲，多重継承のことが分かっていないと混乱する．
ちょうどいい機会なのでまとめておこう．記事分けてもいいし．


## Vulsというサーバーの脆弱性検査ツール
[Vulsを試してみよう](https://toe.bbtower.co.jp/20160623/645/)


## ブログをカスタムドメイン化したい
[カスタムドメインの GitHub Pages で HTTPS を使う](http://qiita.com/superbrothers/items/95e5723e9bd320094537)
[ブログをgithub pagesに移行した](http://blog.calcurio.com/pelican-github-pages.html)


## Jekyll+Github Pagesでブログを構築する
[参考にしているページ](http://morizyun.github.io/blog/jekyll-blog-github-page/)
[参考にしているページ2](http://melborne.github.io/2013/05/20/now-the-time-to-start-jekyll/)

Jekyllという性的ジェネレーターを使ってブログ作ってみました．
デプロイまでの方法についてメモしときます．

ページ内アンカー設定したいが，どうやってやるんだ？

自分のページがhttpsでCSP? (Content Security-Policy) errorになる．
(Your connection to this site is not fully secureと表示される)
[CSP error with Disqus on github pages](https://stackoverflow.com/questions/41530097/csp-error-with-disqus-on-github-pages)

後，自動でコメント欄が表示されない (Chromeで)．他のサイトではdisqusのコメント欄はこちらがポップアップを許可しなくても表示されるので，自分のサイト側の問題であろう．これを何とかしたい．
解決策か？: [The built-in Disqus now requires visitors to load unsafe scripts on https in order to comment. ](https://github.com/plusjade/jekyll-bootstrap/issues/306)


#### Boostrapカスタマイズ
- [【Boostrap入門】オシャレでレスポンシブなポートフォリオサイトの作り方](https://creive.me/archives/9316/)
- [Bootstrapをカスタマイズする上で必ず知っておきたい考え方](http://blog.yuhiisk.com/archive/2016/03/22/customize-the-css-of-bootstrap.html)



## バグ報告の環境説明はどこまで言えばいい？
他のプログラマがそのバグを再現できるまで，というのは当たり前だと思うのだが，
じゃあ実質どこまで言うのがよいのだろう？
OSのバージョンと使用しているソフトウェアのバージョンだけでよい？
ビルドバージョンは？
`uname -mrsv`の結果も欲しい？



### Jekyllについて
[Jekyll](https://jekyllrb.com/) 静的サイトジェネレータ．Github Pagesが

Jekyllはgemで管理されているパッケージ．以下のコマンドでインストールする．

```sh
gem install jekyll
```

- [Ruby on Rails チュートリアル](https://railstutorial.jp/chapters/beginning?version=5.0#cha-beginning)


- permalink: 強制的なパスの書き換え？
- Liquid: テンプレートエンジン.\{\% hoge \%\}や\{\{ hoge \}\}で囲うやつ．



## 独学でWeb系サイト作るために参考にするもの

[全くの初心者が独学でWeb制作できるようになるまでの効率の良い勉強の進め方](https://creive.me/archives/9157/)
上のリンクで紹介しているサイト

- ドットインストール（動画で学べるWebサービス）
- HTMLクイックリファレンス（制作リファレンスサイト）
- Codecademy（海外の初学者向けWebサービス）
- W3SCHOOL（英語の制作リファレンスサイト）
- UPDE（Web制作関連情報を発信するブログの記事を配信するサイト）


## Liquidのsyntex highlightをしようとすると変になる

![liquid-error](/assets/img/neta/liquid_error.png)
どうした．．．


## google chromeでアドレスバーでサーチしようとすると勝手に"www"と"com"がつく
例:
localhost:4000 → www.localhost.com:4000
のようになる．wwwとcomを消してもう一度アドレスバーでreturnを押すと今度は
入らない．一体何？？
再インストールしても直らなかった．なぜ？？
これ，どこに質問したらよいのだろう？stackoverflowにそれっぽいところあるのか？

## jekyllのサイトをデプロイしてgithub上でサイトを確認するとdropdownが上手く動作しない
解決方法: [Dropdown box appearing incorrectly on GitHub Pages built with Jekyll](https://stackoverflow.com/questions/43841922/dropdown-box-appearing-incorrectly-on-github-pages-built-with-jekyll)

> To solve your problem you need to copy style.css file from \_includes\css to assets\css and rename it to site.css (because you already have style.css in assets\css folder) then you have to add link to \_includes\head.html as below:
>
>     \{\% endcase \%\}
>     <link rel="stylesheet" href="{{ site.baseurl }}/assets/css/site.css">
>     <link rel="stylesheet" href="{{ site.baseurl }}/assets/css/style.css">
>
> <!-- Font Awesome -->


## 更にその他

- vimでカーソルの前にpasteするときは`P`を押すといける

[sslのクォリティ調査用サイト](https://www.ssllabs.com/ssltest/analyze.html?d=www.cslab.co.jp)

Jekyllでurlでアクセスしたときにディレクトリを返却する状態を直したい

pythonでdictのlistからdictの特定のkeyの値をループで回して取得したい
[Getting a list of specific index items from a list of dictionaries in python (list comprehension)](https://stackoverflow.com/questions/940442/getting-a-list-of-specific-index-items-from-a-list-of-dictionaries-in-python-li)

やり方:

```python
[item['key'] for item in list_]
```

- utf-8, utf-16, utf-32の違い
[扱う文字コードに迷ったらUTF-8を選ぼう](http://flat-leon.hatenablog.com/entry/select_utf_8)
[Wikipedia - 結合文字](https://ja.wikipedia.org/wiki/%E7%B5%90%E5%90%88%E6%96%87%E5%AD%97)

- kramdownではコードブロックにタイトルをつけることはできないのか？？

- macでgdbやるときはcertificationが必要 [gdb fails with “Unable to find Mach task port for process-id” error](https://stackoverflow.com/questions/11504377/gdb-fails-with-unable-to-find-mach-task-port-for-process-id-error)
[GDB Wiki - BuildingOnDarwin](http://sourceware.org/gdb/wiki/BuildingOnDarwin)

#### gdbの問題 6/25/15
http://ntraft.com/installing-gdb-on-os-x-mavericks/
↑
certificateの作り方など書いてある。
ここに書いてある通りプロセスをkillしたらgdbが動かない問題は解決？した。しかしまだwarningが出る。
打ったコマンド…
% ps -e | grep taskgated
8:   97 ??         0:23.23 /usr/libexec/taskgated -s
255:55917 ttys000    0:00.00 grep --color -n -I --exclude=*.svn-* --exclude=entries --exclude=*/cache/* taskgated
% sudo kill -9 97
% which gdb
/usr/local/bin/gdb
% codesign -s gdb-cert /usr/local/bin/gdb



- pythonのEllipsis, NotImplementedの定数について [3. Built-in Constants](https://docs.python.org/3/library/constants.html)

- kernelとuserlandについて
[What's the difference of the Userland vs the Kernel? \[duplicate\]](https://unix.stackexchange.com/questions/137820/whats-the-difference-of-the-userland-vs-the-kernel)
> userland is what the daemon does (or can do) when interacting with the operating systems ressouces (I/O, network, memory, cpu time). Those ressources are hidden from the process in the kernel space.


- centosでgdbを使うときは`debuginfo-install glibc libgcc libstdc++`をする必要がある [issing separate debuginfos, use: debuginfo-install glibc-2.12-1.47.el6_2.9.i686 libgcc-4.4.6-3.el6.i686 libstdc++-4.4.6-3.el6.i686](https://stackoverflow.com/questions/10389988/missing-separate-debuginfos-use-debuginfo-install-glibc-2-12-1-47-el6-2-9-i686)


