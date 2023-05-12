---
title: Meilisearchを有効化する
description: 
published: true
date: 2023-05-12T13:04:07.281Z
tags: 
editor: markdown
dateCreated: 2023-05-12T13:04:07.281Z
---

# Meilisearchを有効化する

## 前提条件

- Misskeyから提供されているdocker-compose.ymlを使った環境であること。
- 上記以外の環境の場合は、適宜読み替えてください。

## Meilisearchとは

- OSSな検索エンジンです。
- 利点: 高速で検索できます。MisskeyではスペースでAND検索もできます。
- 欠点: メモリ消費量が増えます。ストレージ使用量も増えます。(元データの10倍程度は見積もった方がいいとのこと)

## MisskeyでMeilisearchを使う

前提: 

docker-compose.ymlの meilisearch を有効化(コメント外し)します。

