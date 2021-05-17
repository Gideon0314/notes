---
title: Python
created: '2021-04-21T01:06:48.916Z'
modified: '2021-05-17T14:28:35.005Z'
---

## Python


##### 计算 1+1/3+1/5+1/7+1/9+....1/99 的结果
```Python
def add(n):
    if n >= 1:
        val = 0
        for i in range(1, n+1):
            val += 1/(2*i-1)
        return val
```
