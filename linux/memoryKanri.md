---
title: メモリーアロケーターをjemallocに変更する
description: メモリーアロケーターを変更して効率的にMisskeyのバックエンド処理を行う
published: true
date: 2024-05-01T04:48:32.212Z
tags: jemalloc
editor: markdown
dateCreated: 2023-08-16T17:59:36.502Z
---

# メモリアロケーターをjemallocに変更する

Misskeyは導入する際に特別な設定を要求しておらず、インストールの手順通りに導入していけば正常に動作します。
標準導入でこれと言って問題を感じていない方は関係ない話かもしれないですが、
僕自身使ってみてメモリの使用方法が行儀良くないな...と思ったのでアロケーターを変更して対策します。

## メモリアロケーターって何？

メモリアロケーターは、コンピューターのプログラム内での動的にメモリを確保、解放するための仕組みです。
プログラムは実行時に処理を行う為に処理に必要な量のメモリを要求します。この要求をそのまま聞いてしまうとコンピューターは必要な時に必要な分のメモリを確保しようとする処理に時間をかける傾向にあり、かなりの負荷をメモリに与えてしまいます。
こういった場合にアロケーターを使用するとプログラムの要求に応じて適切なメモリ領域を確保し、効率的にメモリを使いながら不要なメモリの浪費を避けることができます。

## jemallocって何？

jemalloc（ジェマロック）は、メモリーアロケーターの一つです。ジェマロックは通常のメモリアロケーターよりも比較的高速で、メモリフラグメンテーション（メモリが断片化してしまう事）を減少させることができます。また、複数のスレッドが同時にメモリを確保・解放する場合でも、競合状態を避けながら効率的に処理することができます。
UNIXでも主にCやC++みたいな環境で使ったりするんですけど、特徴を見るとMisskeyでも使えそうですね。

## jemallocをMisskeyで導入する

Misskeyに導入する場合、Dockerを使用した場合とBashスクリプトや手動で建てた場合で方法が異なります。MisskeyV12、13で違いはありませんがDockerとノーマルのそれぞれの手順を紹介しておきます。


　

### DockerでMisskeyを建てた場合

Dockerの場合Dockerfileの中身を弄って①jemallocを導入②jemallocを作動させるという挙動になるようにします。ここではV13を参考にしますがV12でも似たようなものなので適宜置き換えてください。

①MisskeyのDockerFile内の63行目から71行目辺りに、apt getでツールを導入している場所があります。

```
RUN apt-get update \
	&& apt-get install -y --no-install-recommends \ 
  ffmpeg tini curl  \　←
	&& corepack enable \
	&& groupadd -g "${GID}" misskey \
	&& useradd -l -u "${UID}" -g "${GID}" -m -d /misskey misskey \
	&& find / -type d -path /proc -prune -o -type f -perm /u+s -ignore_readdir_race -exec chmod u-s {} \; \
	&& find / -type d -path /proc -prune -o -type f -perm /g+s -ignore_readdir_race -exec chmod g-s {} \; \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists
```

矢印で示した部分の
```
	ffmpeg tini curl \
  ```
  を
  ```
  	ffmpeg tini curl libjemalloc2 \
  ```
  に修正します。
  追加後は以下のようになります。
  ```
  RUN apt-get update \
	&& apt-get install -y --no-install-recommends \
	ffmpeg tini curl libjemalloc2 \
	&& corepack enable \
	&& groupadd -g "${GID}" misskey \
	&& useradd -l -u "${UID}" -g "${GID}" -m -d /misskey misskey \
	&& find / -type d -path /proc -prune -o -type f -perm /u+s -ignore_readdir_race -exec chmod u-s {} \; \
	&& find / -type d -path /proc -prune -o -type f -perm /g+s -ignore_readdir_race -exec chmod g-s {} \; \
	&& apt-get clean \
	&& rm -rf /var/lib/apt/lists

```

②次に84行目から87行目に
```
ENV NODE_ENV=production
HEALTHCHECK --interval=5s --retries=20 CMD ["/bin/bash", "/misskey/healthcheck.sh"]
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["pnpm", "run", "migrateandstart"]
```
とありますので、ENVで始まる部分の真下に以下の行を追加してください。
```
ENV NODE_ENV=production
ENV LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2
HEALTHCHECK --interval=5s --retries=20 CMD ["/bin/bash", "/misskey/healthcheck.sh"]
ENTRYPOINT ["/usr/bin/tini", "--"]
CMD ["pnpm", "run", "migrateandstart"]
```

今追加した行によってlibjemalloc.so.2 ライブラリをプリロードすることが指定されています。これにより、Misskeyが通常のアロケータを使うのでなくjemallocメモリアロケーターを使用するように命令します。

この手順を踏まえてMisskeyを立ち上げればメモリアロケーターが採用された状態でMisskeyが動きます。

### BashScriot(かんたんインストール)、手動で導入した場合

今回はUbuntu22.04を例に紹介します。

手順としては①jemallocを導入する②Misskeyサービスの内部を書き換えるという内容です。

①まず、jemallocをインストールします。ターミナルで以下のコマンドを実行してください。
```
sudo apt install libjemalloc2
```
②次にsystemdの内容を弄ります。以下のコマンドでMisskeyサービスを修正できるようタイにします。
```
sudo systemctl edit misskey.service ←Bashの場合インスタンスドメイン
```
ここのmisskey.serviceというのはsystemdで登録したときの名称に置き換えてください。
Bashスクリプトで導入した場合にはここにはドメイン名が該当しますから「hoge.piyo」というドメインなら
```
sudo systemctl edit hoge.piyo
```
です。

入力後にEdit画面が現れ、既にコメントアウトされた何らかの文字が書かれていると思います。
その中から
```
#[Service]
#
```
と記載されている部分がありますので、ます、このハッシュタグを外し、機能を有効化します。
次に`[Service]`の真下に
```
Environment="LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2"
```
と追加します。追加したコードの仕組みは先述の通りです。
修正後はこのような設定になります。

```
[Service]
Environment="LD_PRELOAD=/usr/lib/x86_64-linux-gnu/libjemalloc.so.2"
```
設定後は保存を掛けてシステムを再起動します。

```
sudo systemctl restart misskey.service ←Bashの場合インスタンスドメイン
```
## 実際に導入してみての所感
実際にjemallocを使用してみるにメモリリークが顕著に減少したことが確認できましたので、効果覿面な印象です。
メモリ使用率も30％から16％程度にまで減少したので、メモリが逼迫した状態にある場合には導入するのもありだと思います。尚、これはオプショナルな設定なので変更しなかったからと言ってMisskey上に影響が出るものではありません。余力が残ってる場合は試してみる価値があると思います。

※当該記事は08/16/23に作成しております。バージョンやシステムの変更に付き内容が変わったり使用を控えたほうが良いとなる場合がございますので導入前に各自でご確認ください。
尚、メモの内容に不備、誤りがございましたら適宜修正して頂けますと幸いです。