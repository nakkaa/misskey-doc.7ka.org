---
title: Google Cloud Storageにバックアップする
description: 
published: true
date: 2023-04-30T01:06:50.792Z
tags: 
editor: markdown
dateCreated: 2023-04-30T01:06:50.792Z
---


```
#!/bin/bash
export BOTO_CONFIG="/home/user/.boto"

DIR=/home/user/misskey
BKDIR=/home/user/backup
FILE=misskey_`date '+%Y-%m-%d'`

docker compose -f ${DIR}/docker-compose.yml down
tar -zvcf ${BKDIR}/${FILE}.tgz -C ${DIR}/ .
docker compose -f /home/user/misskey/docker-compose.yml up -d
/home/nakkaa/google-cloud-sdk/bin/gsutil cp ${BKDIR}/${FILE}.tgz gs://<bucket_name>/backup-mi/
```