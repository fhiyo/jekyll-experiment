---
layout: post
title:  "karabiner-elementsで英数キー単体で入力のトグル切り替えが効かなくなった"
date:   2017-07-03 22:12:32 +0900
categories: mac
tags: karabiner-elements, sierra
---

## 自分の今の環境
```sh

```

## 何が起こったか

karabiner-elementsでcommand単体で押したら英数キーとして反応するように設定していたんだが，karabiner-elementsを0.91.5にアップデートしたら効かなくなってしまった．
設定は`~/.config/karabiner/karabiner.json`に以下のような設定を書いて動作させていた．

```json
{
    "profiles": [
        {
            "complex_modifications": {
                "parameters": {
                    "basic.to_if_alone_timeout_milliseconds": 1000
                },
                "rules": []
            },
            "devices": [],
            "name": "Default profile",
            "one_to_many_mappings": {},
            "selected": true,
            "simple_modifications": {
                "caps_lock": "left_control",
                "left_command": "left_command",
                "right_command": "right_command"
            },
            "standalone_keys": {
                "left_command": "japanese_eisuu",
                "right_command": "japanese_kana"
            },
            "virtual_hid_keyboard": {
                "caps_lock_delay_milliseconds": 0,
                "keyboard_type": "ansi",
                "standalone_keys_delay_milliseconds": 200
            }
        }
    ]
}
```

ここのstandalone_keysという設定で左右のコマンドキーが単体で押されたときの
挙動を英数/かなに変更できていたので快適に使えていたのだが,あるとき
karabiner-elementsをアップデートしたところいくら押しても入力ソースが切り替わらない
という問題が発生．日本語打つ人にとって入力ソースをスムーズに切り替えられないのは
死活問題．困った．

## 解決策
karabiner-elementsのリポジトリのissueに同様の報告が上げられていた．
[0.91.5にアップデートしたらstandalone_keysが効かなくなった #802](https://github.com/tekezo/Karabiner-Elements/issues/802)

Preferences→Complex Modifications→Add rule→Import more rules from the Internet (open a web browser)の順に遷移させて，[Karabiner-Elements complex_modifications rules](https://pqrs.org/osx/karabiner/complex_modifications/)のページを開く．
下の方にFor Japaneseというセクションがあるので，そこのImportのボタンを押して設定をインポートする．
するとCommand単体で押したときに英数/かなキーを送信する設定がダウンロードされるので，Enableのボタンを押して設定を有効にする．(左コマンドは英数，右コマンドはかなに対応している)

これで，今まで通り左右のコマンドキーで入力ソースの切り替えができるようになりました．
よかった．
