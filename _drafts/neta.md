---
layout: post
title:  "投稿記事のネタ置き場"
date:   2017-07-03 22:12:32 +0900
categories: ネタ置き場
tags: hoge
comments: true
---
次に投稿できるようにネタだけ置いておく場所

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
- Liquid: テンプレートエンジン.\{% hoge %\}や\{\{ hoge \}\}で囲うやつ．




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

[sslのクォリティ調査用サイト](https://www.ssllabs.com/ssltest/analyze.html?d=www.cslab.co.jp)

Jekyllでurlでアクセスしたときにディレクトリを返却する状態を直したい

pythonでdictのlistからdictの特定のkeyの値をループで回して取得したい
[Getting a list of specific index items from a list of dictionaries in python (list comprehension)](https://stackoverflow.com/questions/940442/getting-a-list-of-specific-index-items-from-a-list-of-dictionaries-in-python-li)

やり方:

```python
[item['key'] for item in list_]
```



## エラー対策

```sh
jekyll 3.5.0 | Error:  Zero vectors can not be normalized
```

が起きてしまうので対策として一番下に適当に何か書いておく．

↑
何も書いてないからこれが起きるというわけではない？
related-postsが生成してるページの部分に内容が入っていないからこれが出るのかもしれない．

