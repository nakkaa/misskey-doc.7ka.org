---
title: vultrでのオブジェクトストレージ設定
description: 
published: true
date: 2023-05-21T07:56:12.741Z
tags: vultr
editor: markdown
dateCreated: 2023-05-21T07:56:12.740Z
---

# vultrでのオブジェクトストレージ設定
ここでは3つの部門に分けて記載していきます


## 1.【vultrでの作業】
-Product＞Strage＞Object StorageでAddObjectStrageを押す
-Locationを選択（私は近いシンガポールを選択）
-Addを押す
-Readyになるまで待つ
-Readyになったら、作ったオブジェクトストレージを選択
-Bucketsタブを選択、CreateBucketを押す
-Bucketに名前を付ける

## 2.【コンソールでの作業】
-設定ファイルを開く
```# sudo vi /etc/nginx/conf.d/misskey.conf```


-設定文面上部の「proxy_cache_path /tmp/nginx_cache levels=1:2 keys_zone=cache1:16m max_size=1g inactive=720m use_temp_path=off;」の下に一行追記
```proxy_cache_path /var/cache/nginx/proxy_cache_images levels=1 keys_zone=images:2m max_size=20g inactive=90d;```

-「# Proxy to Node
    location / {
	～
       add_header X-Cache $upstream_cache_status;
       }」の下に下記を追記

※proxy_passを vultrのオブジェクトストレージのHostname/bucketsnameに変更
※proxy_set_header HostをvultrのオブジェクトストレージのHostnameに変更

```
location /storage/ {

      resolver 1.1.1.1 valid=100s;
      proxy_pass https://sjc1.vultrobjects.com/misskey/; # vultrのオブジェクトストレージのHostname/bucketsnameに変更
      proxy_buffering on;
      proxy_redirect off;
      proxy_http_version 1.1;
      tcp_nodelay on;

      expires max;
      proxy_set_header Host sjc1.vultrobjects.com; #proxy_set_header HostをvultrのオブジェクトストレージのHostnameに変更
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_hide_header etag;
      proxy_hide_header Set-Cookie;
      proxy_ignore_headers Set-Cookie;
      proxy_set_header cookie "";
      proxy_cache images;
      proxy_cache_valid 200 302 90d;
      proxy_cache_valid any 5m;
      proxy_ignore_headers Cache-Control Expires;
      proxy_cache_lock on;
      add_header X-Cache $upstream_cache_status;

  }
 ```

-変更内容に問題がないか確認
```# nginx -c /etc/nginx/nginx.conf```

-問題がなければ、nginxを再起動
```# sodo systemctl restart nginx```

-エラーが出ていないか確認
```# journalctl -f```

## 3.【misskeyでの作業】

コントロールパネル>オブジェクトストレージに移動
-オブジェクトストレージを使用:ON
-BaseURL：記入
-Bucket名：vultrで付けたBucket名入力
-Prefix：files
-Endpoint：vultrのHostname入力
![オブジェクトストレージ１.png](/beaco/オブジェクトストレージ１.png)
-Region:Hostnameの地名の部分のみ入力
	例）sgp1.vultrobjects.com（シンガポール）→sgp1
-Access key:vultrのアクセスキー部分を入力
-Secret key:vultrのシークレットキーを入力
-SSLを使用:ON
-Proxyを利用:ON
-アップロード時:ON
-s3ForcePathStyle:ON
![オブジェクトストレージ２.png](/beaco/オブジェクトストレージ２.png)

## 以上設定が完了したら、
-Misskeyのドライブに画像をアップロードする
-コントロールパネル＞ファイルでアップロードした画像を探しクリック
-上タブの＜/＞Raw dataをクリック
-url:"https://自身のMisskeyのURL/storage/files/～.png"になっていることを確認する
