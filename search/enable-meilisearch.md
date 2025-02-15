---
title: Meilisearchを有効化する
description: 
published: true
date: 2024-05-01T04:58:30.985Z
tags: 
editor: markdown
dateCreated: 2023-05-12T13:04:07.281Z
---

# MisskeyでMeilisearchを有効化する

## 前提条件

- Misskey v13.12.2
- Misskeyから提供されているdocker-compose.ymlを使った環境であること。
- 上記以外の環境の場合は、適宜読み替えてください。

## Meilisearchとは

- OSSな検索エンジンです。
- 利点: 高速で検索できます。MisskeyではスペースでAND検索もできます。
- 欠点: メモリ消費量が増えます。ストレージ使用量も増えます。(元データの10倍程度は見積もった方がいいとのこと)

## MisskeyでMeilisearchを使う

docker-compose.ymlの meilisearch を有効化(コメント外し)します。

default.ymlのmeilisearchの設定を有効化します。

apiKeyはuuidなど適当なキーを生成してください。

indexはMisskeyサーバーのドメインをお勧めします。(ドットは使えないため、ハイフンにします)

```
meilisearch:
  host: meilisearch
  port: 7700
  apiKey: ''
#  ssl: true
  index: 'example-com'
```

`.config/meilisearch.env` に先ほど生成したapiKeyを記載します。

```
MEILI_MASTER_KEY=
```

これで有効化は完了です。

## 日本語検索がおかしい時

- 検索キーワードに漢字のみを入れる(例えば「日本酒」「自信」など)と、中国語の投稿のみが結果に出ることがあります。次のdocker image( `getmeili/meilisearch:prototype-japanese-2` )を使うと解決できます。
- [Force japanese v1.1.0 by ManyTheFish · Pull Request #3588 · meilisearch/meilisearch](https://github.com/meilisearch/meilisearch/pull/3588)

## v13.12.1以前からの移行

> Meilisearchの設定にindexが必要になりました。値はMisskeyサーバーのホスト名にすることをお勧めします(アルファベット、ハイフン、アンダーバーのみ使用可能)。例: misskey-io 過去に作成されたnotesインデックスは、<index名>---notesにリネームが必要です。例: misskey-io---notes https://misskey-hub.net/docs/releases.html#_13-12-2


- [misskey-13.12.2-meilisearch.md](https://gist.github.com/Akkiesoft/d9b705b53de1181e6ce8d1288d49bc10)

## 関連Issue

- [[13.12.0 beta.5]Meilisearchで導入以前の過去のノートを検索できるようにマイグレーションしたい · Issue #10789 · misskey-dev/misskey](https://github.com/misskey-dev/misskey/issues/10789)
- [MeiliSearchのデータの保存期間の設定 · Issue #10835 · misskey-dev/misskey](https://github.com/misskey-dev/misskey/issues/10835)
