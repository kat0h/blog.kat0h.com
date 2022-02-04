---
title: "[Swift iOS] PDFをレンダリングする"
date: "2022-02-04"
tags: ["swift", "pdf", "iOS"]
---

## 大きく3つのアプローチがある
- CoreGraphicsを使用する
- PDFKitのPDFThumbnailViewを使用する
- PDFKitのPDFViewを使用する

## それぞれの特徴

|アプローチ|方式|ズームの可否|描画速度|
|---|---|---|---|
|CoreGraphics|CGContextに直接描画|不可(描画範囲を指定して再描画)|速い|
|PDFKit Thumbnail|UIImageとして取得|不可(描画範囲を指定して再取得)|速い|
|PDFView|UIViewとして取得|可|遅い（読み込み中は低解像度のサムネイルが差し込まれる)|


|アプローチ|テキスト選択|ドキュメント|iOS Ver|
|---|---|---|---|
|CoreGraphics|不可(自前の実装が必要)|https://developer.apple.com/documentation/coregraphics/cgpdfdocument|iOS 2.0+|
|PDFKit Thumbnail|不可(ただのUIImage)|https://developer.apple.com/documentation/pdfkit/pdfview|iOS 11.0+|
|PDFView|可(PDFKitが管理)|https://developer.apple.com/documentation/pdfkit/pdfthumbnailview|iOS 11.0+|


- SafariのようにPDFをScrollViewで表示したいだけ
    - 　PDFKitを利用
- PDFのページ画像が欲しいだけ
    - PDFKitのPDFThumbnailViewで欲しいサイズのサムネイルをUIImageとして取得する
- テキストを選択できるようにしたい
    - PDFKitを利用
        - (i文庫HDなどは自前で選択UIを実装している(多分))

### それぞれのアプローチで動作するサンプルコード (Playground)
#### CoreGraphics
[https://qiita.com/Nonchalant/items/b94ee09e9bac5c938c62](https://qiita.com/Nonchalant/items/b94ee09e9bac5c938c62)

### PDFThumbnailView
```swift
import PlaygroundSupport
import UIKit
import PDFKit

guard let url = URL(string: "https://www.ci.seikei.ac.jp/yamamoto/lecture/automaton/text.pdf") else { fatalError() }
guard let doc = PDFDocument(url: url) else { fatalError() }

guard let page = doc.page(at: 0) else { fatalError() }
let image = page.thumbnail(of: CGSize(width: 1000, height: 1000), for: PDFDisplayBox.trimBox)

let view = UIImageView(image: image)
view.backgroundColor = UIColor.systemGray
view.contentMode = .scaleAspectFit
PlaygroundPage.current.liveView = view
```

### PDFView
```swift
import PlaygroundSupport
import UIKit
import PDFKit

guard let url = URL(string: "https://www.ci.seikei.ac.jp/yamamoto/lecture/automaton/text.pdf") else { fatalError() }
guard let doc = PDFDocument(url: url) else { fatalError() }

let view = PDFView()
view.document = doc
PlaygroundPage.current.liveView = view
```