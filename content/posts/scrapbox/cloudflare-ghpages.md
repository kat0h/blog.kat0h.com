---
title: "CloudflareとGithubPagesでブログ"
date: "2022-01-25"
categories: ["web"]
tags: ["cloudflare", "githubpages"]
---

## Cloud Flareでドメインを取得
今回は`kat0h.com`を取得した (年間$8)

## Github Pagesはホスティングにのみ使用する (`kat0h.github.io`)
### CloudFlareのネームサーバーを利用する
- CloudFlare→Websites→`kat0h.com`→DNS→`Add record`
    - Type
        - CNAME (指定のドメインに飛ばす)
    - Name (www.example.comのwwwの部分)
        - blogにした
        - `@`で`kat0h.com`へのアクセスを捌けるっぽい
    - Target
        - `kat0h.github.io`

Githubでホストしているサイトのリポジトリに飛ぶ  
Settings→Pages→Custom domainに`blog.kat0h.com`を登録  
`blog.kat0h.com`からのリクエストにも応答するようになる  

## その他  
ホスティングにはCloudFlarePagesを使うのもあり  
[https://riq0h.jp/2021/08/07/132024/](https://riq0h.jp/2021/08/07/132024/)
リポジトリの本番に指定していないブランチも含めてビルドされるので注意  
