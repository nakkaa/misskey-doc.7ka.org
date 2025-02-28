---
title: FTT(Misskey Fanout Timeline)のキャッシュを消す
description: 
published: true
date: 2025-02-28T13:22:13.715Z
tags: 
editor: markdown
dateCreated: 2025-02-28T13:22:13.715Z
---

# FTTのキャッシュを消す

Misskeyには[Fan-out Timeline Technology (FTT) | Misskey Hub](https://misskey-hub.net/ja/docs/for-admin/features/ftt/)という負荷軽減機能がある。
この機能を無効化して再び有効化すると、キャッシュが残っているため無効化時点の古いタイムラインが表示されてしまう。(無効化から再有効化までの期間の投稿が表示されなくなる。)
時間経過で解消するとはいえ、不便ではあるため、キャッシュを消す方法をメモしておく。

## 前提条件

- Misskey 2025.2.1
- Redis 7.4.x
- FTTが無効であること

## 事前準備

Redisにパスワードを設定している場合は、以下のように環境変数を記載したファイルを作成し保存する。  
今回は.envとする。

```
export REDIS_PASSWORD=hogehoge
```

記載後、環境変数を読み込む。

```
source .env
```

## 削除実行

ターミナルで次のコマンドを実行する。
パスワードを指定していない場合は、 `-a` オプションは不要。

```
redis-cli -a "$REDIS_PASSWORD" -p 6379 --scan --pattern "*:list:localTimeline*" | xargs redis-cli -a "$REDIS_PASSWORD" -p 6379 DEL

redis-cli -a "$REDIS_PASSWORD" -p 6379 --scan --pattern "*:list:homeTimeline*" | xargs redis-cli -a "$REDIS_PASSWORD" -p 6379 DEL

redis-cli -a "$REDIS_PASSWORD" -p 6379 --scan --pattern "*:list:userTimeline*" | xargs redis-cli -a "$REDIS_PASSWORD" -p 6379 DEL

redis-cli -a "$REDIS_PASSWORD" -p 6379 --scan --pattern "*:list:channelTimeline*" | xargs redis-cli -a "$REDIS_PASSWORD" -p 6379 DEL

redis-cli -a "$REDIS_PASSWORD" -p 6379 --scan --pattern "*:list:userListTimeline*" | xargs redis-cli -a "$REDIS_PASSWORD" -p 6379 DEL
```

## 確認

`keys` コマンドで何も表示されなかったら削除が完了している。

```
redis-cli -a "$REDIS_PASSWORD" -p 6379  KEYS "*:list:localTimeline*"

redis-cli -a "$REDIS_PASSWORD" -p 6379  KEYS "*:list:homeTimeline*"

redis-cli -a "$REDIS_PASSWORD" -p 6379  KEYS "*:list:userTimeline*"

redis-cli -a "$REDIS_PASSWORD" -p 6379  KEYS "*:list:channelTimeline*"

redis-cli -a "$REDIS_PASSWORD" -p 6379  KEYS "*:list:userListTimeline*"
```

なお、antennaTimelineは消すと何も表示されなくなるため、投稿を残したい場合は消さないこと。

## 参考

[FTTを無効化する際にキャッシュを削除するように by penginn-net · Pull Request #15078 · misskey-dev/misskey](https://github.com/misskey-dev/misskey/pull/15078)