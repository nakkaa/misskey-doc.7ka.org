---
title: Faviconを変更する
description: 
published: true
date: 2024-05-01T05:06:04.015Z
tags: 
editor: markdown
dateCreated: 2023-04-01T10:44:14.199Z
---

# faviconの変更方法

## Docker Compose (docker image利用)

前提条件: misskey v13.10.x

docker-compose.ymlに以下を追記する。

```
- ./examplecom-icons:/misskey/packages/backend/assets:ro
```

カスタムアイコンを保存するディレクトリを作成する。

```
mkdir ./examplecom
cp -pr packages/backend/assets ./examplecom-icons/
sudo chown -hR 991.991 ./examplecom-icons
```

以下のアイコンを独自の画像に差し替える。

- apple-touch-icon.png
- favicon.ico
- favicon.png
- icons
  - 192.png
  - 512.png
- splash.png