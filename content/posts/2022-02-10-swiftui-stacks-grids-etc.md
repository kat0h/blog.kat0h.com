---
title: "[iOS] ファイルのsecurity scopeについて"
date: "2022-02-10"
tags: ["iOS", "SwiftUI"]
---

# [SwiftUI][WWDC] Stacks, Grids, and Outlines in SwiftUI

cf. [https://developer.apple.com/videos/play/wwdc2020/10031/](https://developer.apple.com/videos/play/wwdc2020/10031/)<br>
SwiftUIでStackやGrid、アウトラインを取り扱う方法<br>

    - VStackを用いることで要素を順に並べることができる
            - 並べる要素の個数が固定かつ少ない場合特に問題になることはない
            - ViewはVStackの読み込み時に全て読まれるので数が増えてくるとレスポンスが悪くなる
    - LazyVStackとLazyHStackは画面に表示された要素のみを読み込む
```siwft
struct View1: View {
    var body: some View {
        ScrollView {
            LazyVStack(spacing: 0) {
                ForEach(1...100000, id: \.self) { i in
                    Text("「\(i)」")
                }
            }
        }
    }
}
```
            - Lazyを利用することで高速化が期待できる場合にのみ利用することが好ましい

    - LazyGrid
            - VStackやHStackではViewを四方に敷き詰める際、Viewのサイズを見て融通を効かせることが面倒だった
            - LazyGridはViewを四方に敷き詰めるだけでなく最小幅以上のViewをいい感じに並べる機能を持つ
```swift
struct View1: View {
    let colors = [
        Color.blue,
        Color.red,
        Color.green,
        Color.purple,
    ]
    var body: some View {
        ScrollView {
            LazyVGrid(columns: [GridItem(
                        .adaptive(minimum: 20
                        ))]) {
                ForEach(1...10000, id: \.self) { i in
                    Text("「\(i)」")
                        .background(colors[Int.random(in: 0..<colors.count)])
                        .border(Color.black)
                        .padding(1)
                }
            }
        }
    }
}
```
    - List
            - 子要素を教えることで再帰的なデータを読む

    - Playgroundで実行する
    - swift
```siwft
import PlaygroundSupport
import SwiftUI
 
// ここにサンプルコードを貼り付ける
 
PlaygroundPage.current.liveView = UIHostingController(rootView: View1())
```

[#swift](https://scrapbox.io/kat0h/swift)<br>
