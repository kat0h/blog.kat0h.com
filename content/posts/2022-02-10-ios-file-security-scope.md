---
title: "[iOS] ファイルのsecurity scopeについて"
date: "2022-02-10"
tags: ["iOS", "Swift"]
---


iOSのアプリはSandboxの中で動作しているため、自らのSandboxの外にあるファイルには通常アクセスできない。  
この制限を超えるには`UIDocumentPickerViewController`や`UIDocumentBrowserViewController`から返される`URL`を利用する必要がある。  
このURLにはシステムからファイルへの一時アクセス許可が付与されており、`url.startAccessingSecurityScopedResource()`と`url.stopAccessingSecurityScopedResource()`を用いてurlのデータにアクセスすることができる。  

ファイルへのアクセス許可はアプリを再起動すると失われるため、URLへ永続的にアクセスするには`url.bookmarkData()`を用いてData型のsecurity scoped bookmarkを取得しておく必要がある。  

以下サンプルコード(WWDC2018 Managing Documents In Your iOS Appsより抜粋)  
```
let bookmarkdata = try? url.bookmarkData()

var bookmarkdataisstale = false
var documentURL = try? URL(resolvingBookmarkData: bookmarkData,
                           bookmarkDataIsStale: &bookmarkDataIsStale)
```


参考文献  
- https://developer.apple.com/videos/play/wwdc2018/216/
    - 35:00~で今回のテーマが扱われている
