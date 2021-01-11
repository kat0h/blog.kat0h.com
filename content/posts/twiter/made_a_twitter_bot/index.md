---
title: "時間割をつぶやくTwitterBotを作った"
date: 2021-01-11T17:22:06+09:00
tags: ["twitter", "python"]
categories: ["twitter"]
---

時間割の予定を毎日0:00分にツイートするスクリプトを書きました。  
この記事ではスクリプトについて簡単に解説をします。

成果物はこちらです↓  
[kato-k/timetable_bot（Github）](https://github.com/kato-k/timetable_bot)  
[Twitter（@timetable_ysr）](https://twitter.com/timetable_ysr)

Botの大まかな仕組みは
```
[Pythonスクリプト] -> [twty] -> [Twitter]
```
といった形になっており、一連の流れを記述したシェルスクリプトをcrontabを使いRaspberryPiで動かすことで自動ツイートを実現しております。

### Pythonスクリプト
弊校の時間割はクラスごとのA週・B週2種類の時間割と、共通に割り当てられた{A, B}{月, 火,水 木, 金, 土, 日}の予定から決定されます。  
ですので、こちらの2つの情報を総合することで特定の日時の時間割を求めることができます。

{{<figure src="./timetable_desition.png" alt="時間割決定の仕組み">}}
~~スクリプト内でデータのCSVのパスを直接書き込んでるのは内緒~~

最後にスクリプト内でツイートする文面を作成しています。

### twty
通常Twitterは、TwitterWebAppや公式のTwitterクライアントから投稿します。  
これに対しTwitterBotはTwitterAPIを利用してツイートの投稿をします。  
こちらのAPIはTwitterに申請をすることで取得できるのですが......

> Thank you for your interest in the Twitter developer platform. Based on our review of your use case, we are unable to approve your developer application at this time.
> - Applications may be rejected if they are found to be in violation of any section of the Developer Agreement and Policy, Automation Rules, Display Requirements, and/or the Twitter Rules.
> - We don’t currently allow you to appeal this decision. We are investigating options to allow people > - who feel they’ve been inappropriately rejected to appeal. Please stay informed for future updates.
> - We cannot comment on specific applications in public channels, including through official Twitter handles or our developer forum.

> [意訳]  
> オメーのクソみてーなユースケースにAPIの利用権限渡すわけねーだろバーカ
>  - そもそもTwitterAPIの利用ポリシーに違反してんだよこのクズ
>  - 俺の決定は絶対だからな異議申し立て?んなこと受け付けるわけねーだろ
> -  オメーはTwitterDevから永久追放な

......F****** Twitter!!!!  

まあ、最後はキレ気味にメールに返信していたので仕方ないでしょう。  
再申請も不可とか......は?  
そんなことがありましたので、shell上で動作するGolang製TwitterClientである```twty```をツイートに利用することにしました。


```twty```はファイルから文面を読み込む場合、改行を含めて文章がツイートされます。  
ですから、スクリプトで作成した文面を一時ファイルに保存してから```twty```を実行しています。

### crontab
じゃあ以上で完成したシェルスクリプトをcrontabに登録して毎日、定期ツイートしましょう。  
crontabの使い方なんて読者の方が詳しいでしょうから詳しくは書きません。
強いて言えばスクリプトの実行時のディレクトリに気をつければ無問題でしょう。

### まとめ
以上がBotの解説です。  
作ったものは基本的にツイートするだけのものですから、構造は非常に簡単なものです。  

### 最後に
{{<tweet 1348128939667910656>}}
そういうことです
