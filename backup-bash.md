---
title: シェルスクリプトで外部ストレージにバックアップを取るようにしてみる
description: 要：rcloneで外部のストレージとの接続
published: true
date: 2023-06-18T09:54:09.719Z
tags: バックアップ
editor: markdown
dateCreated: 2023-06-18T09:54:09.719Z
---

# シェルスクリプトで外部ストレージにバックアップを取るようにしてみる
まずは下記のページ等を参考に外部のストレージとrcloneを接続してください。
https://reiji02.com/2022/08/18/%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92%E5%AE%9A%E6%9C%9F%E7%9A%84%E3%81%ABonedrive%E3%81%B8%E3%82%A2%E3%83%83%E3%83%97%E3%83%AD%E3%83%BC%E3%83%89/

接続したらバックアップフォルダを作ります。
`$ sudo mkdir -p /var/backup/`
`$ sudo chmod 777 /var/backup/`

作ったら次はBashファイルを作ります。
`$ sudo mkdir -p /var/backup_sh`
`$ sudo vi /var/backup_sh/pg_backup.sh`

ファイルの内容は、

---
`#!/bin/bash`

`# バックアップファイルを残しておく日数`
`PERIOD='+7'`
`# 日付`
`DATE=　date '+%Y%m%d-%H%M%S'`
`# バックアップ先ディレクトリ`
`SAVEPATH='/var/backup/'`
`# 先頭文字`
`PREFIX='postgres-'`
`# 拡張子`
`EXT='.sql'`
`# gzip拡張子`
`ZIPEXT='.gz'`
`# データーベース名`
`DBNAME='postgres'`
`# パスワードを入力`
`PASSWORD='（パスワードを記入）'`

`# バックアップ実行`
`pg_dumpall -f $SAVEPATH$PREFIX$DATE$EXT -U postgres`
`gzip $SAVEPATH$PREFIX$DATE$EXT`

`# 保存期間が過ぎたファイルの削除`
`find $SAVEPATH -type f -daystart -mtime $PERIOD -exec rm {} \;`

`# if文を使う`
`if [ -e $SAVEPATH$PREFIX$DATE$EXT$ZIPEXT ]; then`
`# ユーザーを切り替えてファイルを切り替え`
`su root << EOF`
`$PASSWORD`

`rclone copy $SAVEPATH$PREFIX$DATE$EXT$ZIPEXT （連携しているドライブに付けた名前）:（フォルダ）`
`EOF`
`fi`
---
参考サイト

PostgreSQLのバックアップをcronで定期的におこなう方法
http://vdeep.net/postgres-backup