---
title: オブジェクトストレージ(wasabi)の設定方法
description: 
published: true
date: 2024-05-01T05:09:50.845Z
tags: wasabi, objectstorage
editor: markdown
dateCreated: 2023-04-04T13:48:56.154Z
---

# Wasabiの設定

## 前提条件

- Misskey v13.10.3
- Wasabi
- オブジェクトストレージのファイルはリダイレクトする設定が入っていること

## Nginxの設定

vhost.dの設定はこんな感じ。サブドメインを使いたかったがやり方わからなかった。

```
~/www/nginx/vhost.d$ cat sk.204.jp 

location /wsb/ {
  proxy_set_header Host s3.ap-northeast-1.wasabisys.com;
  proxy_set_header X-Real-IP $remote_addr;
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header X-Forwarded-Proto https;

  resolver 8.8.8.8 valid=100s;
  proxy_set_header Proxy "";
  proxy_pass https://s3.ap-northeast-1.wasabisys.com/<bucket-name>/;
  proxy_buffering off;
  proxy_redirect off;
  proxy_http_version 1.1;
  proxy_set_header Upgrade $http_upgrade;
  proxy_set_header Connection upgrade;

  tcp_nodelay on;
}

gzip on;
client_max_body_size 50m;
```



docker-compose.ymlはこんな感じ。

```
version: "3"
services:
  nginx-proxy:
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro
      - ./certs:/etc/nginx/certs:ro
      - ./conf.d:/etc/nginx/conf.d
      - ./vhost.d:/etc/nginx/vhost.d
      - ./logs:/var/log/nginx
      - ./html:/usr/share/nginx/html
    restart: always
    networks:
      - br-ext

  letsencrypt:
    image: jrcs/letsencrypt-nginx-proxy-companion
    container_name: letsencrypt
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./certs:/etc/nginx/certs:rw
      - ./vhost.d:/etc/nginx/vhost.d
      - ./conf.d:/etc/nginx/conf.d
      - ./html:/usr/share/nginx/html
    volumes_from:
      - nginx-proxy
    restart: always
    networks:
      - br-ext

networks:
  br-ext:
    external: true
```

## Misskeyの設定

| 項目 | 値 | 備考 |
| --- | --- | --- |
| オブジェクトストレージを使用 | オン | |
| Base URL | https://example.com/wsb | |
| Prefix | files | bucket/files 配下にファイルを保存する|
| Endpint | s3.ap-northeast-1.wasabisys.com/ | httpsいらない |
| Region | ap-northeast-1 | |
| Accsess key | アクセスキーを書く | |
| Secret key | シークレットきーを書く | |
| SSLを使用する | オン | 環境によるかも |
| Proxyを利用する | オフ | 環境によるかも |
| アップロード時にPublic-readを設定する | オフ | 環境によるかも |
| s3ForcePathStyle | オン | |

## 参考

- https://github.com/misskey-dev/misskey/issues/10408