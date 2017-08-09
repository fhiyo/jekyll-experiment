---
layout: post
title:  "投稿記事のネタ置き場"
date:   2017-07-03 22:12:32 +0900
categories: ネタ置き場
tags: hoge
comments: true
---
次に投稿できるようにネタだけ置いておく場所

## covariance (共変) と invariance (不変) について (用語説明)
[Covariance, Invariance and Contravariance explained in plain English?](https://stackoverflow.com/questions/8481301/covariance-invariance-and-contravariance-explained-in-plain-english)


## lsの代わりにexaコマンド、progressコマンドでcpなどの進捗を表示

[Linuxメモ : 「exa」Rustで書かれたカラフルなls代替コマンドを試す](http://wonderwall.hatenablog.com/entry/2017/08/07/222350)
[Linuxメモ : progressでLinuxコマンド(cp, mv, dd, tar, cat…)の進捗を表示](http://wonderwall.hatenablog.com/entry/2017/08/04/073000)

## treeコマンドで文字化けを防ぐ

```sh
tree -N .
# -N     Print non-printable characters as is instead of as escaped octal numbers.
```


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


## Zabbix監視ツール



## 更にその他

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

- centosでgdbを使うときは`debuginfo-install glibc libgcc libstdc++`をする必要がある [issing separate debuginfos, use: debuginfo-install glibc-2.12-1.47.el6_2.9.i686 libgcc-4.4.6-3.el6.i686 libstdc++-4.4.6-3.el6.i686](https://stackoverflow.com/questions/10389988/missing-separate-debuginfos-use-debuginfo-install-glibc-2-12-1-47-el6-2-9-i686)


