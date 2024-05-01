---
title: メールサーバーの設定(iCloud編)
description: MisskeyのメールサーバーにiCloudを指定する
published: true
date: 2024-05-01T05:12:04.964Z
tags: mail, smtp, icloud
editor: markdown
dateCreated: 2023-03-25T09:26:10.075Z
---

# メールサーバの設定(iCloud)

- Misskeyのメール送信にiCloudメールのSMTPサーバーを使う方法を紹介する。
- iCloud+ (月額130円)を契約すると、独自ドメインのメールアドレスがiCloudメールで使える。

## 前提条件

- iCloud+を契約済み
- 独自ドメインのメールアドレスを設定済み (参考: [iCloud メールでカスタムメールドメインを使う - Apple サポート (日本)](https://support.apple.com/ja-jp/HT212514) )

## 設定方法

1. [App 用パスワードを使って Apple ID で App にサインインする - Apple サポート (日本)](https://support.apple.com/ja-jp/HT204397)　を参考に、App用パスワードを作成する
2. Misskeyのコントロールパネルを開いてメールサーバーを選択する。
3. `メール配信機能を有効化する (推奨)` をオンにする。
4. `メールアドレス` に独自ドメインのメールアドレス(Misskeyからのメール通知の送信元となる)を入力する。
5. `SMTP サーバーの設定` を以下のように設定する。

| 項目 | 設定値 | 備考 |
| --- | --- | --- |
|ホスト|smtp.mail.me.com| |
|ポート|587| |
|ユーザー名|example@icloud.com|iCloudのメールアドレス|
|パスワード|***|1で設定したApp用パスワード|

6. `SMTP 接続に暗黙的なSSL/TLSを使用する` はオフにする。
7. 保存ボタンを押す
8. 配信テストボタンを押して成功すれば完了。