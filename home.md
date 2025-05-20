---
title: Misskey鯖缶Wiki
description: 
published: true
date: 2025-05-20T03:15:39.846Z
tags: 
editor: markdown
dateCreated: 2023-03-25T09:18:07.348Z
---

# Misskey鯖缶Wikiへようこそ

Misskey鯖缶Wikiは、Misskeyサーバーの管理・運用のナレッジ情報を共有するための非公式なWikiです。
詳しくは[当Wikiについて](/about)へ。 

当Wikiと直接は関係ありませんが、他にも[サーバー管理者向けコミュニティ](/ref)があります。あわせてご確認ください。


> **:warning: 注意事項**
> なるべく正確な情報を記載するように心がけていますが、不正確な情報が含まれている可能性があります。
> 記載内容を利用する際は自己責任でお願いします。

# 脆弱性情報

- Misskeyの脆弱性情報は[Security Advisories · misskey-dev/misskey](https://github.com/misskey-dev/misskey/security/advisories)で確認できます。脆弱性対応のため、`Patched versions`のバージョンに更新することをおすすめします。
- 正式版リリース時と脆弱性公開時に投稿するbotを作成しました。RSSリーダーなどで購読できます。https://misskey.7ka.org/@misskey_bot  

# カテゴリ

ページの作成、編集方法については[当Wikiについて](/about)をご確認ください。

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

## Redis

- [FTTのキャッシュを消す](redis/delete-ftt-keys)

## Melisearch

- [Meilisearchを有効化する](search/enable-meilisearch)

## Linux系Tips

- [Raspberry Pi 4でMisskeyを立てる際のTips](linux/misskey-on-raspberry-pi-4-tips)
- [Ubuntu系OS本体におけるストレージ補正](linux/ubuntu-storage-ajust)
- [メモリーアロケーターをjemallocに変更する](linux/memoryKanri)
- [中古PCでMisskeyインスタンスを建てる際の備考](linux/remarks-setup-misskey-oldpc)

## 規約の雛形

利用規約やプライバシーポリシーなどドキュメント類の雛形を置くカテゴリです。

- [利用規約の雛形](terms/kiyaku)
- [プライバシーポリシーひな形](terms/policy)
