---
title: 2000人までのインスタンス運営
description: 
published: true
date: 2024-05-01T04:34:37.431Z
tags: operation, delivery
editor: markdown
dateCreated: 2023-04-07T22:51:26.419Z
---

:warning: この記事はGoogleキャッシュからサルベージしました。(4月以前に投稿された記事です)

# 2000人までのインスタンス運営

## お一人様スタート

2CPU 4GBMemory くらいのVPSサーバーでdocker-compose upでうごく ただし、Object Strageは使うこと。 書き込み頻度やなんやらで書き込みに失敗し、DBとファイルシステム上のファイルの状態が一致しなくなることがある

## トータル300人くらいまでの壁（同時接続100人〜）

2CPU 8GBMemory くらいじゃないときつくなる CDN(Cloudflare)いれたのもこの辺

## トータル700人くらいの壁（同時接続200人〜）

RDBが詰まってなにもかもが死ぬ

どうやら接続が詰まっているらしい pg-bouncer の設定が必要になると思われるが、pg-bouncerを有効にする設定が結構難しい。

また、前後してInbox queueとDeliver queueがつまりそうになる .config/default.yml のワーカー周りの設定を調節する必要がある デフォルトだと1ワーカーで deliverJobConcurrency: 128, inboxJobPerSec: 16になっている メモリが余っているなら clusterLimit を増やすとその分ワーカーが増えるので、メモリ消費がその倍数増えていって計算がしやすい 実際に処理できる速度と設定上の速度をある程度あわせてあげないと、うまくパフォーマンスがでないらしい（顕著なのはメモリ上限を超えるとストレージにスワップし始める）

## トータル1000人くらいの壁（同時接続300人前後）

Websocket（ストリーミング）は安定してるけど、DBのSelectが遅くなる DBにリードレプリカを設定すると改善する

## 同時接続400人の壁

DBのスペックが足りないので上げる リードレプリカを1個足す


test