---
title: "Vim script で湯婆婆を実装してみる"
date: 2021-01-05
tags: ["vim", "scrap"]
categories: ["vim"]
---

## はじめに
この記事は暇つぶしに書いた**ネタ**です。

## コード
```vimscript:yuba-ba.vim
let s:Keiyakusho = function("input", ["契約書だよ。そこに名前を書きな。\n"])

let s:name = s:Keiyakusho()
echo "\nフン。"..s:name.."というのかい。贅沢な名だねぇ。"

let s:name_index = rand()%strchars(s:name)
let s:new_name   = strcharpart(s:name, s:name_index, 1)

echo "今からお前の名前は"..s:new_name.."だ。いいかい、"..
      \s:new_name.."だよ。分かったら返事をするんだ、"..s:new_name.."！"

```
こちらがコードです。  
ファイルに保存し、こちらのコードを `:so yuba-ba.vim` とすれば動きます。  

## コードの解説
### 契約書
Vim scriptでは `input() `関数を用いてプロンプトから文字列を取得できます。 `:h input()`  
これを、 `function()` 関数を用いて部分適用し、セリフを言い名前を聞く `s:Keiyakusho` を作ります。  

文字列の連結は `.` または `..` で行います。  
`.` は辞書の参照 `my_dict.key` 等と被り可読性を落とすので、新しい`..`を用いました。  

```vimscript
let s:Keiyakusho = function("input", ["契約書だよ。そこに名前を書きな。\n"])

let s:name = s:Keiyakusho()
echo "\nフン。"..s:name.."というのかい。贅沢な名だねぇ。"
```

### 名前を奪う
新しい名前を決定するために乱数が必要でした。  
`:h rand()` を調べたところ存在した(!?)ので、こちらの関数を用いることにしました。  

また、最大値を指定するために文字数を取得する `strchars()` 関数を利用しました。Vimには `strlen()` 関数がありますが、マルチバイト文字には対応していません。  

```vimscript
let s:name_index = rand()%strchars(s:name)
let s:new_name   = strcharpart(s:name, s:name_index, 1)
```

## 実行結果
![スクリーンショット 0002-11-08 午後7.19.30.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/242536/5becf656-cd7a-9e83-3579-4b2cf0edeb9e.png)

## まとめ
- 部分適用なのかわからない  
- [github](https://github.com/kato-k/yubaba-vim)  

