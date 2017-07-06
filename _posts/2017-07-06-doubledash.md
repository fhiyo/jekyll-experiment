---
layout: post
title:  "grepで-vを検索したいときはどうするか"
date:   2017-07-06 22:10:21 +0900
categories: bash
tags: shellcommand
---

```sh
$ grep -v foo
```
とすると，


## double dashの意味は引数の明示的な終わり 05/15/17
https://unix.stackexchange.com/questions/11376/what-does-double-dash-mean-also-known-as-bare-double-dash

> More precisely, a double dash (--) is used in bash built-in commands and many other commands to signify the end of command options, after which only positional parameters are accepted.
>
> Example use: lets say you want to grep a file for the string -v - normally -v will be considered the option to reverse the matching meaning (only show lines that do not match), but with -- you can grep for string -v like this:
>
> `grep -- -v file`


