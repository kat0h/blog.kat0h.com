---
title: "ddc.vimでのソースの扱い"
date: "2021-12-05"
categories: ["vim"]
tags: ["vim", "ddc", "lsp"]
---

# ddc.vimでのソースの扱い(vim-lspとarroundを併用したいときなど)

ddc.vimではソースの指定順を優先順位として表示する。例えば

```vim
call ddc#custom#patch_global('sources', ['vim-lsp', 'arround'])
```

このようにすることで、vim-lspとarroundから同じ候補が帰ってきたときにvim-lspの候補を優先して表示する。
これによって、lspのスニペット正しく利用することができる。

また、デフォルトのddcは一つのsourceから同じ候補が帰ってきた際に候補をマージしてしまう。
これはsourceが下記の様な候補を返してきたときに不便になる。

```
#include (snippet: #include <>)
#include (snippet: #include "")
```
これを防ぐには下記のような設定をddcに渡す必要がある。
```vim
call ddc#custom#patch_global('sourceOptions', {
\ 'source_name': {
\   'dup': v:true,
\ },
\ })
```