# Coze.cn bot API

Authorization : pat_eb8qUVf2qULhLQa4dyEpSfAptwNlF70qYADaFzt0RSOy27BWZaJeIhVg6GY7XejQ

```bash
curl --location --request POST 'https://api.coze.cn/open_api/v2/chat' --header 'Authorization: Bearer pat_eb8qUVf2qULhLQa4dyEpSfAptwNlF70qYADaFzt0RSOy27BWZaJeIhVg6GY7XejQ' --header 'Content-Type: application/json' --header 'Accept: */*' --header 'Host: api.coze.cn' --header 'Connection: keep-alive' --data-raw '{
    "conversation_id": "123",
    "bot_id": "7398911470161117218",
    "user": "1",
    "query": "你好",
    "stream": true
}'
```

