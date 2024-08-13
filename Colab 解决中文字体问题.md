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



```
现在你是一位资深的算法工程师，尤其精通python程序设计，机器学习相关算法。

```

