---
title : 'Colab 解决中文字体问题'
date : 2024-08-14T03:36:29+08:00
lastmod: 2026-03-22T20:43:04+08:00
description : "from matplotlib.font_manager import fontManager"
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# Colab 解决中文字体问题

```py
!wget -O simhei.ttf "https://www.wfonts.com/download/data/2014/06/01/simhei/chinese.simhei.ttf"
import matplotlib as mpl
import matplotlib.pyplot as plt
from matplotlib.font_manager import fontManager
mpl.font_manager.fontManager.addfont('simhei.ttf')
mpl.rc('font', family='SimHei')
```

# 常用包（导包）

```py
import numpy as np
from scipy import stats
import math
```





