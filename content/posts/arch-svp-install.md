---
title: "Arch LinuxにSVPを導入する"
date: 2021-08-12
tags: ["linux", "arch", "svp"]
---

## インストール
AURにパッケージがあるのでそれをインストールするだけ。
依存関係の衝突は無視した。（だめ）

```
yay -S spv
```

`leptonica`が404で入手できなかったので自前でコンパイルする。
もしかしたらうまくインストールできるかもしれない。

`$HOME/local/`にインストールすることにする。
```
git clone https://github.com/DanBloomberg/leptonica
cd leptonica
./autogen.sh
./configure --prefix=$HOME/local/
make # make -j16 (faster)
make install
```

参考
`https://tesseract-ocr.github.io/tessdoc/Compiling.html`


## 利用
SVPを起動する。おそらくアプリケーション一覧に入っている。
本記事では動画の再生にVLCプレイヤーを利用することにした。

ドキュメントによるとSVP`/usr/lib/vlc/plugins/video_filter`への書き込み権限を与える必要があるらしい。
めんどくさいので以下のコマンドを利用した（とてもだめ）
(SVPが終了しているか確認する)

```
chmod 777 /usr/lib/vlc/plugins/video_filter
```

その後、SVPコントロールパネルの右上のアイコンをクリックし、
`ユーティリティ→SVP in VLC`をクリックする。
一度VLCが起動するが、その後は普通のアプリケーションメニューから起動してもSVPと連携される。