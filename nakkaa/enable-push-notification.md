---
title: プッシュ通知を有効化する
description: 
published: true
date: 2024-05-01T05:11:20.422Z
tags: push, notification
editor: markdown
dateCreated: 2023-03-30T11:43:53.867Z
---

# プッシュ通知を有効化する

## クライアントの動作環境

以下のクライアントで動作することを確認

- iOS 16.4 以上
- Android 12で動作報告あり

## 設定方法

1. コントロールパネルの全般を開く
2. ブラウザへのプッシュ通知を有効にするのチェックをオンにする
3. Public keyとPrivate keyを入力する
4. 保存ボタンを押下する

## Public keyとPrivate keyの生成方法

node.jsが動く環境で以下を実行し、出力されたキーを入力する。

```sh
npx web-push generate-vapid-keys
```

実行例(macOS)

```
❯ npx web-push generate-vapid-keys
npx: 18個のパッケージを5.044秒でインストールしました。

=======================================

Public Key:
B****

Private Key:
5****
```

## 情報源

@catさん、@cyberrexさん ありがとうございます。

- https://mi.cbrx.io/notes/9cveh93q9d
- https://kokuzei.cyou/notes/9cvepuk8m8