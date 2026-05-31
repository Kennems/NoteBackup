---
title : 'Coze.cn bot API'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-05-04T12:24:02+08:00
description : "Authorization : REDACTED"
image : img/cat.jpg
draft : false
categories : ["Prompt"]
tags : ["学习笔记", "Prompt"]
---
# Coze.cn bot API

Authorization : REDACTED

```bash
curl --location --request POST 'https://api.coze.cn/open_api/v2/chat' --header 'Authorization: Bearer REDACTED' --header 'Content-Type: application/json' --header 'Accept: */*' --header 'Host: api.coze.cn' --header 'Connection: keep-alive' --data-raw '{
    "conversation_id": "123",
    "bot_id": "7398911470161117218",
    "user": "1",
    "query": "你好",
    "stream": true
}'
```

