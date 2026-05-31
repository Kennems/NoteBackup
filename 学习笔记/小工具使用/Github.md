---
title : 'Github'
date : 2025-03-16T10:30:13+08:00
lastmod: 2025-03-16T10:40:13+08:00
description : "Github工具使用" 
categories : ["Github"]
tags : ["Github"]
---

# Github



将仓库文件转换成`raw.githubusercontent.com`的形式

```py
#!/usr/bin/env python3
import sys
import re

def convert_url(url):
    """
    将形如 https://github.com/用户名/仓库名/raw/refs/heads/分支名/文件路径 的 URL
    转换为 https://raw.githubusercontent.com/用户名/仓库名/分支名/文件路径
    """
    pattern = r'^https://github\.com/([^/]+)/([^/]+)/raw/refs/heads/([^/]+)/(.*)$'
    m = re.match(pattern, url)
    if m:
        username, repo, branch, path = m.groups()
        new_url = f"https://raw.githubusercontent.com/{username}/{repo}/{branch}/{path}"
        return new_url
    else:
        return None

def main():
    if len(sys.argv) < 2:
        print("用法：python3 convert_url.py <github_url>")
        sys.exit(1)
    url = sys.argv[1]
    converted = convert_url(url)
    if converted:
        print("转换后的 URL 为：")
        print(converted)
    else:
        print("无法转换该 URL，请确保输入格式为：")
        print("https://github.com/用户名/仓库名/raw/refs/heads/分支名/文件路径")
        sys.exit(1)

if __name__ == '__main__':
    main()

```

