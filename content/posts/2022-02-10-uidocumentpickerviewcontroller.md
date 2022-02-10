---
title: "[SwiftUI] UIDocumentPickerViewController"
date: "2022-02-10"
tags: ["SwiftUI", "iOS"]
---

ファイル選択ダイアログを表示する<br>
(アプリケーションがドキュメントベースの場合は`UIDocumentBrowserViewController`を使用(WWDCに詳しく解説したセッションがある(はず)))  
[![Image](https://scrapbox.io/files/61f5767a63aa43001e89a23e.png?type=thumbnail)](https://scrapbox.io/files/61f5767a63aa43001e89a23e.png)<br>
⚠️ UIKitに含まれる (= SwiftUIで使用するにはラッパを書く必要がある<br>
サンドボックスの外にあるファイルへのアクセスを可能にする性質上さまざまな制約がある<br>
公式ドキュメントを参照<br>

- 📚 公式ドキュメント<br>
   - [https://developer.apple.com/documentation/uikit/uidocumentpickerviewcontroller](https://developer.apple.com/documentation/uikit/uidocumentpickerviewcontroller)

- SwiftUI
  - [https://capps.tech/blog/read-files-with-documentpicker-in-swiftui](https://capps.tech/blog/read-files-with-documentpicker-in-swiftui)
                - txtファイルを開くtutor

- 複数選択を可能にする
  - UIDocumentPickerViewControllerのインスタンスの`allowsMultipleSelection`を`true`にする

- 複数選択が機能しないとき
  - [https://developer.apple.com/forums/thread/653192](https://developer.apple.com/forums/thread/653192)
