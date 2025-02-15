---
title: Misskey鯖缶Wiki
description: 
published: true
date: 2025-02-15T04:20:30.927Z
tags: 
editor: markdown
dateCreated: 2023-03-25T09:18:07.348Z
---

# Misskey鯖缶Wikiへようこそ

Misskey鯖缶Wikiは、Misskeyサーバーの管理・運用のナレッジ情報を共有するための非公式なWikiです。  
詳しくは[当Wikiについて](/about)へ。


> **:warning: 注意事項**
> なるべく正確な情報を記載するように心がけていますが、不正確な情報が含まれている可能性があります。
> 記載内容を利用する際は自己責任でお願いします。

# 脆弱性情報

- Misskeyの脆弱性情報は[Security Advisories · misskey-dev/misskey](https://github.com/misskey-dev/misskey/security/advisories)で確認できます。  
- 脆弱性対応のため、Misskey 2024.11.0以上にバージョンアップすることを推奨します。(2024/11/22現在)

# サーバー管理者向けコミュニティ

ここ以外にもサーバー管理者向けのコミュニティが複数存在します。  
そのうちのいくつかを紹介します。

- [鯖缶工場wiki](https://wiki.sabakan.industries) (Wiki/Discord)
  - [ぐすくま](https://abyss.fun/@guskma)さんが運営している、Mastodonを中心とした、分散型SNSのインスタンスの構築やサーバ管理のためのナレッジを蓄積するためのWikiです。

- [ミス鯖工場](https://misskey.systems/channels/9bul73n598) (Discord)
  - [にゃご](https://summary.ink/@cat)さんら有志が運営しているMisskeyサーバー管理者向けDiscordです。
  - Discord[招待リンク](https://discord.gg/k4twHFp2aE)

- [Misskey鯖缶詰所](https://nijimiss.moe/notes/01HJ17MGD6WMG73YQ2VFXT036Z) (Discord)
  - [皐月なふ](https://nijimiss.moe/@nafu_at)さんが運営しているMisskey鯖缶向けのコミュニティです。

また、サーバー管理者向けではないですが、脆弱性情報などを取り扱っているアカウント「[fedimagazine.info](https://fedimagazine.info/@antenna)」もあります。

# カテゴリ

`[タイトル](カテゴリ名/リンク)` みたいな感じでリンクを書いて、ページを作成してください。

## Postgresql

### メンテナンス

- [PostgreSQLのチューニング・メンテナンス](postgresql/psql-tune)

### データのバックアップ・リストア

- [シェルスクリプトで外部ストレージにバックアップを取るようにしてみる](postgresql/backup-bash)
- [Postgresqlのデータをバックアップする(docker compose)](postgresql/online-backup-postgresql)
- [Google Cloud Storageにバックアップする](postgresql/gcs-backup)

## メールサーバー

- [無償メールサーバーとしてZOHOmailを利用する](misskey/enable-mail-zoho)
- [メールサーバーの設定(iCloud編)](misskey/mail-smtp-icloud)

## オブジェクトストレージ

- [vultrでのオブジェクトストレージ設定](misskey/vultrでのオブジェクトストレージ設定)
- [オブジェクトストレージ(wasabi)の設定方法](misskey/object-storage-wasabi)

## 見た目のカスタマイズ

- [Faviconを変更する](misskey/change-favicon)
- [ログイン画面のテーマ色を変える](misskey/change-theme-color)

## Misskey Tips

- [スパム対策の手引き](misskey/spam-countermeasure)
- [絵文字を一括して編集する](misskey/絵文字を一括して編集する)
- [プッシュ通知を有効化する](misskey/enable-push-notification)
- [サマリープロキシを自前で準備する](misskey/summaly-proxy)
- [misskeyインスタンスを連合せずに建てる](misskey/disable-federation)
- [2000人までのインスタンス運営](misskey/2k-instance)

## Melisearch

- [Meilisearchを有効化する](search/enable-meilisearch)

## Linux系Tips

- [Raspberry Pi 4でMisskeyを立てる際のTips](linux/misskey-on-raspberry-pi-4-tips)
- [Ubuntu系OS本体におけるストレージ補正](linux/ubuntu-storage-ajust)
- [メモリーアロケーターをjemallocに変更する](linux/memoryKanri)
- [中古PCでMisskeyインスタンスを建てる際の備考](linux/remarks-setup-misskey-oldpc)
