---
title: サマリープロキシを自前で準備する
description: サマリープロキシをMisskey本体のじゃなくて外部のやつ使いたいときのメモ
published: true
date: 2024-08-15T12:28:43.236Z
tags: 
editor: markdown
dateCreated: 2023-11-05T20:43:39.452Z
---

## いきさつ

[こちら](https://hide.ac/articles/Y504SIabp)で紹介されていたサマリープロキシが数週間前に一般利用者向けの提供終了されたと思しきことが判明した（ある日突然プレビューが表示されなくなったのでおそらく死亡）ので、今回は自前でサマリープロキシを用意します。

Misskey本体にサマリープロキシ機能は標準搭載されているので、サマリープロキシの項目は未入力でも全く問題はございません。

今回は以下のような項目に該当される方がオプションで追加されることを想定してメモを残します。

・複数のサーバーを運営している方（各インスタンスごとにリクエストを要求するよりも一元でリクエストを送るようにしたい）

・TLに膨大なページ数のPDFが投下されたときに耐えうるサーバースペックを鯖が持ってない方

・Misskeyを運営するサーバーそのものでサマリープロキシとしてアクセスするのが望ましくないと思う方



## 1.サーバーサイドの準備

まず、サマリープロキシとしてめいめいさんがフォーク開発なさってる[summaly](https://github.com/mei23/summaly)というリポジトリを利用します。

リポジトリの説明の通り

### インストールとビルド
```
yarn install
yarn build
```

### サーバーとして動かす

```
yarn server
```

までを済ませます。エラーが発生せず起動が確認出来たらOK(自動起動化するならポート競合するので落としておく)

## 2.名前解決する

サマリープロキシが動作してる所にプロキシするURLを投げかけることで処理するので、URLがアクセスできる状態にしなければなりません。
極論IPベタ打ちでも通るのは通るのですが、URLを叩いてつなぐ方がスマートだと思うのでそうしましょう。

おそらく大半の場合はCloudflareのトンネルを通したりしているはずなので、今回はトンネルを使った方法を紹介します。

1.Cloudflare→Access→Tunnnelに移動
2.Misskeyを立てたときと同様にトンネルを作成する
　-画面の指示に従いCloudflaredをサーバーにインストール
　-画面の指示に従い情報を入力。上は解決するURL下は解決したいIP。ここではHTTPを選択する
　何も弄ってなければポートは3030となる
 
 ![タイトルなし.png](/cat/タイトルなし.png)

※SSLの認証などはサーバーサイドでよしなに






## 3.Misskeyのコントロールパネルで設定する

サマリープロキシの項目で先ほど割り当てたURL+/urlという形で入力する。

```
http(もしくはhttps)://example.com/url
```

コンパネで適用され次第リンクのプレビューなどが反映されるようになると思う。



### 余談

おすすめのサマリープロキシリポジトリを薦めて頂いたり、導入方法を詳細に教示していただき大変助かりました。ミス鯖工場の皆さんご協力ありがとうございました。

尚、サーバーを再起動する場合は再起動時にサーバーサイドのプロセスが死んでしまう事になるので、基本はsystemctlで自動起動するように設定しておきましょう。



sudo nano/vi/vim(好みのエディタ)  /_etc_/_systemd_/system/summaly.service　にて

```
[Unit] 
Description=Yarn Server Service

[Service]

User= //実行するユーザー名 
WorkingDirectory=　 //summalyまでのパス 
ExecStart=/usr/bin/yarn server //実行するコマンド
Restart=on-failure 

[Install] 
WantedBy=multi-user.target

```

こんな感じで設定して

`sudo systemctl daemon-reload`

`sudo systemctl enable summaly.service`

`sudo systemctl start summaly.service`

を実行して有効化すれば自動で起動するようになります。
もしstatusを確認して失敗していたらおそらくポートの競合か何かだと思うので適宜journal -dなりでご確認いただけますと幸いです。


