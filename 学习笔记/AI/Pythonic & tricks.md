# Pythonic & tricks

```py
import os
print("cwd:", os.getcwd())
print("尝试读取 CSV 的目录:", os.path.abspath(os.path.dirname(INPUT_CSV)))
print("该目录下都有什么文件:", os.listdir(os.path.abspath(os.path.dirname(INPUT_CSV))))

```

