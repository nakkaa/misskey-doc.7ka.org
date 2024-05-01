---
title: ログイン画面のテーマ色を変える
description: ログイン画面のテーマ色を緑から変える方法
published: true
date: 2024-05-01T05:12:42.410Z
tags: login, theme, color
editor: markdown
dateCreated: 2023-04-13T11:44:44.215Z
---

# ログイン画面のテーマ色を変更する

コントロールパネルの全般 -> テーマ のサーバーデフォルトのライトテーマ、サーバーデフォルトのダークテーマで変更できる。

idは `uuidgen` で適当に生成する。
accentを変えるとログイン画面の色が変わる。

ライトテーマ例

```
{
	id: '4eea646f-7afa-4645-83e9-83af0333cd39',
	name: 'ライト',
	author: 'nakkaa',
	desc: 'Default light theme',
	base: 'light',
	props: {
		bg: '#f9f9f9',
		accent: '#f8b500',
	},
}
```

ダークテーマ例

```
{
	id: '8050783a-7f63-445a-b270-36d0f6ba1679',
	name: 'ダーク',
	author: 'nakkaa',
	desc: 'Default light theme',
	base: 'dark',
	props: {
		accent: '#f8b500 ',
		bg: '#212526',
	},
}
```