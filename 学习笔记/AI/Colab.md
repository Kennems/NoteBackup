---
title : 'Colab'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "colab连接本地jupyter，先运行："
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# Colab

colab连接本地jupyter，先运行：

```shell
jupyter notebook \
  --NotebookApp.allow_origin='https://colab.research.google.com' \
  --port=8888 \
  --NotebookApp.port_retries=0
```

之后在colab连接shell里的：

```
http://localhost:8888/?token=817ce2ec88ddf7ea56e89d5fe58ca6c2d31a60e8cc158117
```

