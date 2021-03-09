---
title: "Vimにeskkをインストールする"
date: 2021-01-23T17:10:07+09:00
categories: ["vim"]
tags: ["vim", "skk"]
---

## はじめに
Vim上での日本語入力の手段として[eskk.vim](https://github.com/tyru/eskk.vim)を導入しましたので自分用のメモも兼ねて残しておきます。

SKK・eskk.vim共にこれまで使ったことがないので、参考程度に見てください。

## インストール
### eskk.vim
https://github.com/tyru/eskk.vim

好みのプラグインマネージャでeskkを導入します。

### 辞書
skkを使うには読みとの対応が列挙された辞書が必要になるので、ダウンロードして、eskkに教えてあげる必要があります。

辞書ファイルはこちら↓などで公開されている物を利用させていただくと良いでしょう。

https://skk-dev.github.io/dict/

自分でファイルをダウンロードして配置するのも良いですが、それも面倒ですので、自動でダウンロードするスクリプトを用意しました。

以下のスクリプトは辞書ファイルを ` ~/.config/eskk `  下にダウンロード・配置します。

こちらは、~/.config/eskk 配下に辞書が無いときのみ発動するので、`.vimrc`にでも書き込んでおけば幸せになれます。

``` vim
if !filereadable(expand('~/.config/eskk/jisyo'))
    call system('mkdir -p ~/.config/eskk')
    call system('cd ~/.config/eskk/ && wget http://openlab.jp/skk/dic/SKK-JISYO.L.gz && gzip -d SKK-JISYO.L.gz && nkf -Ew SKK-JISYO.L > jisyo && rm -f SKK-JISYO.L')
endif
```

また、↓を`.vimrc`に書き込んでおけばダウンロードしてきたファイルがeskkに読まれます。

``` vim

let g:eskk#directory = "~/.config/eskk"
let g:eskk#dictionary = { 'path': "~/.config/eskk/my_jisyo", 'sorted': 1, 'encoding': 'utf-8',}
let g:eskk#large_dictionary = {'path': "~/.config/eskk/jisyo", 'sorted': 1, 'encoding': 'utf-8',}

```

## skkの使い方
以上の設定を終えた後、Insertモードで<C-j>をタイプすることでeskkが起動するはずです。

この状態ではローマ字しか打てないのですが、漢字で打ちたい単語の初めの文字を大文字で打つことで、▽の文字が入りスペースキーで変換をすることが出来ます。

また、それぞれのキーを押下することで、平仮名・片仮名・英数を入力できます。

{{<figure src="./eskk.png" alt="モード" width="75%">}}

また、送り仮名の付いた漢字は、

> 送り仮名  
> ↓  
> OkuRiGana

といった形で入力できます。

### StickyShift
skkの入力では漢字を入力するたびにShiftキーを押すのですが、それでは小指が壊れてしまいます。

ですので、eskkにはStickyShiftという便利な仕組みが存在します。

この時、Shiftキーの代わりに";"キーを利用することができます。  
例えば、

> 漢字  
> ↓  
> ;kanji

のように入力できます。

この動作を変更し、StickyShiftを無効にするなどの設定は↓のように行うことができます。
以下の例では、SrickyShiftのキーとして;を無効にし、Qを新たに割り当てています。
```vim
autocmd User eskk-initialize-post call s:eskk_initial_pre()
function! s:eskk_initial_pre() abort
    EskkUnmap -type=sticky ;
    EskkMap -type=sticky Q
endfunction
```
を.vimrcに書き込んでおくと良いです。(多分eskk-initialize-postでやらないと動かない)
> [参考](https://github.com/tyru/eskk.vim/blob/master/doc/eskk.jax)

## StatusLineに変換モードを表示

`g:eskk#statusline()` 関数を使うと現在の変換モードを取得できるのですが、どうもLightline等に関数を設定するとVimの起動がめちゃ遅くなります。

そこで、新たに↓のような関数を定義しておくと良いでしょう。

```vim
function L_eskk_get_mode()
    if (mode() == 'i') && eskk#is_enabled()
        return g:eskk#statusline()
    else
        return ''
    endif
endfunction
```

私はもう少しシンプルに表示したいので、↓のような設定に落ち着きました。

```vim
function L_eskk_get_mode()
    if (mode() == 'i') && eskk#is_enabled()
        return g:eskk#statusline_mode_strings[eskk#get_mode()]
    else
        return ''
    endif
endfunction

let g:lightline = {
\   'active': {
\     'left': [ ['mode', 'paste'], ['readonly', 'filename', 'eskk', 'modified'] ]
\   },
\   'component_function': {
\     'eskk': 'L_eskk_get_mode'
\   },
\ }
```

おわり
