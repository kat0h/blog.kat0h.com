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