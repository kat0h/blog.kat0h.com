---
title: "Amazon Fire TVにtermuxを導入
"
date: "2022-01-25"
categories: ["android"]
tags: ["linux", "android", "termux"]
---

できること<br>
    - python3などのスクリプトを実行
    - vim

必要なもの<br>
    - Bluethooth接続のキーボード&マウス

手順<br>
大きく3つのステップに別れる<br>
    - F-droidの導入
    - Termuxの導入
    - sshdの設定
    - F-droidの導入
            - アプリケーションに管理にF-droidを用いる
                    - ApkPureなどの著作権を侵害したapkストアではない
                    - 起動直後はアプリケーションリストの更新に時間がかかるため待つ
                            - 更新に失敗した場合マウスで画面を上から下にドラッグする
            - 手順
                    - Firetvの設定から未知のアプリケーションのインストールをONにする
                            - Myfiretv -> 開発者設定
                    - FiretvアプリストアからDownloaderアプリをインストールする
                            - 勝手に課金されるようなことはないと思われるが、注意すること
                    - DownloaderアプリからF-droidをインストールする
    - Termuxの導入
            - 未知のアプリのインストール権限をF-droidに付与する
            - Termuxをインストールする
    - sshdの設定
            - TermuxではFireTVのソフトウェアキーボードを利用できない
                    - Termuxを起動すると数秒画面がちらつくので待つ
                    - マウスで下部ボタンを左にフリックするとテキストボックスが展開する
                            - コマンドはここに入力する
                            - 文字の削除やカーソルキーは利用できないので間違えないようにする
                            - ソフトキーボードが展開している場合、物理キーボードのESCキーで改行できる
                            - FireTVの言語をEnglishに変更しておくとIMEがないため快適
                    - sh
                            - $ apt update && apt upgrade
                            - $ apt install openssh
                            - $ whoami #ユーザー名を確認 
                            - $ passwd #パスワードを設定
                            - $ ip a | grep 192.168 #ipアドレスを確認
                            - $ sshd -p 8020 #20番ポートは使えないので8020を使う
                    - less等のページャは利用できないのでgrepを駆使して必要な行を特定
                    - 詳しいリファレンス
                            - [https://wiki.termux.com/wiki/Remote_Access](https://wiki.termux.com/wiki/Remote_Access)
    - 接続
            - `$ ssh ****@192.168.***.*** -p 8020`
            - 登録したパスワードとユーザー名でログインできるはず
