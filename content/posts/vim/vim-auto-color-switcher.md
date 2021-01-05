---
title: "OSのダークモードにVimのcolorschemeを追従させるプラグイン"
date: 2021-01-05
categories: ["vim"]
tags: ["vim", "mac"]
---

この記事は[VimAdventCalendar2020](https://qiita.com/advent-calendar/2020/vim)、20日目の記事です。  
19日目はyutakatayさんの[超融合!時空を越えた絆 Neo Vim(VSCode)を試してみた](https://zenn.dev/yutakatay/articles/vscode-neovim)でした。  

## はじめに
katoと申します。（[twitter](https://twitter.com/uvrub)）そこらへんのこーこーせーです()  
[スカウター](https://github.com/thinca/vim-scouter)で計測した[vimrc]()の戦闘力は93ですのでたぶんよわいvimmerです。  
この記事ではmacOS/Windows(多分)上で動く、OSのダークモードに合わせてVimのColorschemeを切り替えるプラグインを書いた話をします。  


## 成果物
https://github.com/kato-k/vim-auto-color-switcher  

![](https://storage.googleapis.com/zenn-user-upload/3r3lhfyemcgwc42lm3ewm8xmksaw)

GIFのようにmacOSの外観モードの変更に合わせて即座にカラースキームを切り替えらるプラグインです。

Windows用のソースも用意したのですが、うごきませんでした。  
jobあたりでコケているはずですが、WindowsのVimの環境を作っていないので検証していません。  
~~私にとってのWindowsはゲーム機なので仕方ないです。HITMANたのしい~~  

## 発端
私はmacOS Mojaveから追加されたダークモードを頻繁に利用します  
ですが、外観モードを変更する度にVimのカラースキームを変更しなければならないことに不満を抱いていました。  
そこで自分で作ってみることにしました。  

MacVim用であれば同じことが可能なプラグインが[ある](https://github.com/L-TChen/auto-dark-mode.vim)のですが、これはMacVim独自の`OSAppearanceChanged`と、`v:os_appearance`が必要でした。  
(実はこのプラグインについては作った後に[Vim-jp Slack](https://vim-jp.org/docs/chat.html)で[教えていただきました](https://vim-jp.org/slacklog/CLKR04BEF/2020/12/#ts-1607139153.026300)。ありがとうございます)  

## 使い方
詳しくはGithubの[README](https://github.com/kato-k/vim-auto-color-switcher)を見ていただきたいのですが、軽く説明させていただきます。  

### インストール
Macを利用している方で、Dein・Vim-plug等のプラグインマネージャを利用している場合、
リポジトリをvimrc等に追記・buildのオプションにmakeを追加をすることでバイナリのビルドを含めたインストールができます。

#### それ以外の場合
はじめに下記のようにして、バイナリを作成してください。  
``` sh
$ curl -l https://raw.githubusercontent.com/kato-k/vim-auto-color-switcher/main/plugin/auto_color_switcher.swift > ~/Downloads/auto_color_switcher.swift
$ swiftc ~/Downloads/auto_color_switcher.swift -o ~/Downloads/auto_color_switcher
```
このバイナリのパスをvimrcなどに  
`let g:auto_color_switcher#binary_path=expand('バイナリのパス')`  
のように教えてやるととひとまず動作します。  

#### 補足
この記事を投稿した後、Vim-jpでプラグインマネージャには自動でmakeする機能があることを教えていただきました。  
ありがとうございます!  

### オプション
このままでは、backgroundを切り替えるだけなので対応していないカラースキームでは[よくない動作](https://github.com/kato-k/vim-colorscheme-settings/pull/7)を引き起こします。  
ですので、その場合は  

```
let g:auto_color_switcher#command={
    \ 'light': 'colorscheme xcodelight',
    \ 'dark' : 'colorscheme xcodedark'
    \}
```
こちらのようにして、動作を上書きすることができます。  


## 概要

OSの外観モードが変更されたタイミングでstdoutに状態を出力するコマンドを作り、Vimとお喋りさせてます。

### Vim側の動作
Vimで外部プログラムと非同期に通信するには、`job`機能を利用します。  
サーバーを立ててごにょごにょする手もあるようなのですが、そこまで高度なことをする必要もないと判断したため`job`で起動したコマンドの標準出力を引っ掛ける`out_cb`を利用しました。  

### コマンドの動作 (macOSの場合)
Vim単体ではOSの外観モードを取得できないor遅い（AppleScriptを利用）ので、swiftを利用して変更の検知と通知をするコマンドを書きました。  
Foundationの`UserDefaults`から次のように現在の状態を取得しています。  
`UserDefaults.standard.string(forKey: "AppleInterfaceStyle") ?? "Light"`  

OSの設定の変更を検知するためにcocoaのobserver(???)を使い、変更があった時のみ↑の結果を取得するようにしています。  

## 他OSのサポートについて
### Windows
こちらはとりあえず動くソースをC#で書いてみたんですが、(多分)うごきませんでした。  
WindowsのVimはまったくわかりません。  
そもそも1フレームごとにレジストリにアクセスする仕様なのでHDD/SSDに負荷をかけそうで怖いです。（キャッシュに乗ってるのでしょうか）  

### Linux
こちらは調べてみましたが、無理です。  
例えば、Ubuntuでは設定から、ダークモード・ライトモード等を切り替えることができるように見えます。  
ですが、これはGNOMEの一テーマであるyaru-dark/yaru-lightを切り替えているに過ぎずwmレベル（詳しくないので間違ったこと言ってるかも）でのLight/Darkモードではありません。  
UbuntuのGNOMEですらもこの有様ですから、さすがに無理と判断しました。  

## まとめ
このプラグインと[Xcodeカラースキーム](https://github.com/arzg/vim-colors-xcode)を一緒に利用したところ、非常にmacに馴染むVimができました。うれしいです。  

OSとの親和性が気になるmacOSをお使いのVimmerの皆さん！  
ぜひお使いください。  

ちなみにこの記事は[CotEditor](https://coteditor.com)で書きました。  
SKK勉強してAlacrittyでも日本語打てるようにしてみようかしら？  

###### おまけ
これまでに作ったVimプラグイン  
https://github.com/kato-k/nyancat.vim  
https://github.com/kato-k/vim-colorscheme-settings  


