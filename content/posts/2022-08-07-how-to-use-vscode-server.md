---
title: "VS Code Serverの使い方"
date: "2022-08-07"
tags: ["editor", "vscode"]
---

VSCodeのJune2022 Updateが正式に公開されました。  
本稿では、アップデートと同時に公開されたVisual Studio Code Serverの使い方について解説します。  

![](https://storage.googleapis.com/zenn-user-upload/7081c7615186-20220710.png)

## 概要
VSCode Serverの概要については、下記記事を閲覧ください。  
（窓の杜）https://forest.watch.impress.co.jp/docs/news/1423348.html  
（公式）https://code.visualstudio.com/blogs/2022/07/07/vscode-server  

Code Serverは将来的にVSCodeの**code(1)に統合**されることを念頭に設計された機能で、ChromeなどのWebブラウザーをVSCodeのフロントエンドとして利用します。  
これまでもブラウザーの中で完結するVSCodeとして[vscode.dev](https://vscode.dev)や[github.dev](https://github.dev)を利用できましたが、`code-server`はあくまでコンピューター上で実行されているのでLanguage serverなどの外部プロセスを利用する拡張なども利用できます。  
また、vscode.devからローカルのサーバにアクセスが可能になる**トンネリング機能**をサポートしているため、サーバーを開発用のDockerにインストールして別のマシンのvscode.devからアクセスするといったことが可能になります。  
トンネリング機能のみPrivate Previewとしてアクセスをリクエストする必要がありますが、ローカルにサーバーを建てるだけであればwaitlistに登録することなく利用できます。  

## インストール
基本的にすべての手順は下記ページにあります。正確な情報が知りたい場合、こちらを参照してください。  
https://code.visualstudio.com/blogs/2022/07/07/vscode-server  

下記スクリプトをダウンロード後実行するだけで、インストールが完了します。  
途中、管理者権限を要求されます。インストール先→`/usr/local/bin/code-server`  
```sh
$ wget -O- https://aka.ms/install-vscode-server/setup.sh | sh
```

`code-server`コマンドにPATHが通っていることを確認してください。  

### アンインストール
```sh
$ sudo code-server uninstall
```

また、`~/.vscode-server`・`~/.vscode-cli`ディレクトリが生成されています。任意で削除してください。  

## 起動方法
トンネリングサービスはまだ一般に公開されていませんのでサーバーをローカルにのみ公開する方法を紹介します。  

`$ code-server serve-local`  
ライセンスに同意するかのプロンプトが出るので、同意してください。軽く読んだ限りだと、アプリケーションの画像などを公開することを制限するような文言は見られませんでした。  
`--accept-server-license-terms`オプションをつけると、ライセンスに同意したものとしてサーバーが起動します。  

ログに下記のようなメッセージが表示されます。URLにローカルからアクセスするとVSCodeを利用できます。  
`Web UI available at http://localhost:8000/?tkn=0e6b04be-fdf2-4392-b2bf-07ffd7d90143`  

### 終了
`Ctrl-C`でサーバーを終了できます。カーソルの描画がおかしくなる場合は`reset`コマンドを実行してください。  
その他のコマンドは`$ code-server -h`から確認できます。  

## 小技
WindowsのChromeではCtrl-Wなどのキーがブラウザーに取られてしまいます。  
Chromeの「ショートカットを作成」→「ウィンドウで開く」機能を利用すると、ほとんどのショートカットを利用できるようになるほか、画面を広く使うことができるため活用してください。  
![](https://storage.googleapis.com/zenn-user-upload/a4947cd1faa2-20220710.png)  

以上  
