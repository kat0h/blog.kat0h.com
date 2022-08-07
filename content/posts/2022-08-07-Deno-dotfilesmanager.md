---
title: "Denoで自分用Dotfilesマネージャーを作ってみた"
date: "2022-08-07"
tags: ["deno", "dotfiles"]
---

https://github.com/kat0h/dfm  

DFMというDotfilesマネージャーフレームワークを自分用に作ってみました。  
Denoで動くTypescript製のプログラムで、現在のところ下記機能をプラグインとして実装しています。  
- シンボリックリンクの管理  
- コマンドの存在確認  
- gitコマンドの統合  

## 経緯  
個人的に、これまでのDotfilesマネージャに以下のような不満がありました。  
- コマンド名・オプションをすぐ忘れる  
- DSLが複雑で覚えにくい  
- インストールが面倒かつ、複雑な依存関係を持つ  

そこで、これらの不満を解決できる仕組みはないか考えたところ、Denoを利用するアイデアを思いつきました。Denoの依存解決システムを利用することで、下記のようなファイルを一つ用意するだけでそのファイルが、設定ファイル・実行するコマンドの両方を兼ねることができます。また、DSLによる複雑な条件分岐などを覚えずに慣れ親しんだTypescriptの記法を利用できます。  

```typescript  
#!/usr/bin/env deno run -A  
import Dfm from "https://deno.land/x/dfm/mod.ts";  
import {  
  CmdCheck,  
  Repository,  
  Symlink,  
} from "https://deno.land/x/dfm/plugin/mod.ts";  
import { fromFileUrl } from "https://deno.land/std/path/mod.ts";  
import { os } from "https://deno.land/x/dfm/util/mod.ts";  

const dfm = new Dfm({  
  dotfilesDir: "~/dotfiles",  
  dfmFilePath: fromFileUrl(import.meta.url),  
});  
const s = new Symlink(dfm);  
const c = new CmdCheck();  
const r = new Repository(dfm);  

s.link([  
  ["zshrc", "~/.zshrc"],  
  ["tmux.conf", "~/.tmux.conf"],  
  ["vimrc", "~/.vimrc"],  
  ["vim", "~/.vim"],  
  ["config/alacritty", "~/.config/alacritty"],  
]);  

c.cmd([  
  "vim",  
  "nvim",  
  "clang",  
  "curl",  
  "wget",  
]);  

if (os() === "darwin") {  
  c.cmd(["cot"]);  
}  

dfm.use(s, c, r);  
dfm.end();  
```  

↑のファイルはそのまま実行が可能で、↓が実行した様子です。  

![](https://user-images.githubusercontent.com/45391880/181011253-1f5bc9b7-47aa-42d1-97cd-ab39ff8c4931.png)  

フレームワークはコマンド本体となるDfm classと、実際にリンクなどを管理するPluginに分けられています。  
まだ実装していませんがPluginとしてデフォルトシェルの設定機能や、キーボードの再レイアウト、フォントの設定機能など柔軟に機能を追加できる基盤を整えました。  

## 使い方  
```typescript  
#!/usr/bin/env deno run -A  
import Dfm from "https://deno.land/x/dfm/mod.ts";  
import { fromFileUrl } from "https://deno.land/std/path/mod.ts";  

const dfm = new Dfm({  
  dotfilesDir: "~/dotfiles",  
  dfmFilePath: fromFileUrl(import.meta.url),  
});  

dfm.end();  
```  
プラグインを利用しない場合、設定ファイルは↑のようになります。  
この設定ファイル兼コマンドを実行すると↓のような出力を得ることができます。  
```  
$ ./command.sh  

dfm(3) v0.3  
	A dotfiles manager written in deno (typescript)  

USAGE:  
	deno run -A [filename] [SUBCOMMANDS]  

SUBCOMMANDS:  
	stat	show status of settings  
	list	show list of settings  
	sync	apply settings  
	help	show this help  
```  

dfmは標準で4つのサブコマンドを実装しています。  
- stat  
	- それぞれのプラグインに渡された設定がきちんと適用されているかをチェックします。例えば、Symlinkプラグインはstatコマンドでリンクがきちんと貼られているかを確認します。  
- list  
	- それぞれのプラグインが管理する設定の一覧を確認できます。  
- sync  
	- プラグインが設定を同期します。  
- help  
	- ヘルプを表示します。  


このままでは何もできないので、Pluginを利用してDFMの動作を拡張します。  
今のところ、dfmでは3つのPluginを実装しています。  
- symlink.ts  
	- シンボリックリンクを管理します  
- cmdcheck.ts  
	- 指定されたコマンドが存在しているかをチェックします  
- repository.ts  
	- Dotfilesディレクトリを起点にgitコマンドや$EDITORを実行するサブコマンドを提供します  

記事の冒頭で載せたソースコードはプラグインを利用した様子です。プラグインのインスタンスの参照をdfmに渡しdfmの管理下に置きます。  

![](https://user-images.githubusercontent.com/45391880/181011253-1f5bc9b7-47aa-42d1-97cd-ab39ff8c4931.png)  
↑が実際にコマンドを実行した様子です。  
statサブコマンドが.vimrcのリンク切れを検知している様子がわかります。  

## ユーティリティ関数  
https://deno.land/x/dfm/util/mod.ts から利用できます。  
- expandTilde()  
	- "~/"などを展開します、"~user"形式は展開できません。  
- resolvePath(path: string, basedir?: string)  
	- ~ $BASEDIR -> $HOME  
	- ../ $BASEDIR -> $BASEDIR/../  
	- ./ $BASEDIR -> $BASEDIR  
	- a $BASEDIR -> $BASEDIR/a  
	- ./hoge/hugo -> join($(pwd), "./hoge/hugo")  
	- /hoge/hugo -> "/hoge/hugo"  
	- ~/hoge -> "$HOME/hugo"  
- isatty()  
	- Cのisatty()と同じ  
- os()  
	- Denoが何のOS向けにビルドされたかを返します  

## セキュリティ  
denoは外部のURLからソースコードをimportしてそのまま実行している関係上、サプライチェーン攻撃に晒されるリスクが一定あります。ただし、[deno.land/x/](deno.land/x/) については、deno.landがバージョン番号をURLに指定している限り中身が不変であることを保証しています。  
もしdfmを利用する際はimportの際にURLに必ずバージョン番号を指定することが肝心となります。  

## 反省  
Denoの依存解決システムを使って、インストールの必要がない設定付きコマンドを作るというアイデア自体は便利だと思うのですが、正直ちょっとしたDotfiles管理にこれはやりすぎたなとおもいました。  
プラグインシステムの出来もお世辞には良いとは言えないし...  
