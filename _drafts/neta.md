---
layout: post
title:  "投稿記事のネタ置き場"
date:   2017-07-03 22:12:32 +0900
categories: ネタ置き場
tags: hoge
comments: true
---
次に投稿できるようにネタだけ置いておく場所

## markdownで注釈が使える

```
ほげほげ[^1]

本文本文

[^1]: ほげほげの注釈
```

みたいな感じで使える．知らなかった．．



## lispで`(func foo)`がOKのとき，`(func (foo))`がダメな理由

[ポーランド記法](https://ja.wikipedia.org/wiki/%E3%83%9D%E3%83%BC%E3%83%A9%E3%83%B3%E3%83%89%E8%A8%98%E6%B3%95)

> ^ LISPはこのような見た目だが、LISPで括弧が必須なのはLISPの理由による。すなわち、LISPでは式を読み込む時に式の木構造を構築するのだが、括弧なしで式の木構造を構築するには、演算子を識別し、その引数の数がわかっていなければならない。しかし、読み込み時にはその文字列から演算子かどうかはわからないし、任意個の引数をとる演算子もある。このため括弧なしには木構造を構築できないので、LISPでは括弧が必須なのである。

Lispは式の読み込みの際に木構造を構築して実行するから．  
`(func foo)`はfuncの子がfooであるのに対し，  
`(func (foo))`はfuncの子が(foo)であるため正常な実行ができない．



## bashでbrace展開がされない場合

```sh
#!/usr/bin/env bash

foo=xxxxxxxx

printf '%.s=' {1..8}
printf '%.s=' {1..${#foo}}
```

```sh
$ bash -x printf_test.sh
+ foo=xxxxxxxx
+ printf %.s= 1 2 3 4 5 6 7 8
========+ printf %.s= '{1..8}'
=%
```

~~後半がbrace展開されてない．Stackexchengeに聞くか．~~
↓  
[How can I use $variable in a shell brace expansion of a sequence?](https://unix.stackexchange.com/questions/7738/how-can-i-use-variable-in-a-shell-brace-expansion-of-a-sequence)にあった．${seq $foo $bar}を使えとのこと．

でも，bashのreadline上では問題なく展開されることについては言及されていなかったので聞きたいかも．

↑

自分が確認していた環境が違った．zshで確認していた．bashを起動して同じように`printf`したところ，やはりbrace expansionはされなかった．



## bashでpadding
[Print a character repeatedly in bash [duplicate]](https://stackoverflow.com/questions/5799303/print-a-character-repeatedly-in-bash)

> There's actually a one-liner that can do this:
>
>     printf "%0.s-" {1..10}
> prints
>
>     ----------
> Here's the breakdown of the arguments passed to printf:
>
> %s - This specifies a string of any length
> %0s - This specifies a string of zero length, but if the argument is longer it will print the whole thing
> %0.s - This is the same as above, but the period tells printf to truncate the string if it's longer than the specified length, which is zero
> {1..10} - This is a brace expansion that actually passes the arguments "1 2 3 4 5 6 7 8 9 10"
> "-" - This is an extra character provided to printf, it could be anything (for a "%" you must escape it with another "%" first, i.e. "%%")
> Lastly, The default behavior for printf if you give it more arguments than there are specified in the format string is to loop back to the beginning of the format string and run it again.


```sh
$ printf '%5.3s=' 1.23456 foobar
  1.2=  foo=%
```

`1.23456`の例はいらないかも．  
`%m.n`でm個padding, n文字表示という感じか？  
ブログに書くときには調べておく．



## `for i, v in enumerate(l)`で後でlの値を変えてもvの値は変わらない

当たり前だが，ハマってしまったのできちんと書いておく．

(`sys.stdin.readline()`よりも`input()`の方が楽だろうか？)



## 脆弱性関連でOSSに対してコミットした方法
[Ruby の脆弱性を見つけた話](http://techlife.cookpad.com/entry/2017/10/04/181946)  
手順が書かれている．過去の脆弱性を見てどこで修正が加えられているかを確認．



## python3のlistに対する掛け算はshallow copy?

```python3
>>> l_ = [[False] * 2] * 3
>>> l_
[[False, False], [False, False], [False, False]]
>>> l_[1][1] = True
>>> l_
[[False, True], [False, True], [False, True]]
```


## `local readonly foo=f`は多分間違いの構文

[Local and readonly variables in shells](https://gist.github.com/weakish/836753#file-wrong-sh)

`readonly`と`foo`の2つの変数をlocalスコープで宣言している．localスコープで書き換え禁止の変数を宣言したければ，

```sh
f() {
  local x
  readonly x='f'
}
```

のように分けて記述する．


## vim-easy-alignで正規表現が使える

gaで入った後ctrl + xで．
[vim-easy-align](https://github.com/junegunn/vim-easy-align)



## bash [ vs [[
[What's the difference between [ and [[ in Bash?](https://stackoverflow.com/questions/3427872/whats-the-difference-between-and-in-bash)


## ${arr[@]} vs ${arr} vs ${arr[\*]}

[A confusion about ${array[\*]} versus ${array[@]} in the context of a Bash completion](https://stackoverflow.com/questions/3348443/a-confusion-about-array-versus-array-in-the-context-of-a-bash-comple/)

↑  
`${arr[@]}`と`${arr[*]}`の違いについて解説している．


[Check if a Bash array contains a value](https://stackoverflow.com/questions/3685970/check-if-a-bash-array-contains-a-value)
>I just have to remember to pass the array as with quotes: "${array[@]}". Otherwise elements containing spaces will break functionality.



## シェルスクリプトのサイト
[UEC - usp engineers' community site](https://uec.usp-lab.com/TOP/CGI/TOP.CGI)



## 絶対に忘れてはいけない言葉
[高校生にWeb上でプログラミングを教え始めたエンジニアがこの8ヶ月間で得た気づき](https://qiita.com/sifue/items/7e7c7867b64ce9742aee?utm_source=Qiita%E3%83%8B%E3%83%A5%E3%83%BC%E3%82%B9&utm_campaign=d98df52469-Qiita_newsletter_273_09_20_2017&utm_medium=email&utm_term=0_e44feaa081-d98df52469-33372373)

>**FizzBuzzやフィボナッチ数列を異なるアルゴリズム1で3種類書いてくれと言われたときに、ホワイトボードにさらさらと書けるようになる。ちゃんとコードを書いてきたプログラマーであれば造作もないことだと思います。**



## プロジェクト管理の粒度は細かいほうがよい
[プロジェクトの残業を50%削減したタスク管理手法を惜しみなく公開する](https://qiita.com/0w0/items/0b287be30af6539ac5e9?utm_source=Qiita%E3%83%8B%E3%83%A5%E3%83%BC%E3%82%B9&utm_campaign=d98df52469-Qiita_newsletter_273_09_20_2017&utm_medium=email&utm_term=0_e44feaa081-d98df52469-33372373)  
個人のタスク管理では当てはまらないところもあるだろうが，自分の今の状況の問題と照らし合わせて解決できる部分もあるかもしれない



## unixコマンドツール色々よいもの揃えてる
[日常から使えるUnix系OS業務効率up技](https://qiita.com/onokatio/items/50fb616f71bf3c5021b9)



## 技術トレンドを調べるのに良い記事

[TECHNOLOGY RADAR VOL.16](https://www.thoughtworks.com/radar)

Slackのmohikanz #-generalより．
>lee [10:41 AM]
https://www.thoughtworks.com/radar
和田さん講演会で聞いた技術トレンド分析？的なもの
テクニック・ツール・プラットフォーム・言語&フレームワークに分けて
Adopt(採用でいい)/Trial(試してみ)/Assess(もっと調べて)/Hold(待て)で評価してる感じです。
Ansibleが2014に彗星のごとく現れて次の連載で即Adoptされてたのがすごい



## 配列の初期値を宣言する場合と後で代入する場合の違い

```c
char a[3] = {1, 2, 3};
```

と

```c
char a[3];
a[0] = 1;
a[1] = 2;
a[2] = 3;
```

は挙動が異なる．何が違うかというと，OS自作入門のp.86を参照．

アセンブリ言語に直してみる．  
前者がdb命令で直接メモリに値を書き込むのに対して，  
後者は`a[3]`分の領域を一度確保してからmov命令で1つずつ値を代入している．  
後者の方が無駄な命令に使うメモリが大きくなる．




## ADS (代替ストリーム) について
[代替データストリーム（ADS）について色々調べてみた](https://qiita.com/minr/items/c2393f532b2df35f7a9d)



## pythonのUnitTestによる大量のテストの書き方
[Python の 単体テストで 大量の入力パターンを効率よくテストする方法](https://qiita.com/Asayu123/items/61ef72bb829dd8baba9f)  
> Subtestを用いると、途中で失敗するパターンがあっても、テストを最後まで実行し、成否をパターン毎に個別に出力してくれるようになります。
> これは、Parameterized Testと呼ばれる手法の１つで、 **Python Parameterized test** などで検索すると、他のテストランナーでの手法も見つかります。



## putenv()とsetenv()について
[putenv() and setenv()](https://www.greenend.org.uk/rjk/tech/putenv.html)

メモリリークの危険がある話だが，結局何なのかはよくわからん．．


## source commandは現在のシェルで実行される

なので，シェルの環境変数を変更することができる．

```sh
$ cat test.sh
myVariable=foo
$ unset myVariable; source test.sh; echo ${myVariable:-null}
foo
$ unset myVariable; bash -s < test.sh; echo ${myVariable:-null}
null
```
[Using source vs bash commands](https://askubuntu.com/questions/306523/using-source-vs-bash-commands)


## bash scriptがいる場所を取得する

[The Right Way to Get the Directory of a bash Script](http://www.ostricher.com/2014/10/the-right-way-to-get-the-directory-of-a-bash-script/)

3つ目の例は"[[ hoge ]]"を使ってるからbash専用になるが，これはできればやめたいなぁ．．  
2つ目は"`DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"`"は例のやつなので覚えておく．

↑これに関連して，  

`pwd -L`と`pwd -P`の違いを見ておきたい．`readlink`してsymbolic linkを解決する際に必要なテクニックなはず．

python使って，`realpath="python -c 'import os, sys; print os.path.realpath(sys.argv[1])'"`でもいいし，  
perl使って，`perl -MCwd -e 'print Cwd::abs_path shift' ~/non-absolute/file`

でもよさそうだ．


## gitコマンド

[torvalds/linux V3s #462](https://github.com/torvalds/linux/pull/462)

```sh
git send-email
git format-patch
```


## 怠惰な Linux: 管理者に必須の 10 の秘訣

[怠惰な Linux: 管理者に必須の 10 の秘訣](https://www.ibm.com/developerworks/jp/linux/library/l-10sysadtips/index.html)



## KeyCast  --  Record key stroke for screencast

[KeyCast](https://github.com/cho45/KeyCast)


## goの書き方、環境の整え方

[How to Write Go Code](https://golang.org/doc/code.html)

[goのrepl - gore](http://motemen.hatenablog.com/entry/2015/02/go-repl-gore)



## cygwinでXを起動

参考: http://qiita.com/mansonsp/items/1c52668b2f46002a754c

### 環境構築
1. cygwinのsetup-x86_64.exeを起動して、以下のコマンドをインストールする。
- xorg-server
- xinit
- openssh

2. C:\Users\username\AppData\Roaming\Microsoft\Windows\Start
Menu\Programs\Cygwin-X
のXWin Serverのプロパティを開き、リンク先を書き換える。
変更前:
C:\cygwin64\bin\run.exe --quote /usr/bin/bash.exe -l -c "cd; exec /usr/bin/startxwin"
変更後:
C:\cygwin64\bin\run.exe --quote /usr/bin/bash.exe -l -c "cd; /usr/bin/Xwin.exe :0 -multiwindow -listen tcp"

3. ~/.bashrcに以下の記述を追加する。

```sh
if [ -z "{$DISPLAY}" ]; then
export DISPLAY='localhost:0.0
fi
```

4. 以下のコマンドを実行する。

$ source ~/.bashrc

### 接続方法
1. C:\Users\username\AppData\Roaming\Microsoft\Windows\Start
Menu\Programs\Cygwin-X\XWin Server.lnk
を実行する。
2. 以下のコマンドを実行する。
$ ssh -Y hostname@servername
で接続する。以下のコマンドでX Windowが正常に動作しているかを確認する。
$ xeyes



## goでwebスクレイピングを作る、を見て真似するだけ

```sh
# go1.9のインストール
$ curl -LO https://storage.googleapis.com/golang/go1.9.linux-amd64.tar.gz
$ sudo yum install -y perl-Digest-SHA
$ shasum -a 256 go1.9*.tar.gz
d70eadefce8e160638a9a6db97f7192d8463069ab33138893ad3bf31b0650a79  go1.9.linux-amd64.tar.gz
# このハッシュ値と`https://golang.org/dl`に書いてあるハッシュ値が一致していればOK
$ sudo tar -C /usr/local -xvzf go1.9.linux-amd64.tar.gz
$ vim ~/.bashrc
# export PATH=$PATH:/usr/local/go/bin の文の付け加える
$ source ~/.bashrc
$ go get -d 'github.com/PuerkitoBio/goquery'  # 依存ライブラリのインストール
$ go run main.go
```


## [mac]Option-Shift-Command-Vでstyleを貼り付け先に合わせてペーストする

[Take Control of Your Mac's Clipboard](https://computers.tutsplus.com/tutorials/take-control-of-your-macs-clipboard--mac-30472)

リンク先にも書いてあるが、指がつる。styleを保持したままペーストする方が珍しい気がするので、ショートカットを変更した方がいいかも。



## luaについてまとめ

[Patterns Tutorial](http://lua-users.org/wiki/PatternsTutorial)
> Limitations of Lua patterns
>
> Especially if you're used to other languages with regular expressions, you might expect to be able to do stuff like this:
>
> '(foo)+' -- match the string "foo" repeated one or more times
> '(foo|bar)' -- match either the string "foo" or the string "bar"
> Unfortunately Lua patterns do not support this, only single characters can be repeated or chosen between, not sub-patterns or strings. The solution is to either use multiple patterns and write some custom logic, use a regular expression library like lrexlib or Lua PCRE, or use LPeg [3]. LPeg is a powerful text parsing library for Lua based on Parsing Expression Grammar [4]. It offers functions to create and combine patterns in Lua code, and also a language somewhat like Lua patterns or regular expressions to conveniently create small parsers.

[Lpeg Tutorial](http://lua-users.org/wiki/LpegTutorial)
> f'x' is equivalent to f('x') in Lua; using single quotes has the same meaning as double quotes


## 標準出力と標準エラー出力の入れ替え

I try to swap stdout and stderr.  
By using `{}`, I successed to gain output what I want. But I do know why not using `{}` version's output is not swapped.

```sh
$ { echo stdout; echo stderr 1>&2; } 3>&1 1>&2 2>&3 1>1.txt 2>2.txt;
$ cat 1.txt
stdout
$ cat 2.txt
stderr

$ { { echo stdout; echo stderr 1>&2; } 3>&1 1>&2 2>&3; } 1>1.txt 2>2.txt;
$ cat 1.txt
stderr
$ cat 2.txt
stdout
```

なぜ`{}`のありなしで結果に違いが出る？？理由がわからない．



[DUP](http://man7.org/linux/man-pages/man2/dup.2.html)  
DUPについてのmanページ．



## 09/08/17の気になった内容
#### leakyreluとは？reluとなんか違う？
[What is the difference between LeakyReLU and PReLU?](https://datascience.stackexchange.com/questions/18583/what-is-the-difference-between-leakyrelu-and-prelu)  
> Leaky ReLUs allow a small, non-zero gradient when the unit is not active
Parametric ReLUs take this idea further by making the coefficient of leakage into a parameter that is learned along with the other neural network parameters

#### SpatialFullConvolution vs SpatialConvolution
[SpatialFullConvolution vs SpatialConvolution](https://groups.google.com/forum/#!topic/torch7/upg9CV05EBk)

SpatialFullConvolutionは自己符号化のときに使われるようだ。。入力のサイズを下げないということ？  
SpatialFullConvolutionはupsampling, SpatialConvolutionはdownsamplingだと言っている。

#### wheelとanacondaの関係について
[wheelのありがたさとAnacondaへの要望](http://ymotongpoo.hatenablog.com/entry/2017/02/02/182647)



## 気になるブログメモ

- [linuxと暮らす](http://linux.just4fun.biz/): linux, windowsから暗号通貨まで様々なコンテンツについて情報をメモしているサイト。


## busted  --  Lua unit testing tool.

[Olivine-Labs/busted](https://github.com/Olivine-Labs/busted)


## mutexとは

[「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典イメージぴよ画像「分かりそう」で「分からない」でも「分かった」気になれるIT用語辞典  -- mutex](http://wa3.i-3-i.info/word13360.html)
いつも忘れる

[Difference between binary semaphore and mutex](https://stackoverflow.com/questions/62814/difference-between-binary-semaphore-and-mutex)
> the problem is with the statement that says "A mutex is really a semaphore with value 1" but that is not the case. ThreadA and only ThreadA can increment (and hence release) the mutex that it decremented whereas ThreadB can increment the binSemaphore decremented by ThreadA, which also happens to be the answer to the question in question.

binary semaphoreとmutexの違いは何なのか．上の引用では，binary semaphoreはsemphoreを取ったthread以外のthreadでもsemaphoreの値を戻すことができるが，mutexはmutexを取ったthreadしか元に戻せない．

Toilet exampleは間違っているようだ．わかりやすい例えだと思ったが，本質が違うらしい．

[Mutex vs. Semaphores – Part 2: The Mutex](https://blog.feabhas.com/2009/09/mutex-vs-semaphores-%E2%80%93-part-2-the-mutex/)
> The concept of ownership enables mutex implementations to address the problems discussed in part 1:
>
> Accidental release
> Recursive deadlock
> Task-Death deadlock
> Priority inversion
> Semaphore as a signal

> Mutual Exclusion / Synchronisation
Due to ownership a mutex cannot be used for synchronization due to lock/unlock pairing. This makes the code cleaner by not confusing the issues of mutual exclusion with synchronization.

どうもsemaphoreは同期を取るために使うらしい．一方mutexは排他制御．自分以外は俺が解放するまでこのリソースに触るな，といっている (気がする)．  
semaphoreはタスクAがsemaphoreを取ってpending状態になり，タスクBがsemaphoreを与えてAのpending状態から次に進むよう制御する，という感じか？？


3つ以上のsemaphoreで同期を取るやり方がよくわからん．

semaphoreの使い所でググればいいのか？

[ファイヤープロジェクト セマフォ](http://www.fireproject.jp/feature/c-language/ipc/semaphore.html)



## mohikanのapps-infraチャンネル
> lsyncd+unisonについて (サーバー・クライアント間同期)

> vultrも結構便利。ローカルネットワークとかセキュリティグループとかあって、簡易クラウド的な使い方が出来る。
vultrとconohaは価格的にも機能的にも大体同じくらいだな (edited)
イメージ的には、日本regionがあるdigitaloceanみたいなかんじ

[ほほほのほ acme-client](https://www.seirios.org/seirios/dokuwiki/doku.php?id=networkapp:acme-client#acme-client)  
↑mohikanのserios氏のブログ

> `last`コマンドについて

> `systemd-analyze blame`

## bashコマンド知らなかったやつ
- `mkfifo`
- `exec` (builtin) [How to redirect an output file descriptor of a subshell to an input file descriptor in the parent shell?](https://stackoverflow.com/questions/15153158/how-to-redirect-an-output-file-descriptor-of-a-subshell-to-an-input-file-descrip)

```sh
$ exec 3>&1  # 何してる？
```

`info bash`の3.6.10
3.6.10 Opening File Descriptors for Reading and Writing

The redirection operator

[n]<>word
causes the file whose name is the expansion of word to be opened for both reading and writing on file descriptor n, or on file descriptor 0 if n is not specified. If the file does not exist, it is created.

`<>`は`mkfifo`と似たやつなのだろうか？？

```sh
$ cat myfile.txt
line1
line2
line3
line4
$ cat <&3
line1
line2
line3
line4
$ cat <&3
$ cat <&3
$ echo -e "line1\nline2\nline3\nline4" >&3
$ cat <&3
$ cat <&3
$ rm myfile.txt
$ echo -e "line1\nline2\nline3\nline4" > myfile.txt
$ exec 3<> myfile.txt
$ echo -e "LINE1\nLINE2" >&3
$ cat <&3
line3
line4
```

```sh
$ cat foo.txt
line1
line2
line3
line4
$ echo -e "LINE1\nLINE2" 1<>foo.txt
$ cat foo.txt
LINE1
LINE2
line3
line4
```
面白い．
[How does cat \<\> file work?  Answer](https://unix.stackexchange.com/questions/164391/how-does-cat-file-work/164449#164449)



## git credential helperを設定してrecursiveなリポジトリにいちいちパスを聞かれないようにする

```sh
git config credential.helper 'cache --timeout=300'
```

gitconfigファイルの書き方も調べて記述する


## サーバー環境でもgit diffを見やすくしたい -- git difftool

[git difftool --dir-diff が便利すぎて泣きそうです](http://tech.nitoyon.com/ja/blog/2013/07/02/git-dir-diff/)

`tig`使えばいいじゃん、という感じではあるが、difftoolならgit入れたときに一緒に入ってくるやつだから余計なインストールしなくて済む。
上の記事見て、もうちょい使い方を勉強したほうがいいかも。


## grepってcache使ってる？

```sh
$ du -ms
30108	.
$ time grep -r foo * 1>/dev/null

real	1m30.188s
user	0m3.571s
sys	0m4.854s
$ time grep -r foo * 1>/dev/null

real	0m3.661s
user	0m2.331s
sys	0m1.327s
$ sudo bash -c "sync; echo 3 > /proc/sys/vm/drop_caches"
$ time grep -r foo * 1>/dev/null

real	1m30.504s
user	0m3.573s
sys	0m5.035s
```

[Does grep use a cache to speed up the searches?](https://unix.stackexchange.com/questions/8914/does-grep-use-a-cache-to-speed-up-the-searches)
↑  
同じこと考えてる人いた。grepがcache使ってるんじゃなくて、filesystem側が操作されたディレクトリをon memoryにしており、grepはまずmemory側を見るので
file I/O分 (？) 速くなるようだ。

実行時間計測してみる

[Documentation for /proc/sys/vm/\* kernel version 2.6.29](https://www.kernel.org/doc/Documentation/sysctl/vm.txt)

drop_cachesってなんだ．．
```txt
drop_caches

Writing to this will cause the kernel to drop clean caches, as well as
reclaimable slab objects like dentries and inodes.  Once dropped, their
memory becomes free.

To free pagecache:
	echo 1 > /proc/sys/vm/drop_caches
To free reclaimable slab objects (includes dentries and inodes):
	echo 2 > /proc/sys/vm/drop_caches
To free slab objects and pagecache:
	echo 3 > /proc/sys/vm/drop_caches

This is a non-destructive operation and will not free any dirty objects.
To increase the number of objects freed by this operation, the user may run
`sync' prior to writing to /proc/sys/vm/drop_caches.  This will minimize the
number of dirty objects on the system and create more candidates to be
dropped.

This file is not a means to control the growth of the various kernel caches
(inodes, dentries, pagecache, etc...)  These objects are automatically
reclaimed by the kernel when memory is needed elsewhere on the system.

Use of this file can cause performance problems.  Since it discards cached
objects, it may cost a significant amount of I/O and CPU to recreate the
dropped objects, especially if they were under heavy use.  Because of this,
use outside of a testing or debugging environment is not recommended.

You may see informational messages in your kernel log when this file is
used:

	cat (1234): drop_caches: 3

These are informational only.  They do not mean that anything is wrong
with your system.  To disable them, echo 4 (bit 3) into drop_caches.
```

[How do i view / enable kernel logs on centos / red hat linux?](https://serverfault.com/questions/308897/how-do-i-view-enable-kernel-logs-on-centos-red-hat-linux?newreg=e49de168cccf468d97fe423cf2a9dd69)  
↑  
kernel logを見ようと思ったけどcentos/7でどこ見ればいいかわからなかったのでメモ．


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



## torch image install

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
$ luarocks install argparse  # 0.5.0-1  command line argument parser
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

```
   Special Parameters
       The shell treats several parameters specially.  These parameters  may
       only be referenced; assignment to them is not allowed.
       *      Expands to the positional parameters, starting from one.  When
              the expansion occurs within double quotes,  it  expands  to  a
              single  word with the value of each parameter separated by the
              first character of the IFS special variable.  That is, "$*" is
              equivalent  to  "$1c$2c...", where c is the first character of
              the value of the IFS variable.  If IFS is unset,  the  parame‐
              ters  are separated by spaces.  If IFS is null, the parameters
              are joined without intervening separators.
       @      Expands to the positional parameters, starting from one.  When
              the  expansion  occurs  within  double  quotes, each parameter
              expands to a separate word.  That is, "$@"  is  equivalent  to
              "$1"  "$2" ...  If the double-quoted expansion occurs within a
              word, the expansion of the first parameter is joined with  the
              beginning  part of the original word, and the expansion of the
              last parameter is joined with the last part  of  the  original
              word.   When  there  are no positional parameters, "$@" and $@
              expand to nothing (i.e., they are removed).
       #      Expands to the number of positional parameters in decimal.
       ?      Expands to the exit status of the most recently executed fore‐
              ground pipeline.
       -      Expands  to the current option flags as specified upon invoca‐
              tion, by the set builtin command, or those set  by  the  shell
              itself (such as the -i option).
       $      Expands  to the process ID of the shell.  In a () subshell, it
              expands to the process ID of the current shell, not  the  sub‐
              shell.
       !      Expands  to the process ID of the most recently executed back‐
              ground (asynchronous) command.
       0      Expands to the name of the shell or shell script.  This is set
              at  shell  initialization.   If bash is invoked with a file of
              commands, $0 is set to the name of  that  file.   If  bash  is
              started  with the -c option, then $0 is set to the first argu‐
              ment after the string to be executed, if one is present.  Oth‐
              erwise,  it  is  set  to the file name used to invoke bash, as
              given by argument zero.
       _      At shell startup, set to the absolute pathname used to  invoke
              the  shell  or  shell  script  being executed as passed in the
              environment or argument list.  Subsequently,  expands  to  the
              last  argument to the previous command, after expansion.  Also
              set to the full pathname used to invoke each command  executed
              and  placed in the environment exported to that command.  When
              checking mail, this parameter holds the name of the mail  file
              currently being checked.
```


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


## linux cui コマンド
lsの代わりにexaコマンド、progressコマンドでcpなどの進捗を表示

[Linuxメモ : 「exa」Rustで書かれたカラフルなls代替コマンドを試す](http://wonderwall.hatenablog.com/entry/2017/08/07/222350)
[Linuxメモ : progressでLinuxコマンド(cp, mv, dd, tar, cat…)の進捗を表示](http://wonderwall.hatenablog.com/entry/2017/08/04/073000)

pexpect: [Pexpect version 4.2](https://pexpect.readthedocs.io/en/stable/)  
expectコマンドの代わりの使いやすいやつ？


## x11でec2インスタンスに接続

```sh
$ sudo yum -y install xauth
$ sudo yum -y install xterm
$ sudo yum -y install xorg-x11-apps
$ xeyes

$ sudo yum -y install pango pango-devel cairo glib2 redhat-lsb redhat-lsb-graphics libtiff libtiff-devel libjpeg-devel gcc
$ sudo yum -y install eog  # 画像ファイルを見るためのアプリケーション
$ sudo yum -y install firefox
$ firefox  # エラーが出る
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


## upstreamのいう名前でfork元をaddしておけば追従できる
で，普通にfetch & mergeすればOK.



- pythonのEllipsis, NotImplementedの定数について [3. Built-in Constants](https://docs.python.org/3/library/constants.html)

- kernelとuserlandについて
[What\'s the difference of the Userland vs the Kernel? \[duplicate\]](https://unix.stackexchange.com/questions/137820/whats-the-difference-of-the-userland-vs-the-kernel)
> userland is what the daemon does (or can do) when interacting with the operating systems ressouces (I/O, network, memory, cpu time). Those ressources are hidden from the process in the kernel space.

- centosでgdbを使うときは`debuginfo-install glibc libgcc libstdc++`をする必要がある [issing separate debuginfos, use: debuginfo-install glibc-2.12-1.47.el6_2.9.i686 libgcc-4.4.6-3.el6.i686 libstdc++-4.4.6-3.el6.i686](https://stackoverflow.com/questions/10389988/missing-separate-debuginfos-use-debuginfo-install-glibc-2-12-1-47-el6-2-9-i686)

- [go fundme](https://www.gofundme.com/)  個人への寄付をするサイト
- [valu](https://valu.is/)  個人へ株式投資のようにお金を投資するサイト
- [Wifi Widget - See, Test, and Share Wi-Fi](https://itunes.apple.com/us/app/wifi-widget-see-test-and-share-wi-fi/id1192965614?mt=8)  wifi管理の自動化？iPhoneアプリ
- [オレオレ証明書を使いたがる人を例を用いて説得する](https://qiita.com/Sheile/items/dc91128e8918fc823562)
- [レビューしてもらいやすいPRの書き方](http://in.fablic.co.jp/entry/2017/10/05/090000) → [How to write the perfect pull request](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)
