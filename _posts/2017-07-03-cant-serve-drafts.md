---
layout: post
title:  "Jekyllで記事を\__draftsに置いとこうとしたら詰まった"
date:   2017-07-03 22:12:32 +0900
categories: ruby
tags: jekyll, bundle
---

<a href="# 概要"><h2>概要</h2></a>

[Jekyll](https://jekyllrb.com/)は`_drafts`というディレクトリの中に公開する前の
記事を置いておくことができます．しかしそこで記事を書いてローカルで動かしてみたところ，
エラーが出て動かなかったのでメモ．

### 環境

```sh
$ ruby -v
ruby 2.3.1p112 (2016-04-26 revision 54768) [x86_64-darwin15]
$ jekyll -v
jekyll 3.5.0
```

### エラー内容とやったこと

```sh
$ jekyll serve --drafts
WARN: Unresolved specs during Gem::Specification.reset:
      listen (< 3.1, ~> 3.0)
WARN: Clearing out unresolved specs.
Please report a bug if this causes problems.
Configuration file: /Users/fhiyo/Archive/fhiyo.github.io/_config.yml
            Source: .
       Destination: ./_site
 Incremental build: disabled. Enable with --incremental
      Generating...
  Populating LSI...
Rebuilding index...
jekyll 3.5.0 | Error:  Zero vectors can not be normalized
```

とりあえずエラー文でググった．
[Zero vectors can not be normalized](https://github.com/jekyll/classifier-reborn/issues/64)  
classifier-rebornのリポジトリのissueに同様の症状が書いてある．
何かstop wordだけで入力の文字列が構成されていると同じエラーがでるようだ．

記事に適当に何か書いて再度`jekyll serve --drafts`を実行．しかし以下のエラーが出る．

```sh
% jekyll serve --drafts
WARN: Unresolved specs during Gem::Specification.reset:
      listen (< 3.1, ~> 3.0)
WARN: Clearing out unresolved specs.
Please report a bug if this causes problems.
Configuration file: /Users/fhiyo/Archive/fhiyo.github.io/_config.yml
            Source: .
       Destination: ./_site
 Incremental build: disabled. Enable with --incremental
      Generating...
  Populating LSI...
Rebuilding index...
jekyll 3.5.0 | Error:  comparison of Float with NaN failed
```

何かさっきから出ているwarningが関係してるのか？

```sh
bundle clean --force
```

でwarningは消え，正常に実行された．
参考: [Guard and Unresolved specs](https://blog.mikecordell.com/2014/01/26/guard-and-unresolved-specs.html)

なぜ`bundle clean --force`でwarningが消えたのか？
ヘルプを見てみる．

```sh
$ bundle clean -h
Usage:
  bundle clean [OPTIONS]

Options:
      [--dry-run=Only print out changes, do not clean gems], [--no-dry-run]
      [--force=Forces clean even if --path is not set]
      [--no-color], [--no-no-color]                                          # Disable colorization in output
  -r, [--retry=NUM]                                                          # Specify the number of times you wish to attempt network commands
  -V, [--verbose], [--no-verbose]                                            # Enable verbose output mode

Cleans up unused gems in your bundler directory
```
まず，`bundle clean`は使用していないgemのパッケージを消し去ってくれるコマンドのようだ．
システム上のgemパッケージを消し去ってしまうのは危険な行為なので，`--force`をつけないと
実行できないようになっている．これで古いパッケージを参照することがなくなったため，
warningがでなくなったということか．

しかし，なぜこれで上手くいったのか結局よくわからないな．．．