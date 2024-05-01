---
title: Raspberry Pi 4でMisskeyを立てる際のTips
description: 
published: true
date: 2024-05-01T04:47:45.801Z
tags: delivery, raspberry pi
editor: markdown
dateCreated: 2023-04-07T22:58:04.892Z
---

> **Note**
> - この記事はGoogleキャッシュからサルベージしました。(4月以前に投稿された記事です)
> - WIki管理人注: ページタイトルを変更しました。


## ちょっとメモ

RPi 4B 4GBのやつを使う場合はswapをoffにする
redisがswapに書き込み始めてwating queueがめっちゃものすごく増える

postgresqlのeffective_cacheみたいなのはなるべく増やしたほうが良い 

crontabでsystemctl restart misskey みたいな感じで定期的に再起動をかけるべきかも 
Misskeyがものすごくメモリリークするので...