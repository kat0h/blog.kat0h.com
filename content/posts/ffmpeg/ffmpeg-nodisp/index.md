---
title: "[FFmpeg]  -nodispについて"
date: 2021-01-05
categories: ["ffmpeg"]
tags: ["ffmpeg", "shell"]
---

## FFplayの音声再生時に画面出力をしない

### 結論
``` $ ffplay -nodisp ```  
``` $ ffplay -v quiet ```

### 解説

#### ``` $ ffplay -nodisp ```

その名の通りグラフィカルな画面表示を無効化する

#### ``` $ ffplay -v quiet ```

-v は -loglevel のエイリアスでログレベルを指定する。
quietは全てのshellの出力をとめる。
error くらいが丁度良い。

### おまけ

以下のコマンドでNHK第一をShellから聴ける

``` ffplay -i 'https://nhkradioakr1-i.akamaihd.net/hls/live/511633/1-r1/1-r1-01.m3u8' -nodisp -v quiet& ```

### 参考文献
[https://ffmpeg.org/ffplay.html](https://ffmpeg.org/ffplay.html)
[https://gist.github.com/riocampos/93739197ab7c765d16004cd4164dca73](https://gist.github.com/riocampos/93739197ab7c765d16004cd4164dca73)
