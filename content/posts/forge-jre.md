---
title: "[1.12.2][JRE] Minecraft Forgeサーバが起動しないとき"
date: "2022-01-24"
categories: ["minecraft"]
tags: ["minecraft", "java"]
---

1.12.2のForge鯖にはJRE8が必要。
グローバルにjava17がインストールされているとサーバーが起動しない

JRE8をを鯖インストールフォルダに突っ込んで直接利用する
https://www.oracle.com/java/technologies/downloads/

run.batにはjava.exeの相対パスを記入する。

以下run.batの例
```bat
@echo off
.\jre1.8.0_321\bin\java.exe  -Xmx4096M -Xms4096M -jar forge-1.12.2-14.23.5.2859.jar nogui
pause
```