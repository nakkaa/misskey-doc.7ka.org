---
title: Postgresqlのデータをバックアップする(docker compose)
description: 
published: true
date: 2023-04-12T13:06:15.220Z
tags: postgresql, backup
editor: markdown
dateCreated: 2023-04-12T13:06:15.219Z
---

# Postgresqlのデータをバックアップする

## 前提条件

docker composeでpsqlを動かしていること

## バックアップ

### s3cmdを使う

おじさんなのでs3cmdを使います。  
s3cmdを直接インストールしたかったのですが、Dockerを使えと言われたので以下のページを参考にします。

参考: [docker コンテナの s3cmd をホスト側からいい感じに使う - Qiita](https://qiita.com/tily/items/1c1965937e85d9e2db52)

```sh
$ cat Dockerfile 
FROM ubuntu
RUN apt-get update && apt-get install -y wget s3cmd

ENV DOCKERIZE_VERSION v0.3.0
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

ADD dot.s3cfg.tmpl /root/.s3cfg.tmpl
ENTRYPOINT ["dockerize", "-template", "/root/.s3cfg.tmpl:/root/.s3cfg", "s3cmd"]

WORKDIR /work
```

```sh
cat dot.s3cfg.tmpl 
[default]
signature_v2 = True

host_base = example.com
host_bucket = example.com/%(bucket)s

access_key = {{ .Env.ACCESS_KEY }}
secret_key = {{ .Env.SECRET_KEY }}
```

### バックアップスクリプトを書く

以下を参考にシェルスクリプトを書く。

参考: [docker-composeで運用しているMisskeyインスタンスをv13にアップグレードする - Qiita](https://qiita.com/nexryai/items/a1a4d07fce338796535b)

```sh
$ cat bk.sh 
#!/bin/bash

PRX=example.com

HOME=/home/user
DIR=${HOME}/www/${PRX}
BKDIR=/tmp/${PRX}
FILE=`date "+%Y%m%d-%H%M%S"`-${PRX}.dump

# backup postgresql
docker compose \
 -f ${DIR}/docker-compose.yml \
 exec -T db bash -c "pg_dump -Fc -U misskey -d misskey > /var/lib/postgresql/data/misskey-db.dump"

rm -rf ${BKDIR}
mkdir -p ${BKDIR}

mv ${DIR}/db/misskey-db.dump ${BKDIR}/${FILE}

# run s3cmd on docker
docker run \
  --rm -v ${BKDIR}:/work \
  -e ACCESS_KEY=*** \
  -e SECRET_KEY=*** \
  s3cmd put /work/${FILE} s3://${PRX}/

rm -rf ${BKDIR}
```

rootのcronに設定する。毎日3時5分に実行する。

```
5 3 * * * /home/user/bk.sh
```

## リストア手順

```
# DBを起動して初期化
docker compose up -d db

# PostgreSQLのコンテナID取得
docker ps

docker cp misskey_db.dump [先程調べたコンテナID]:/db.dump

docker compose exec -T db pg_restore -U misskey -d misskey /db.dump

docker compose down
```

